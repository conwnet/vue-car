<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading"></div>
        <div class="row coachPerson-sharrow" style="background-color:#00a0e9;">
            <div class="col-xs-12">
                <div class="row coachPerson-sharrow" style="background: transparent;">
                    <div class="col-xs-2" style="text-align: center; padding-top: 21px; padding-left: 5px;padding-right: 5px;">
                        <img :src="get_image_path(coach.coachPhoto)" class="coachPerson-photo">
                    </div>
                    <div class="col-xs-2 coachPerson-name">
                        {{coach.coachName}}<p style="font-size: 9px;">{{coach.drivingSchoolName}}</p>
                    </div>
                    <div class="col-xs-8 coachPerson-buzz">
                        <div style="text-align: right;padding-top: 4px;" @click="logOut"><span>登出</span></div>
                        <div style="padding-top: 20px;">
                            <span>人气：{{coach.buzzCount}}&nbsp;关注：{{coach.concernCount}}</span>
                        </div>
                    </div>
                </div>
                <div class="row" style="margin-top: 13px;background: transparent;">
                    <div class="col-xs-4 coachPerson-iconshare" style="border-right: solid 1px #dfdfdf;" @click="upCoachMsg()">
                        <div><img src="../images/coach-person-pop.png" class="coachPerson-icon"></div>
                        <div class="coachPerson-iconfont">个人资料</div>
                    </div>
                    <div class="col-xs-4 coachPerson-iconshare" style="border-right: solid 1px #dfdfdf;" @click="seletOrderByCoach()">
                        <div><img src="../images/coach-person-heart.png" class="coachPerson-icon"></div>
                        <div class="coachPerson-iconfont">关注</div>
                    </div>
                    <div class="col-xs-4 coachPerson-iconshare" @click="servicePhone()">
                        <div>
                            <img src="../images/coach-person-service.png" class="coachPerson-icon">
                        </div>
                        <div class="coachPerson-iconfont">客服投诉</div>
                    </div>
                </div>
                <div class="row" style="background: transparent;">
                    <div style="height: 15px;"></div>
                </div>
            </div>
        </div>
        <div class="row coachPerson-applyiconsharrow">
            <div class="col-xs-12">
                <div class="row" style="margin-top: 20px;text-align: center;">
                    <div class="col-xs-4">
                        <div><img src="../images/coach-person-learn.png" alt="" class="coachPerson-applyicon"></div>
                        <div class="coachPerson-applyfont">申请约学车认证</div>
                    </div>
                    <div class="col-xs-4">
                        <div><img src="../images/coach-person-deposit.png" alt="" class="coachPerson-applyicon"></div>
                        <div class="coachPerson-applyfont">申请担保金认证</div>
                    </div>
                    <div class="col-xs-4">
                        <div><img src="../images/coach-person-pay.png" alt="" class="coachPerson-applyicon"></div>
                        <div class="coachPerson-applyfont">申请先学后付</div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row" style="margin-top: 5px;">
            <div class="col-xs-12" style="border:solid 2px #f1f1f1;">
            </div>
        </div>
        <div class="row">
            <div class="col-xs-8 coachPerson-publish">点击发布活动/优惠信息</div>
            <div class="col-xs-2"></div>
            <div class="coachPerson-publishbtn" @click="releaseAvtivity">发布</div>
        </div>
        <div class="row">
            <div class="col-xs-12" style="border:solid 2px #dfdfdf;margin-top: 5px;">

            </div>
        </div>

        <!--<div id="dialogs">-->
            <!--<div class="js_dialog" id="CoachShareDialog" style="display: none;">-->
                <!--<div class="weui-mask"></div>-->
                <!--<div class="weui-dialog">-->
                    <!--<div class="weui-dialog__bd">是否拨打13655555555</div>-->
                    <!--<div class="weui-dialog__ft">-->
                        <!--<div style="width: 50%;">-->
                            <!--<a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="coachcloseDialog">取消</a>-->
                        <!--</div>-->
                        <!--<div style="width: 50%;border-left-style: solid;border-left-color: #f1f1f1;">-->
                            <!--<a class="weui-dialog__btn weui-dialog__btn_primary">确认</a>-->
                        <!--</div>-->
                    <!--</div>-->
                <!--</div>-->
            <!--</div>-->
        <!--</div>-->
        <div class="row">
            <div class="col-xs-12">
                我的活动
            </div>
        </div>
        <div>
            <component is='activePageShare' :coachImg='coach.coachPhoto' :coachName='coach.coachName' :drivingName='coach.drivingSchoolName' :coachId='coach.coachId'></component>
        </div>
        <div>
            <component is='footerpage' :isSelected='4'></component>
        </div>
    </div>
</template>
<style>

     .coachPerson-photo{
        border-radius:50%;
        width: 60px;
        height: 60px;
        border: solid 1px white;
        text-align:center;
    }

    .coachPerson-name{
        margin-top: 31px;
        white-space: nowrap;
        color: white;
        padding-left: 18px;
    }
    .coachPerson-buzz{
        white-space: nowrap;
        font-size: 9px;
        padding: 0px 0px 0px 36px;
        text-align:center;
        color: #ffffff;
    }

    .coachPerson-sharrow{
        padding-left: 5px;
        padding-right: 5px;
    }

    .coachPerson-icon{
            width: 20;
            height: 20px;
    }

    .coachPerson-iconshare{
        text-align: center;
    }

    .coachPerson-iconfont{
        color:#ffffff;
    }

    .coachPerson-applyicon{
         width: 30px;
    }
    .coachPerson-applyfont{
         font-size: 5px;
         white-space: nowrap;
    }

    .coachPerson-applyiconsharrow{
        padding-left: 10px;
        padding-right: 10px;
    }

    .coachPerson-publish{
        color: #929292;
    }

    .coachPerson-publishbtn{
        color: #29a1f7;
        text-align: -webkit-center;
    }


</style>
<script>
import * as types from '../store/mutation-types'
import * as storage from '../store/localStorage'
    export default{
        data(){
            return{
                coach:null,
                loading: true,
                img_url:types.IMG_URL,
                get_image_path : types.GET_IMAGE_PATH
            }
        },
        created:function(){
            this.getCoachInfo();
        },
        components: {
            'footerpage': require('../components/footerPage.vue'),
            'activePageShare': require('../components/activePageShare.vue'),
            'loading':require('../components/loading.vue')
        },
        methods:{
            coachcloseDialog: function () {
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            },
            servicePhone:function(){
                var $iosDialog2 = $('#CoachShareDialog');
                $iosDialog2.fadeIn(200);
            },
            getCoachInfo:function(){
                this.coach=storage.getJsonLocalStorageItem(types.CACHE_COACH_INFO);
                this.loading=false;
            },
            seletOrderByCoach:function(){
                this.$router.push({path:"/concernPage",name:"concernPage",params:{coachId:this.coach.coachId}});
            },
            releaseAvtivity:function(){
                this.$router.push({path:"/releaseActivityPage",name:"releaseActivityPage",params:{coachId:this.coach.coachId}});
            },
            upCoachMsg:function(){
                this.$router.push({path:"/registerCoach.vue",name:"registerCoach"});
            },
            logOut:function(){
                storage.cleanLocalStorage();
                this.$router.push({path:"/"});
            },
        }
    }
</script>
