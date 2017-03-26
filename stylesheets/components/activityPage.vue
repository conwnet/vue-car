<style>
    .activity-coach, .activity-image {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 15px 15px 5px 15px;
    }

    .activity-coach .activity-coach-item-flex {
        flex-flow: column;
        display: flex;
    }

    .activity-coach .activity-coach-item-flex img {
        height: 40px;
        width: 40px;
    }

    .activity-coach .activity-coach-item-flex-column {
        -webkit-box-flex: 1;
        -webkit-flex: 1;
        flex: 1;
    }

    .activity-coach .activity-coach-item-flex-2 {
        flex-flow: column;
        display: flex;
        flex: 3;
        padding-left: 10px;;
    }

    .activity-image {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 0 60px;
        flex-direction: row;
        flex-wrap: wrap;
    }

    .activity-image .activity-image-item {
        width: 33%;
    }

    .activity-image .activity-image-item img {
        width: 100%;
        height: auto;
        padding: 5px;
    }

    .activity-banner {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 0 60px;
    }

    .activity-banner .activity-banner-time, .activity-banner .activity-banner-icon {
        width: 50%;
        padding: 0 5px 5px 5px;
    }

    .activity-banner .activity-banner-time {
        text-align: left;
        color: #c3c3c3;
        font-size: 1rem;
    }

    .activity-banner .activity-banner-icon {
        text-align: right;
    }

    .activity-banner .activity-banner-icon img {
        width: 15%;
        margin-left: 10px;
    }

    .bottom-solid {
        border: solid 1px #e6e6e6;
    }

    .coach-name-color {
        color: #566b96;
    }

    .activity-content-font {
        font-weight: bold;
    }
    .activity-top-img{
        position:relative;
    }
    .activity-top-img img{
        position: absolute;
        width: 6%;
        height:auto;
        top:30%;
        right: 5%;
    }
    .activity-banner-icon{
        color: #3ebefd;
        font-size: .5rem;
    }
    .activity-banner-icon img{
        vertical-align: baseline;
    }
</style>
<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="row page-title activity-top-img">活动<img v-if="isShowRelease" src="../images/release-activity-icon.png"  alt="" @click="gotoreleaseActivity"></div>
            <div v-for="actAll in coachListPage">
                <div class="row bottom-solid" v-for="item in actAll">
                    <div class="activity-coach" v-for="coachmsg in item.coach">
                        <div class="activity-coach-item-flex" @click="goCoachInfo(coachmsg.coachId)">
                            <img :src="get_image_path(coachmsg.coachPhoto)" alt="" class="img-responsive"></div>
                        <div class="activity-coach-item-flex-2">
                            <div class="activity-coach-item-flex-column coach-name-color" @click="goCoachInfo(coachmsg.coachId)">{{coachmsg.coachName}}</div>
                            <div class="activity-coach-item-flex-column activity-content-font">{{item.activityContent}}</div>
                        </div>
                    </div>
                    <div class="activity-image">
                        <div class="activity-image-item" v-for="img in item.activityImage"><img
                                :src="img_url+img.url" alt=""></div>
                    </div>
                    <div class="activity-banner">
                        <div class="activity-banner-time">{{item.releaseTime}}</div>

                        <div class="activity-banner-icon">
                            <div v-if="item.praiseTab===0">
                                <img src="../images/praise-count-icon1.png" @click="updateActivityById(item)"><img
                                    src="../images/share-icon.png" @click="jumpPage(item)">
                            </div>
                            <div v-else>
                                <img src="../images/praise-count-icon.png" @click="updateActivityById(item)">&nbsp;{{item.praiseTab}}<img
                                    src="../images/share-icon.png"  @click="jumpPage(item)"></div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-xs-12" style="height:40px;text-align: center;" @click="nextPage">
                    <div v-if="showmore" style="padding-top: 8px;">
                        <span>点击加载更多</span>
                    </div>
                    <div v-if="showPage">
                        <span>加载中...</span><img  src="../images/loadingPage.gif" style="height: 40px;width: 40px;">
                    </div>
                    <div v-if="showNull" style="padding-top: 8px;">
                        <span>无更多信息</span>
                    </div>
                </div>
            </div>
            <div id="dialogs">
                <div class="js_dialog" id="iosDialog2" style="display: none;">
                    <div class="weui-mask"></div>
                    <div class="weui-dialog">
                        <div class="weui-dialog__bd">{{dialog_info}}</div>
                        <div class="weui-dialog__ft">
                            <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                        </div>
                    </div>
                </div>
            </div>

            <div class="row" style="margin-bottom: 65px"></div>

            <component is='footerpage' :isSelected='4'></component>
        </div>
