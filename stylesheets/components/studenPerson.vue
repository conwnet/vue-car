<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
        <div class="row student-share-row" style="background-color: #29a1f7;height: 90px;">
            <div class="col-xs-12">
                <div class="row logout-class" @click="logOut">
                    <span>登出</span>
                </div>
                <div class="row student-share-row" style="background-color: #29a1f7;">
                    <div class="col-xs-9 coach-share-row" style="text-align:center;display:flex;">
                        <div style="padding: 0px 5px 0px 0px;">
                            <img :src="student.userPhoto" class="userphoto">
                        </div>
                        <div style="padding: 7px 2px 0px 1px;color: #ffffff;">
                            <span>{{getdecodeUTF8(student.userName)}}</span>
                        </div>
                    </div>
                    <div class="col-xs-3 student-share-row" style="text-align:center;padding: 10px 0px 0px 13px;">
                        <div class="row" style="margin-right:10px;background: transparent;">
                            <img class="studen-user-icon" src="../images/service-icon.png">
                        </div>
                        <div class="row" style="margin-right:10px;font-size:12px;background: transparent;color: #ffffff;">
                            积分：{{getStuSorce(student.userScore)}}
                        </div>

                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12">
                <div class="weui-grids">
                    <a href="javascript:;" class="order-grid" style="border-bottom:none;">
                        <div class="weui-grid__icon">
                            <img src="../images/coupon-icon.png" alt="">
                        </div>
                        <p class="weui-grid__label2">优惠券</p>
                    </a>
                    <a href="javascript:;" class="order-grid">
                        <div class="weui-grid__icon">
                            <img src="../images/index-bgx.png" alt="">
                        </div>
                        <p class="weui-grid__label2">保过险</p>
                    </a>
                    <a href="javascript:;" class="order-grid">
                        <div class="weui-grid__icon">
                            <img src="../images/index-fqxc.png" alt="">
                        </div>
                        <p class="weui-grid__label2">分期学车</p>
                    </a>
                    <a href="javascript:;" class="order-grid">
                        <div class="weui-grid__icon">
                            <img src="../images/index-jfsc.png" alt="">
                        </div>
                        <p class="weui-grid__label2">积分商城</p>
                    </a>
                    <div class="order-grid" @click="goStudentOrder(student.userId)">
                        <!--<div class="weui-grid__icon">-->
                        <!--<img src="../../dist/images/order-icon.png" alt="">-->
                        <!--</div>-->
                        <div class="weui-grid__icon">
                            <img src="../images/order-icon.png" alt="">
                            <span class="student-weui-badge student-weui-badge_dot" style="position: absolute;top: 1.9em;right: 1.9em;" v-if="showDot===1">
                            </span>
                        </div>
                        <p class="weui-grid__label2">订单详情</p>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12">

            </div>
        </div>
        <div class="row" style="margin-top: 10px;">
            <div class="col-xs-4">我的关注</div>
            <div class="col-xs-6"></div>
            <div style="text-align: -webkit-center;color:#29a1f7;" @click="updatestudent">编辑</div>
        </div>
        <div class="row">
            <div class="col-xs-12">

            </div>
        </div>
        <div v-for="(item,index) in allConcernPage">
            <div v-if="item.coach===null">
                <div class="row" style="margin-bottom: 4px;padding-top: 5px;">
                    <div class="col-xs-12">
                        <div class="row school-share-row">
                            <div class="col-xs-3 school-share-row" style="text-align:center;" @click="gotoSchoolInfo(item.coachId)">
                                <img :src="img_url + item.drivingSchool.drivingSchoolPhoto" style="width: 60px;height: 60px;">
                            </div>
                            <div class="col-xs-7 school-share-row" @click="gotoSchoolInfo(item.coachId)">
                                <div class="row school-share-row">
                                    {{item.drivingSchool.drivingSchoolName}}
                                </div>
                                <div class="row school-share-row">
                                    <div v-for="stars in 5 ">
                                        <img v-if="stars <= item.drivingSchool.drivingSchoolScore" src="../images/lightstar.png"
                                             class="satrimg">
                                        <img v-else src="../images/graystar.png" class="satrimg">
                                    </div>
                                    <div style="display: flex;white-space: nowrap;">&nbsp;¥{{item.drivingSchool.drivingCheapPrice}}起</div>
                                </div>
                                <div class="row school-share-row" style="margin-top: 1px;">
                                    <div class="fontstyle1" v-for="screeningRelateds in item.drivingSchool.screeningRelateds">
                                        {{getScreenNameByCode(screeningRelateds.screeningId)}}
                                    </div>
                                </div>
                            </div>

                            <div class="col-xs-2 school-share-row">
                                <div class="schoolcancle" @click="schooldelete(item,index)"  v-if="showDelete">
                                    移除
                                </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-xs-3"></div>
                            <div class="col-xs-9" style="border: solid 1px #f1f1f1;margin-top: 6px;text-align: right;">
                                <span class="distance">{{getDistance(item.drivingSchool.distanc)}}</span>
                            </div>
                        </div>
                        <div class="row school-share-row">
                            <div class="col-xs-3 school-share-row"></div>
                            <div class="col-xs-7 school-share-row">
                                <div class="row school-share-row">
                                    <div v-if="item.drivingSchool.checkStatue===1">
                                        <img src="../images/learncard.png" class="school_photo"/>&nbsp;&nbsp;<span>约学车认证</span>
                                    </div>
                                    <div v-if="item.drivingSchool.studyDeposit===1">
                                        <img src="../images/learndeposit.png" class="school_photo"/>&nbsp;&nbsp;<span>学车担保金</span>
                                    </div>
                                </div>
                            </div>
                            <div class="col-xs-2"></div>
                        </div>
                    </div>
                </div>
            </div>
            <div v-else>
                <div class="row" style="margin-bottom: 4px;padding-top: 5px;">
                    <div class="col-xs-12">
                        <div class="row coach-share-row">
                            <div class="col-xs-3 coach-share-row" style="text-align:center;" @click="gotoCoachInfo(item.coachId)">
                                <img :src="get_image_path(item.coach.coachPhoto)" style="width: 60px;height: 60px;">
                            </div>
                            <div class="col-xs-7 coach-share-row" @click="gotoCoachInfo(item.coachId)">
                                <div class="row coach-share-row">
                                    {{item.coach.coachName}}
                                </div>
                                <div class="row coach-share-row">
                                    <div v-for="stars in 5 ">
                                        <img v-if="stars <= item.coach.coachScore" src="../images/lightstar.png"
                                             class="satrimg">
                                        <img v-else src="../images/graystar.png" class="satrimg">
                                    </div>
                                    <div style="display: flex;white-space: nowrap;">&nbsp;¥{{item.coach.coachCheapPrice}}起</div>
                                </div>
                                <div class="row coach-share-row" style="margin-top: 1px;">
                                    <div class="fontstyle1" v-for="screeningRelateds in item.coach.screeningRelateds">
                                        {{getScreenNameByCode(screeningRelateds.screeningId)}}
                                    </div>
                                </div>
                            </div>
                            <div class="col-xs-2 coach-share-row">
                                <div class="coachcancle" @click="coachdelete(item,index)" v-if="showDelete">
                                    移除
                                </div>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-xs-3"></div>
                            <div class="col-xs-9" style="border: solid 1px #f1f1f1;margin-top: 6px;text-align: right;">
                                <span class="distance">{{getDistance(item.coach.distanc)}}</span>
                            </div>
                        </div>
                        <div class="row coach-share-row">
                            <div class="col-xs-3 coach-share-row"></div>
                            <div class="col-xs-7 coach-share-row">
                                <div class="row coach-share-row">
                                    <div v-if="item.coach.checkStatue===1">
                                        <img src="../images/learncard.png" class="coach_photo"/>&nbsp;&nbsp;<span>约学车认证</span>
                                    </div>
                                    <div v-if="item.coach.studyDeposit===1">
                                        <img src="../images/learndeposit.png" class="coach_photo"/>&nbsp;&nbsp;<span>学车担保金</span>
                                    </div>
                                </div>
                            </div>
                            <div class="col-xs-2"></div>
                        </div>

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
            <div class="js_dialog" id="dialogs_comment" style="display: none;">
                <div class="weui-mask"></div>
                <div class="weui-dialog">
                    <div class="weui-dialog__bd">{{dialosText}}</div>
                    <div class="weui-dialog__ft">
                        <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">好的</a>
                    </div>
                </div>
            </div>
        </div>
        <component is="indextopdrivingschool"></component>
    </div>
