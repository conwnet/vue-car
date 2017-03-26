<style>
    .page-title {
        height: 44px;
        color: white;
        background-color: #29a1f7;
        line-height: 44px;
        text-align: center;
    }

    .star-icon {
        width: 7%;
        height: auto;
    }

    .margin-top-twenty {
        padding-top: 20px;
    }

    .margin-top-five {
        padding-top: 5px;
    }

    .img-flex {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 5px 10px;
    }

    .img-flex .img-flex-item {
        width: 20%;
        padding: 0 5px;
    }

    .img-flex img {
        width: 60px;
    }

    .more-image {
        /*background-color: #f0f0f0;*/
        /*color: #b6b6b6;*/
        /*text-align: center;*/
        /*padding: 10px !important;*/
    }

    .estiomate-flex {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        padding: 0 10px;
    }

    .estiomate-flex .estiomate-flex-item {
        width: 17%;
        padding: 0 5px;
    }

    .estiomate-flex .estiomate-flex-item div {
        color: #02a0e9;
        border: solid 1px #02a0e9;
        border-radius: 7px;
        text-align: center;
        overflow: hidden;
        white-space: nowrap;
    }

    .learn-icon {
        width: 6%;
        height: auto;
    }

    .learn-item span {
        margin-left: 5px;
        font-size: 12px;
    }
</style>
<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="page-title row">详情页</div>
            <div class="row margin-top-twenty">
                <div class="col-xs-9" style="text-align: left;">
                    <span>{{drivingSchool.drivingSchoolName}}</span>
                </div>
                <div class="col-xs-3" style="text-align: right;">
                    <a @click="userConcern" v-if="!roleType">+关注</a>
                    <a v-if="roleType  && !isCoach">已关注</a>
                </div>
            </div>
            <div class="row margin-top-five">
                <div class="col-xs-6">
                    <img v-for="start in drivingSchool.drivingSchoolScore" src="../images/lightstar.png" alt="" class="star-icon"><img
                        v-for="start in 5-drivingSchool.drivingSchoolScore" src="../images/graystar.png" alt="" class="star-icon">
                </div>
                <div class="col-xs-6"></div>
            </div>
            <div class="row margin-top-five img-flex" v-if="drivingSchool.drivingSpaceImages.length > 5">
                <div class="img-flex-item" v-for="item in 4">
                    <img src="../images/car-img.png" alt="" class="img-responsive">
                </div>
                <!--<div class="img-flex-item more-image">-->
                <!--更多<br>图片-->
                <!--</div>-->
                <div class="img-flex-item more-image">
                    <img src="../images/more-image-icon.png" alt="" class="img-responsive">
                </div>
            </div>
            <!--图片没超过5张 -->
            <div class="row margin-top-five img-flex" v-else>
                <div class="img-flex-item" v-for="item in drivingSchool.drivingSpaceImages">
                    <img src="../images/car-img.png" alt="" class="img-responsive">
                </div>
            </div>
            <div class="row fs-share-row" style="padding-left:15px;">
                <div class="fontstyle1" v-for="screeningRelateds in drivingSchool.screeningRelateds">
                        {{getScreenNameByCode(screeningRelateds.screeningId)}}
                </div>
            </div>
            <div class="row margin-top-five" v-if="drivingSchool.checkStatue==1">
                <div class="col-xs-12 learn-item">
                    <img src="../images/learncard.png" alt="" class="learn-icon"><span>约学车认证</span>
                </div>
            </div>
            <div class="row margin-top-five" v-if="drivingSchool.studyDeposit == 1">
                <div class="col-xs-12 learn-item">
                    <img src="../images/learndeposit.png" alt="" class="learn-icon"
                         style="padding-bottom: 5px;"><span>学车担保金</span>
                </div>
            </div>
            <component is="schoolAddressPage" :Address=drivingSchool.drivingAddress></component>
            <component is="schoolIntroducePage" :Title="title" :Desc="drivingSchool.schoolIntroduction" ></component>
            <component is="schoolTestifyPage" v-if="checkInfo" :checkInfo="checkInfo"></component>
            <component is="schoolClassPage" :ClassTypes="filteredData" :drivingSchoolId="drivingSchool.drivingSchoolId" :drivingSchoolName="drivingSchool.drivingSchoolName" :drivingSchoolScore="drivingSchool.drivingSchoolScore" :deposit="drivingSchool.deposit"></component>
            <component v-if="drivingSchool && ealuatevList" is="evaluatePage" :ealuatev="ealuatevList" :pid="drivingSchool.drivingSchoolId" :name="drivingSchool.drivingSchoolName" :type="1"  :count="datacount"></component>
            <div class="row" v-if="ealuatevList && datacount>0">
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
            <div class="row" style="margin-bottom: 85px"></div>
            <component v-if="payfooter" is="paySchoolFooterPage" :payfooter="payfooter"  :coachId="drivingSchool.drivingSchoolId" :coachName="drivingSchool.drivingSchoolName" :coachScore="drivingSchool.drivingSchoolScore" :deposit="drivingSchool.deposit"></component>
        </div>
    </div>