</template>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    module.exports = {
        data: function () {
            return {
                dialog_info:"",
                loading: true,
                coachActive: [],
                coachListPage: [],
                pageInt:1,
                isDataNull:false,
                showPage:false,
                showmore:true,
                showNull:false,
                isShowRelease : false,
                img_url:types.IMG_URL,
                get_image_path : types.GET_IMAGE_PATH
            }
        },
        created: function () {
            this.getAllActivity();
            types.DEL_ROUTER_PATH();//删除存储的路由跳转信息
            this.isShowRelease = true;
           // this.isShowRelease = storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS) == 1 ? true : false;//判断用户角色.1教练 2学员,学员不显示发布活动按钮
        },
        methods: {
            nextPage:function(){
                if(this.isDataNull){return;}
                this.pageInt++;
                this.showPage=true;
                this.showmore=false;
                this.getAllActivity();
            },
            gotoreleaseActivity : function(){
                this.$router.push({path:"/releaseActivityPage",name:"releaseActivityPage",params:{coachId:"111"}});
            },
            jumpPage:function(item){
                this.$router.push({path:"/coachSharePage",name:"coachSharePage",params:{coachId:item.coachId,activityId:item.activityId}});
            },
            getAllActivity:function(){
                var self = this;
                this.$http.post(types.HTTP_URL+'/activity/getAllActivity',{pageIndex:this.pageInt},
                {emulateJSON:true}).then(function(response){
                       if(response.data.status==1){
                            this.coachActive = response.data.list;
                            this.coachListPage.push(this.coachActive);

                            this.loading=false;
                            this.showPage=false;
                            this.showmore=true;
                            this.showNull=false;
                       }else{
                            this.isDataNull=true;
                            this.showPage=false;
                            this.showmore=false;
                            this.showNull=true;
                            this.loading=false;
                       }
                })
            },
            closeDialog : function(){
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            },
            updateActivityById:function(item){
                var pid="";
                if(storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)||storage.getJsonLocalStorageItem(types.CACHE_COACH_INFO)){
                        if(storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS)==1){
                            pid = storage.getJsonLocalStorageItem(types.CACHE_COACH_INFO).coachId;
                        }else{
                            pid = storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO).userId
                        }
                        this.$http.post(types.HTTP_URL+'/praiseTab/getPraiseCount',{activityId:item.activityId,pid:pid},
                        {emulateJSON:true}).then(function(response){
                            if(response.data.status===1){
                                   this.dialog_info="亲,您已经赞过了";
                                   var $iosDialog2 = $('#iosDialog2');
                                   $iosDialog2.fadeIn(200);
                            }else{
                                var praiseMsg={activityId:item.activityId,pid:pid};
                                this.$http.post(types.HTTP_URL+'/praiseTab/clickPraise',praiseMsg,
                                {emulateJSON:true}).then(function(response){
                                    if(response.data.status===1){
                                          item.praiseTab = item.praiseTab+1;
                                    }else{
                                          console.log("更新失败");
                                    }
                                })
                            }
                        })
                }else{
                     //storage.setJsonLocalStorageItem(types.ROUTER_FORM_PATH,{path:"activityPage"});
                     //var routerinfo=storage.getJsonLocalStorageItem(types.ROUTER_FORM_PATH);
                     this.routerPush();
                }
            },
            goCoachInfo:function(id){
                this.$router.push({path:"/coachInfoPage",name:"coachInfoPage",params: { paramId: id}});
            },
            routerPush:function(){
                    types.ADD_ROUTER_PATH("/activityPage","activityPage");
                    this.$router.push({path:"/register"});
            }
        },
        components: {
            'footerpage': require('../components/footerPage.vue'),
            'loading':require('../components/loading.vue')
        }
    }
</script>