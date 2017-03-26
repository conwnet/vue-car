<style>
    .release_active-content {
        columns: 5;
        resize: none;
        width: 100%;
        border: 0;
        padding: 10px 0;
    }

    .page-title .activity-but {
        position: absolute;
        top: 0px;
        right: 15px;
        text-decoration: none;
        color: white;
    }
    .re-img{
        padding-top: 9px;
        padding-left:5px;
        padding-right:5px;
        text-align:center;
    }
    .release-addimg{
        padding-top: 5px;
        padding-left: 5px;
        padding-right: 5px;
        text-align: center;
    }
</style>
<template>
    <div>
        <div class="row page-title" style="position:relative;">发表活动 <a class="activity-but" @click="releaseActivity">发布</a></div>

        <div class="row" style="padding-bottom: 15px;">
            <div class="col-xs-12">
                <textarea class=" weui-textarea" style="width:100%" rows="5" placeholder="输入活动内容"></textarea>
            </div>

        </div>
        <div class="row" style="padding-bottom: 15px;">
            <div class="col-xs-12">
                <div class="col-xs-4 re-img" v-for="img in imgAddress">
                    <img :src="img" style="width: 90px;height: 90px;">
                </div>
                <div class="col-xs-4 re-img">
                    <img style="width:90px;height:90px;" src="../images/add-image--icon.png" alt="" @click="wxImages">
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import * as types from "../store/mutation-types";
    module.exports = {
        data: function () {
            return {
                coachId:null,
                url :location.href.split('#')[0],
                jssdk : null,
                //imgAddress:null,
                weixinimgPath:null,
                img_url:types.IMG_URL,
                imgAddress:null,
                imgResult:[]
            }
        },
        created: function () {
            this.coachId = this.$route.params.coachId;
            this.getWxJsSdk();
        },
        methods: {
            releaseActivity:function(){
            alert(this.imgResult)
            },
            getWxJsSdk(){
                var self = this;
                this.$http.post(types.HTTP_URL+'/wxJsSdk/getWxJsSdk',{url :this.url },
                        {emulateJSON:true}).then(function(response){
                    self.jssdk = response.data.jssdk;
                    console.log("jssdk = " + JSON.stringify(self.jssdk));
                    wx.config({
                        debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
                        appId: self.jssdk.appId, // 必填，公众号的唯一标识
                        timestamp:self.jssdk.timestamp , // 必填，生成签名的时间戳
                        nonceStr: self.jssdk.noncestr, // 必填，生成签名的随机串
                        signature:self.jssdk.signature ,// 必填，签名，见附录1
                        jsApiList: ['startRecord','stopRecord','onVoiceRecordEnd','playVoice','pauseVoice','stopVoice','onVoicePlayEnd','uploadVoice','downloadVoice','chooseImage','previewImage','uploadImage','downloadImage'] // 必填，需要使用的JS接口列表，所有JS接口列表见附录2
                    });
                })
            },
            getLocalImg:function(localId){
                var imgPath="";
                wx.getLocalImgData({
                    localId: localId, // 图片的localID
                    success: function (res) {
                        imgPath=res.localData; // localData是图片的base64数据，可以用img标签显示
                    }
                });
                return imgPath;
            },
            wxImages(){
                var self = this;
                self.imgResult=[];
                wx.chooseImage({
                    count: 9, // 默认9
                    sizeType: ['original', 'compressed'], // 可以指定是原图还是压缩图，默认二者都有
                    sourceType: ['album', 'camera'], // 可以指定来源是相册还是相机，默认二者都有
                    success: function (res) {
                        self.imgAddress = res.localIds; // 返回选定照片的本地ID列表，localId可以作为img标签的src属性显示图
                        var imgList=res.localIds;
                         self.uploadImage(imgList);
                    }
                });
            },
            uploadImage:function(img){
                var self=this;
                var imgId = img.pop();
                    wx.uploadImage({
                    localId: imgId, // 需要上传的图片的本地ID，由chooseImage接口获得
                    isShowProgressTips: 0, // 默认为1，显示进度提示
                    success: function (res) {
                        var curserverId = res.serverId; // 返回图片的服务器端ID
                         self.$http.post(types.HTTP_URL+'/uploadWeiXin/downloadWeiXinMedia',{serverId:curserverId,mediatype:"jpg"},{emulateJSON:true}).then(function(response){
                                if(response.data.status==1){
                                    self.imgResult.push({url:response.data.file});
                                    if(img.length > 0){
				                            self.uploadImage(img);
			                        }
                                }else{
                                    console.log(response.data);
                                }
                         })
                    }
                });
            },
        },
        components: {}
    }
</script>