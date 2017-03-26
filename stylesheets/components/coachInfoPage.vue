<style>
        .coach_star_style{
                width: 19px;
                padding: 1px;
        }
</style>
<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
        <div class="page-title row">详情页</div>
        <div class="row margin-top-twenty">
            <div class="col-xs-3">
                <img src="get_image_path(coach.coachPhoto)" alt="" class="img-responsive">
            </div>
            <div class="col-xs-9" style=" border-bottom: solid 1px #f0f0f0;">
                <!--<div class="row">-->
                    <div class="row">
                        <div class="col-xs-6" style="text-align: left;margin-left: -15px;">
                            <span>{{coach.coachName}}</span>
                        </div>
                        <div class="col-xs-6" style="text-align: right;">
                            <a @click="userConcern" v-if="!roleType">+关注</a>
                            <a  v-if="roleType && !isCoach">已关注</a>
                        </div>
                    </div>
                    <div class="row">
                            <img v-for="start in coach.coachScore" src="../images/lightstar.png" alt=""
                                 class="star-icon coach_star_style"><img
                                v-for="start in 5-coach.coachScore" src="../images/graystar.png" alt=""
                                class="star-icon coach_star_style"><span style="margin-left: 5px;" v-if="payfooter">¥{{payfooter.classtypePrice}}起</span>
                    </div>
                    <div class="row" style="margin-bottom: 5px;">
                        <div class="col-xs-9">
                            <div class="row fs-share-row"  style="padding-left:0px;">
                                    <div class="fontstyle1" v-for="screeningRelateds in coach.screeningRelateds">
                                        {{getScreenNameByCode(screeningRelateds.screeningId)}}
                                    </div>
                            </div>
                        </div>
                        <div class="col-xs-3">
                            <span style="position:absolute;top: 0; right: 15px;">{{getDistance(coach.distanc)}}</span>
                        </div>
                    </div>
                <!--</div>-->
            </div>
        </div>
        <div class="row margin-top-five" v-if="coach.checkStatue==1">
            <div class="col-xs-3"></div>
            <div class="col-xs-9" style="margin-left: -15px;">
                <img src="../images/learncard.png" alt="" class="learn-icon"><span>    约学车认证</span>
            </div>
        </div>
        <div class="row margin-top-five" v-if="coach.studyDeposit == 1">
            <div class="col-xs-3"></div>
            <div class="col-xs-9" style="margin-left: -15px;">
                <img src="../images/learndeposit.png" alt="" class="learn-icon"><span>    学车担保金</span>
            </div>
        </div>
        <div class="row margin-top-five img-flex" v-if="object.images.length > 5">
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
            <div class="img-flex-item" v-for="item in object.images">
                <img src="../images/car-img.png" alt="" class="img-responsive">
            </div>
        </div>
        <component is="schoolAddressPage" :Address="coach.learnSpace"></component>
        <component is="schoolIntroducePage" :Title="title" :Desc="coach.coachIntroduction"></component>
        <component is="schoolTestifyPage" v-if="checkInfo" :checkInfo="checkInfo"></component>
        <component is="coachClassPage"  :ClassTypes="filteredData" :coachId="coach.coachId" :coachName="coach.coachName" :coachScore="coach.coachScore" :deposit="coach.deposit"></component>
        <component v-if="coach && ealuatevList" is="evaluatePage" :ealuatev="ealuatevList" :pid="coach.coachId" :name="coach.coachName" :type="2"  :count="datacount"></component>
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
        <component v-if="payfooter" is="payFooTerPage" :payfooter="payfooter"  :coachId="coach.coachId" :coachName="coach.coachName" :coachScore="coach.coachScore" :deposit="coach.deposit"></component>
    </div>
    </div>
</template>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    module.exports = {
        data: function (){
            return {
                loading: true,
                post: null,
                error: null,
                payfooter:null,
                coach:null,
                screeningList:null,
                checkInfo:null,
                roleType:false,
                isCoach:false,
                title:"个人简介",
                object: {
                    id:"123",
                    Name: "王教练",
                    start: 4,
                    images: [{path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}, {path: "../images/car-img.png"}],
                    estimate: [{estimate: "服务好"}, {estimate: "态度好"}, {estimate: "技术好"}, {estimate: "啥都好"}, {estimate: "技术好"}, {estimate: "啥都好"}, {estimate: "技术好"}, {estimate: "啥都好"}],
                    Address: "铁西区保工街100号",
                    Desc: "驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾校描述驾",
                    price: 2999,
                    distance: 1.5
                },
                currentPage: 1,       //页码
                scrollHeight: $(document.body)[0].clientHeight,//可视窗口高度
                ealuatevList: [],
                pageInt:1,
                showPage:false,
                showmore:true,
                showNull:false,
                datacount:0,
                img_url:types.IMG_URL,
                lngM:storage.getStrLocalStorageItem(types.LNG),
                latM:storage.getStrLocalStorageItem(types.LAT),
                get_image_path : types.GET_IMAGE_PATH
            }
        },
        created: function () {
            //storage.cleanLocalStorage();
            this.fetchData()
            types.DEL_ROUTER_PATH();//删除存储的路由跳转信息
        },
        computed:{
            filteredData: function () {
              var c1List=[];
              var b1List=[];
              var vipList=[];
              var resultData={};
              var ClassTypes=this.coach.classTypes;
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
                 this.$http.post(types.HTTP_URL+'/coach/getCoachDetail',
                 {coachId:this.$route.params.paramId,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                           this.coach=response.data.data
                           console.log(this.coach);
                           if(this.coach.checkInfo){
                                this.getCheckInfo(this.coach.checkInfo);
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
                        this.$http.post(types.HTTP_URL+'/concern/selectConcernInfoWithCoach',{userId:user.userId,coachId:this.coach.coachId},{emulateJSON:true}).then(function(response){
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
                this.$http.post(types.HTTP_URL+'/comment/selectCoachCommentsPage',{coachId:this.coach.coachId,pageIndex:this.pageInt},
                    {emulateJSON:true}).then(function(response){
                            console.log(response.data)
                            if(response.data.status==1){
                                var ealuatev= response.data.data;
                                this.ealuatevList=this.ealuatevList.concat(ealuatev);
                                console.log(this.ealuatevList)
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
              if(storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)){
                     var coachBuzz = this.coach.buzzCount+1;
                     var coachConcern = this.coach.concernCount+1;
                    this.$http.post(types.HTTP_URL+'/coach/concernCoach',{buzzCount:coachBuzz,concernCount:coachConcern,coachId:this.coach.coachId,userId:storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO).userId},
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
                    types.ADD_ROUTER_PATH("/coachInfoPage","coachInfoPage",par);
                    this.$router.push({path:"/register"});
            },
            getDistance:function(distance){
                if(distance < 1000){
                    return distance+"m";
                }
                else if(distance > 1000){
                    return (Math.round(distance/100)/10).toFixed(1) + "km"
                }
            }
        },

        components: {
            'payFooTerPage': require('../components/payFooTerPage.vue'),
            'schoolAddressPage': require('../components/schoolAddressPage.vue'),
            'schoolIntroducePage': require('../components/schoolIntroducePage.vue'),
            'schoolTestifyPage': require('../components/schoolTestifyPage.vue'),
            'coachClassPage': require('../components/coachClassPage.vue'),
            'evaluatePage': require('../components/evaluatePage.vue'),
            'loading':require('../components/loading.vue')
        },

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
