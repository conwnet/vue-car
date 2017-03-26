<style>
    .evaluate-title ,.border-line{
        border-bottom: solid 1px #f0f0f0;
    }
    .evaluate-title{
        position: relative;
    }
    /*.evaluate-title .col-xs-12 img {*/
        /*position: absolute;*/
        /*right: 15px;*/
        /*top: 4px;*/
        /*width: 5%;*/
        /*transform: rotate(-90deg);*/
        /*-ms-transform: rotate(-90deg);*/
        /*-moz-transform: rotate(-90deg);*/
        /*-webkit-transform: rotate(-90deg);*/
        /*-o-transform: rotate(-90deg);*/
    /*}*/
    .evaluate-title .col-xs-12 span {
        float:right;
    }
    .evaluate-user-img img {
        width: 50px;
        height: 50px;
        border-radius: 50%;
    }
    .evaluate-user-fraction img{
        width: 5%;
        height: auto;
    }
    .evaluate-user-estimate{
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
    }
    .evaluate-user-estimate div{
        width: 18%;
        color: #02a0e9;
        border: solid 1px #02a0e9;
        border-radius: 7px;
        text-align: center;
        overflow: hidden;
        white-space: nowrap;
        font-size: .1rem;
        margin-right: 5px;
    }
    .evaluate-user-content{
        margin-top:10px;
    }
</style>
<template>
    <div>
        <div class="row evaluate-title">
            <div class="col-xs-12">网友点评({{count}})<span @click="gotoReleaseEvaluate" v-if="role_type!=2">发表评论</span></div>
        </div>
            <div class="row border-line" v-for="item in ealuatev">
                <div class="col-xs-12" style="text-align: right;color: #dbdbdb;">{{getSubTime(item.currentTime,item.commentTime)}}</div>
                <div class="col-xs-2 evaluate-user-img"><img v-if="item.student" :src="img_url+item.student.userPhoto" alt=""></div>
                <div class="col-xs-10">
                    <div class="evaluate-user-name" v-if="item.student">{{getStrDncode(item.student.userName)}}</div>
                    <div class="evaluate-user-fraction">打分:
                        <img v-for="fraction in item.commentScore" src="../images/lightstar.png" alt=""><img
                                v-for="fraction in 5-item.commentScore" src="../images/graystar.png" alt=""></div>
                </div>
                <div class="col-xs-10 col-xs-offset-2 evaluate-user-content">
                    <p>{{item.commentContent}}</p>
                </div>
            </div>
    </div>
</template>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    module.exports = {
        data: function () {
            return {
                role_type:null,
                img_url:types.IMG_URL
            }
        },
        props: ["ealuatev","pid","name","type","count"],
        created:function(){
            this.isShow();
            console.log(this.ealuatev)
        },
        methods : {
            isShow:function(){
                if(storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS) == 1)
                {
                    this.role_type=2;
                }
            },
            routerPush:function(){
                    if(this.type==1){
                       types.ADD_ROUTER_PATH("/schoolInfoPage","schoolInfoPage",this.pid);
                       this.$router.push({path:"/register"});
                    }else if(this.type==2){
                        types.ADD_ROUTER_PATH("/coachInfoPage","coachInfoPage",this.pid);
                        this.$router.push({path:"/register"});
                    }
            },
            gotoReleaseEvaluate : function (){
                if(!storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS)){
                    this.routerPush();
                }else{
                    this.$router.push({ name: 'releaseEvaluate', params: { pid:this.pid,name:this.name,type:this.type}});
                }
            },
            getStrDncode:function(arr){
            return decodeURI(arr,"utf-8")
            },
            getSubTime:function(a,b){
                var mss=a-b;
                var days=parseInt(mss / (1000 * 60 * 60 * 24));
                if(days>=1){
                    return days+"天前"
                }
                var hours=parseInt((mss / (1000 * 60 * 60 )))
                if(hours>=1){
                    return hours+"小时前"
                }
                var min=parseInt((mss/(1000*60)))
                if(min>=1){
                    return  min+"分钟前"
                }
                var sec=parseInt(mss/1000)
                if(sec>=1)
                {
                return sec+"秒前"
                }
            }
        }
    }
</script>