</template>
<style>

    .logout-class{
        background: transparent;
        text-align: right;
        font-size: 9px;
        color: #ffffff;
        padding: 4px 8px 0px 0px;
    }
  .order-grid{
    position: relative;
    float: left;
    padding: 22px 10px 15px 0px;
    box-sizing: border-box;
    width:20%;
    text-align: center;
  }

    .student-header-line {
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
        height: 80px;
    }
     .flex-11 {
        -webkit-box-flex: 1;
        -webkit-flex: 1;
        flex: 1;
        margin: auto;
        color: white;
    }

    .flex-13 {
        -webkit-box-flex: 2;
        -webkit-flex: 2;
        flex: 2;
        margin: auto;
        position: relative;
    }
    .userphoto{
        border-radius: 50%;
        width: 60px;
        height: 60px;
    }
    .studen-user-icon{
        width: 22px;
    }

    .student-share-row{
        padding-left:5px;
        padding-right:5px;
    }

    .weui-grid__label2 {
        display: block;
        color: #999999;
        white-space: nowrap;
        text-overflow: ellipsis;
        font-size: 9px;
        white-space:nowrap;
        margin-top: 8px;
    }
    .student-weui-badge {
        display: inline-block;
        padding: .15em .4em;
        min-width: 8px;
        border-radius: 18px;
        background-color: #F43530;
        color: #FFFFFF;
        line-height: 1.2;
        text-align: center;
        font-size: 12px;
        vertical-align: middle;
    }
    .student-weui-badge_dot {
        padding: .4em;
        min-width: 0;
    }

    .satrimg{
        flex: 1;
        display: flex;
        float: left;
        width:19px;
        padding: 1px;
    }

    .coach-share-row{
        padding-left:5px;
        padding-right:5px;
    }

    .fontstyle1{
        height: 20px;
        border: 1px solid #00a2ea;
        padding: 0px 5px;
        border-radius: 3px;
        color: #00a2ea;
        font-size: 1rem;
        margin: -4px 4px 0px 0px;
        white-space: nowrap;
         flex: 1;
        display: flex;
        float: left;
        padding: 1px;
        margin-top: 2px;
    }

    .distance{
        position: absolute;
        margin: -29px 0px 0px -34px;
    }

    .coachcancle{
        color:#F43530;
        white-space: nowrap;
    }
    .coach_photo{
        width: 10%;
    }

    .school-share-row{
        padding-left:5px;
        padding-right:5px;
    }

    .schoolcancle{
        color:#F43530;
        white-space: nowrap;
    }

    .school_photo{
        width: 10%;
    }

