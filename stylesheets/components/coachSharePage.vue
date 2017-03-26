<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="header-line">
                <div class="flex-3" style="color:#ffffff;">
                    约学车一站式学车平台
                </div>
                <div class="flex-1" style="margin-right: 20px;" @click="coachconcern">
                    <img src="../images/coach-share-concern.png" style="width: 60px;">
                </div>
            </div>

            <div id="dialogs">
                <div class="js_dialog" id="CoachShareDialog" style="display: none;">
                    <div class="weui-mask"></div>
                    <div class="weui-dialog">
                        <div class="weui-dialog__bd"><img src="../images/jej-QR-code.png" class="qr-code"></div>
                        <div class="weui-dialog__ft">
                            <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="coachcloseDialog">返回</a>
                        </div>
                    </div>
                </div>
            </div>
            <component is='indexslider' :sliderType="4"></component>
            <div class="row" style="margin-bottom: 4px;padding-top: 5px;">
                <div class="col-xs-12">
                    <div class="row coach-share-row" @click="goCoachInfo(coachList.coachId)">
                        <div class="col-xs-3 coach-share-row" style="text-align:center;">
                            <img :src="get_image_path(coachList.coachPhoto)" style="width: 60px;">
                        </div>
                        <div class="col-xs-7 coach-share-row">
                            <div class="row coach-share-row">
                                {{coachList.coachName}}
                            </div>
                            <div class="row coach-share-row">
                                <div v-for="stars in 5 ">
                                    <img v-if="stars <= coachList.coachScore" src="../images/lightstar.png"
                                         class="satrimg">
                                    <img v-else src="../images/graystar.png" class="satrimg">
                                </div>
                                <div style="display: flex;white-space: nowrap;">&nbsp;¥{{coachList.coachCheapPrice}}起</div>
                            </div>
                            <div class="row coach-share-row" style="margin-top: 1px;">
                                <div class="fontstyle1" v-for="screeningRelateds in coachList.screeningRelateds">
                                    {{getScreenNameByCode(screeningRelateds.screeningId)}}
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="row">
                        <div class="col-xs-3"></div>
                        <div class="col-xs-9" style="border: solid 1px #f1f1f1;margin-top: 6px;text-align: right;">
                            <!--<span class="distance">{{coach.distance}}m</span>-->
                        </div>
                    </div>
                    <div class="row coach-share-row">
                        <div class="col-xs-3 coach-share-row"></div>
                        <div class="col-xs-7 coach-share-row">
                            <div class="row coach-share-row">
                                <div v-if="coachList.checkStatue===1">
                                    <img src="../images/learncard.png" class="coach_photo"/>&nbsp;&nbsp;<span>约学车认证</span>
                                </div>
                                <div v-if="coachList.studyDeposit===1">
                                    <img src="../images/learndeposit.png" class="coach_photo"/>&nbsp;&nbsp;<span>学车担保金</span>
                                </div>
                            </div>
                        </div>
                        <div class="col-xs-2"></div>
                    </div>

                </div>
            </div>
            <component is='coachShareContent' :caochName="coachList.coachName" :coachId="coachList.coachId" :activityId="this.activityId"></component>
        </div>
</template>
<style>
    .coachshare-header-line {
        height:44px;
       text-align:center;
       background-color: #29a1f7;
       color:#ffffff;
    }

     .header-line {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        position: fixed;
        z-index: 500;
        width: 100%;
        top: 0;
        left: 0;
        background-color: #29a1f7;
        text-align: center;
        height: 55px;
    }
     .header-line.flex-1 {
        -webkit-box-flex: 1;
        -webkit-flex: 1;
        flex: 1;
        margin: auto;
        color: white;
    }

    .header-line.flex-3 {
        -webkit-box-flex: 3;
        -webkit-flex: 3;
        flex: 3;
        margin: auto;
        position: relative;
        color: white;
        font-family: initial;
        font-size: 20px;
        text-align: left;
        margin-left: 25px;
    }
    .qr-code{
        width:170px;
        height:170px;
    }
</style>
<script>
import * as types from "../store/mutation-types"
import * as storage from '../store/localStorage'
    export default{
        data(){
            return{
                loading: true,
                coachId:null,
                activityId:null,
                coachList:null,
                screeningList:[],
                img_url:types.IMG_URL,
                get_image_path : types.GET_IMAGE_PATH,
                lngM:storage.getStrLocalStorageItem(types.LNG),
                latM:storage.getStrLocalStorageItem(types.LAT),
            }
        },
        created:function(){
            this.coachId = this.$route.params.coachId;
            this.activityId = this.$route.params.activityId;
            this.getCoachByCoachId();
        },
        components:{
            'indexslider':require('../components/indexslider.vue'),
            'coachTable':require('../components/coachTable.vue'),
            'coachShareContent':require('../components/coachShareContent.vue'),
            'loading':require('../components/loading.vue')
        },
        methods:{
            coachcloseDialog: function () {
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            },
            coachconcern:function(){
                var $iosDialog2 = $('#CoachShareDialog');
                $iosDialog2.fadeIn(200);
            },
             getCoachByCoachId:function(){
                this.$http.post(types.HTTP_URL+'/coach/getCoachByCoachId',
                {coachId:this.coachId,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                            this.coachList = response.data.data;
                            this.getAllScreening();
                            this.loading=false;
                            //this.upCoachBuzzByCoachId();
                        }else{

                        }
                 })
             },
             getAllScreening: function () {
                this.$http.post(types.HTTP_URL+'/screening/getAllScreening',{emulateJSON:true}).then(function(response){
                            this.screeningList = response.data.screeningList;
                            this.loading=false;
                 })
            },
            getScreenNameByCode:function(code){
                for(var i=0;i<this.screeningList.length;i++)
                {
                    if(code == this.screeningList[i].id){
                        return this.screeningList[i].name;
                    }
                }
            },
            //upCoachBuzzByCoachId:function(){
            //     var buzzCountNew = this.coachList.buzzCount+1;
            //     var coachBuzzupdate = {buzzCount:buzzCountNew,coachId:this.coachId};
            //     this.$http.post(types.HTTP_URL+'/coach/upCoachBuzzById',coachBuzzupdate,
            //        {emulateJSON:true}).then(function(response){
            //    })
            //}
            goCoachInfo:function(id){
                this.$router.push({path:"/coachInfoPage",name:"coachInfoPage",params: { paramId: id}});
            }
        }
    }
</script>