</template>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    module.exports = {
        data: function () {
            return {
                loading: true,
                post: null,
                error: null,
                payfooter:null,
                drivingSchool:null,
                screeningList:null,
                checkInfo:null,
                roleType:false,
                 isCoach:false,
                title:"驾校简介",
                object: {
                    id : "1",
                    Name: "欣祥和驾校(保工街分校)",
                    start: 4,
                    images: [{path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}],
                    estimate: [{estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}, {estimate: "啥都好"}, {estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}],
                    Address: "铁西区保工街100号",
                    Desc: "驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾",
                    book_study_card:1,
                    study_deposit : 1,
                    ealuatev: {
                        count: 299,
                        ealuatev: [{
                            id:"1",
                            UserName: "铥铥",
                            UserImage: "../images/coachimg.png",
                            Fraction: 4,
                            Time: "1分钟前",
                            Content: "评论内容.......",
                            estimate: [{estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}, {estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}, {estimate: "态度好"}, {estimate: "技术好"}]
                        }, {
                            id:"2",
                            UserName: "铥铥",
                            UserImage: "../images/coachimg.png",
                            Fraction: 4,
                            Time: "1分钟前",
                            Content: "评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容......评论内容.......",
                            estimate: [{estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}]
                        }, {
                            id:"3",
                            UserName: "铥铥",
                            UserImage: "../images/coachimg.png",
                            Fraction: 4,
                            Time: "1分钟前",
                            Content: "评论内容.......",
                            estimate: [{estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}]
                        },]
                    }
                },
                 ealuatevList: [],
                 pageInt:1,
                 showPage:false,
                 showmore:true,
                 showNull:false,
                 datacount:0,
                 lngM:storage.getStrLocalStorageItem(types.LNG),
                 latM:storage.getStrLocalStorageItem(types.LAT),
            }
        },
        created: function () {
             this.fetchData()
             types.DEL_ROUTER_PATH();//删除存储的路由跳转信息
        },
        computed:{
            filteredData: function () {
              var c1List=[];
              var b1List=[];
              var vipList=[];
              var resultData={};
              var ClassTypes=this.drivingSchool.classTypes;
              ClassTypes.forEach(function(classtype){
                     switch(classtype.classType){
                                case 1 :
                                            c1List.push(classtype);
                                            break;
                                case 2 :
                                            b1List.push(classtype);
                                            break;
                                case 3 :
                                            vipList.push(classtype);
                                            break;
                                }
              })
              c1List=c1List.sort(function(a,b){ a.classPrice - b.classPrice})
              b1List=b1List.sort(function(a,b){ a.classPrice - b.classPrice})
              vipList=vipList.sort(function(a,b){ a.classPrice - b.classPrice})
              if(c1List.length>0){
                this.payfooter=c1List[0];
              }else if(b1List.length>0){
                this.payfooter=blList[0];
              }else if(vipList.length>0){
                this.payfooter=vipList[0];
              }
              console.log(this.payfooter);
              resultData["c1List"]=c1List
              resultData["b1List"]=b1List
              resultData["vipList"]=vipList
              return resultData
            }
        },
        methods: {
           nextPage:function(){
                if(this.isDataNull){
                    return;
                }
                this.showPage=true;
                this.showmore=false;
                this.showNull=false;
                this.pageInt++;
                this.getEaluvatevList();
            },
            fetchData :function(){
                this.error = this.post = null
                this.loading = true
                 this.$http.post(types.HTTP_URL+'/drivingSchool/getDrivingSchoolDetail',
                 {drivingSchoolId:this.$route.params.paramId,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                           this.drivingSchool=response.data.data
                           console.log(this.drivingSchool);
                           if(this.drivingSchool.checkInfo){
                                this.getCheckInfo(this.drivingSchool.checkInfo);
                           }
                           this.isNotConcern();
                           this.getscreeningList();
                        }else{
                            alert("this is null");
                        }
                 })
            },
            isNotConcern:function(){
                    if(storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS)==1)
                    {
                       this.roleType=true;
                       this.isCoach=true;
                        return;
                    }
                   if(storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS)==2){
                        var user=storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)
                        this.$http.post(types.HTTP_URL+'/concern/selectConcernInfoWithDrivingSchool',{userId:user.userId,drivingSchoolId:this.drivingSchool.drivingSchoolId},{emulateJSON:true}).then(function(response){
                            if(response.data.status==1 && response.data.data!=null){
                                 this.roleType=true;
                            }
                        })
                   }
            },
            getscreeningList:function()
            {
                this.$http.post(types.HTTP_URL+'/screening/getAllScreening',{},{emulateJSON:true}).then(function(response){
                            this.screeningList = response.data.screeningList;
                            this.getEaluvatevList()
                            this.loading=false
                 })
            },
            getCheckInfo:function(checkInfo){
                this.$http.post(types.HTTP_URL+'/sysAdmin/selectUserAdminByUserId',{userid:checkInfo.adminId},{emulateJSON:true}).then(function(response){
                            this.checkInfo = response.data.data;
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
            getEaluvatevList:function(){
                this.$http.post(types.HTTP_URL+'/comment/selectDrivingSchoolCommentsPage',{drivingSchoolId:this.drivingSchool.drivingSchoolId,pageIndex:this.pageInt},
                    {emulateJSON:true}).then(function(response){
                            console.log(response.data)
                            if(response.data.status==1){
                                var ealuatev= response.data.data;
                                this.ealuatevList=this.ealuatevList.concat(ealuatev);
                                this.datacount=response.data.datacount;
                                this.showPage=false;
                                this.showmore=true;
                                this.showNull=false;
                            }else{
                                this.isDataNull=true;
                                this.showPage=false;
                                this.showmore=false;
                                this.showNull=true;
                            }
                     })
            },
            userConcern:function(){
                //if(this.roleType){
                //    return;
               // }
              if(storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)){
                     var schoolBuzz = this.drivingSchool.buzzCount+1;
                     var schoolConcern = this.drivingSchool.concernCount+1;
                    this.$http.post(types.HTTP_URL+'/drivingSchool/concernDrivingSchool',{buzzCount:schoolBuzz,concernCount:schoolConcern,drivingSchoolId:this.drivingSchool.drivingSchoolId,userId:storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO).userId},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.state==1){
                                console.log("已经关注");
                                this.roleType=true;
                            }else{

                            }
                     })
              }else{
                   this.routerPush();
              }
            },
            routerPush:function(){
                    var par=this.$route.params.paramId;
                    types.ADD_ROUTER_PATH("/schoolInfoPage","schoolInfoPage",par);
                    this.$router.push({path:"/register"});
            }
        },
        components: {
            'paySchoolFooterPage': require('../components/paySchoolFooterPage.vue'),
            'schoolAddressPage': require('../components/schoolAddressPage.vue'),
            'schoolIntroducePage': require('../components/schoolIntroducePage.vue'),
            'schoolTestifyPage': require('../components/schoolTestifyPage.vue'),
            'schoolClassPage': require('../components/schoolClassPage.vue'),
            'evaluatePage': require('../components/evaluatePage.vue'),
             'loading':require('../components/loading.vue')
        }
    }
    //    WeixinApi.ready(function(Api){
    //        var srcList = [];
    //        $.each($('img'),function(i,item){
    //            if(item.src) {
    //                srcList.push(item.src);
    //                $(item).click(function(e){
    //                    // 通过这个API就能直接调起微信客户端的图片播放组件了
    //                    Api.imagePreview(this.src,srcList);
    //                });
    //            }
    //        });
    //    });
</script>