</style>
<script>
import * as types from '../store/mutation-types'
import * as storage from '../store/localStorage'
    export default{
        data(){
            return{
                loading: true,
                studentConcern:null,
                updateshow:false,
                pageInt:1,
                showDot:0,
                screeningList:[],
                showDelete:false,
                isDataNull:false,
                allConcernPage:[],
                showPage:false,
                showmore:true,
                showNull:false,
                lngM:storage.getStrLocalStorageItem(types.LNG),
                latM:storage.getStrLocalStorageItem(types.LAT),
                img_url:types.IMG_URL,
                get_image_path : types.GET_IMAGE_PATH
                }
            },

        components:{
            'schoolTable':require('../components/schoolTable.vue'),
            'coachTable':require('../components/coachTable.vue'),
            'indextopdrivingschool':require('../components/indexTopDrivingSchool.vue'),
            'loading':require('../components/loading.vue')
        },
        created:function(){
            this.getUserInfo();
            this.getConcernByUserId();
            this.getAllScreening();
            this.getUserOrderNew();
             //this.lngM = ;
            //this.latM = ;
        },
        methods:{
           nextPage:function(){
                if(this.isDataNull){return;}
                this.pageInt++;
                this.showPage=true;
                this.showmore=false;
                this.showNull=false;
                this.getConcernByUserId();
            },
            //获取缓存数据
            getUserInfo:function(){
                this.student=storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO);
            },
            //跳转驾校详情
            gotoSchoolInfo:function(id){
                 this.$router.push({path:"/schoolInfoPage",name:"schoolInfoPage",params: { paramId: id}});
             },
             //跳转教练详情
             gotoCoachInfo:function(id){
                 this.$router.push({path:"/coachInfoPage",name:"coachInfoPage",params: { paramId:id}});
             },
             //取消关注显隐
            updatestudent:function(){
               if(this.showDelete===false){
                    this.showDelete=true;
               }else{
                    this.showDelete=false;
               }
            },
            //获得学员关注的驾校和教练
            getConcernByUserId:function(){
                this.$http.post(types.HTTP_URL+'/concern/getAllConcernByUserId',
                {pageIndex:this.pageInt,userId:this.student.userId,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                            console.log( response.data.list);
                            var allConcern = response.data.list;
                            this.allConcernPage=this.allConcernPage.concat(allConcern);
                            this.showPage=false;
                            this.showmore=true;
                            this.showNull=false;
                            this.loading=false;
                        }else{
                            this.isDataNull=true;
                            this.showPage=false;
                            this.showmore=false;
                            this.showNull = true;
                            this.loading=false;
                        }
                 })
             },
             //查询所有筛选项
             getAllScreening: function () {
                this.$http.post(types.HTTP_URL+'/screening/getAllScreening',{emulateJSON:true}).then(function(response){
                            this.screeningList = response.data.screeningList;
                 })
            },
            //对筛选id进行转码
            getScreenNameByCode:function(code){
                for(var i=0;i<this.screeningList.length;i++)
                {
                    if(code == this.screeningList[i].id){
                        return this.screeningList[i].name;
                    }
                }
            },
            //对教练取消关注
            coachdelete:function(coach,index){
                var coachBuzz = coach.coach.buzzCount-1;
                var coachConcern = coach.coach.concernCount-1;
                 this.$http.post(types.HTTP_URL+'/coach/cancelConcernCoach',{buzzCount:coachBuzz,concernCount:coachConcern,coachId:coach.coachId,userId:this.student.userId},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.state==1){
                            this.allConcernPage.splice(index, 1);
                        }else{
                            this.setDialos("服务器忙，请稍后重试。");
                        }
                 })
             },
             //对驾校取消关注
              schooldelete:function(school,index){
               var schoolBuzz = school.drivingSchool.buzzCount-1;
                var schoolConcern = school.drivingSchool.concernCount-1;
                 this.$http.post(types.HTTP_URL+'/drivingSchool/cancelConcernDrivingSchool',{buzzCount:schoolBuzz,concernCount:schoolConcern,drivingSchoolId:school.drivingSchoolId,userId:this.student.userId},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.state==1){
                            this.allConcernPage.splice(index, 1);
                        }else{
                            this.setDialos("服务器忙，请稍后重试。");
                        }
                 })
             },
             //查询是否有新订单
             getUserOrderNew:function(){
                this.$http.post(types.HTTP_URL+'/orderTab/getAllOrderByUserId',{pageIndex:this.pageInt,userId:this.student.userId},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                            var userOrderList = response.data.list;
                            for(var i=0;i<userOrderList.length;i++){
                                if(userOrderList[i].orderNew==0){
                                    this.showDot=1;
                                }else{

                                }
                            }
                        }else{
                             console.log("没有新订单");
                        }
                 })
             },
             goStudentOrder:function(id){
                this.$router.push({path:"/orderTabForStudent",name:"orderTabForStudent",params: { userId: id}});
             },
             logOut:function(){
                storage.cleanLocalStorage();
                this.$router.push({path:"/"});
             },
             closeDialog: function () {
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
             },
             setDialos : function(text){
                this.dialosText = text;
                var $dialogs_comment = $('#dialogs_comment');
                $dialogs_comment.fadeIn(200);
            },
            getDistance:function(distance){
                if(distance < 1000){
                    return distance+"m";
                }
                else if(distance > 1000){
                    return (Math.round(distance/100)/10).toFixed(1) + "km"
                }
            },
            getdecodeUTF8:function(arr){
                  return decodeURI(arr,"utf-8");
             },
             getStuSorce:function(sorce){
                  if(sorce==null){
                    return sorce=0;
                  }else{
                    return sorce;
                  }
             },
        }
    }
</script>
