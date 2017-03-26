<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div style="margin-top: 55px;">
                <component is='headerPage'></component>
                <component is='indexslider' :sliderType="3"></component>
            </div>
            <div class="row" style="position:relative">
                <div class="screening-flex">
                    <div class="screening-flex-item">
                        <div class="screening-flex-tag" @click="clickTag('screening-list')">筛选<img
                                src="../images/arrow-down-icon.png"></div>
                    </div>
                    <div class="screening-flex-item">
                        <div class="screening-flex-tag" @click="getMore(0,'','')">口碑<img src="../images/arrow-down-icon.png"></div>
                    </div>
                    <div class="screening-flex-item">
                        <div class="screening-flex-tag" @click="clickTag('region-list')">区域<img
                                src="../images/arrow-down-icon.png"></div>
                    </div>
                    <div class="screening-flex-item">
                        <div class="screening-flex-tag" @click="clickTag('noopsyche-list')">智能<img src="../images/arrow-down-icon.png"></div>
                    </div>
                    <div class="list-item screening-list">
                        <div id="screening-list">
                            <div class="screening-list-flex" v-for="item in screeningList">
                                <div @click="selectedTag($event,item)">{{item.name}}</div>
                            </div>
                            <div class="fun-but">
                                <div @click="clickTag('screening-list')">取消</div>
                            </div>
                            <div class="fun-but">
                                <div class="color-blue" @click="getMore(5,'','screening-list')">确认</div>
                            </div>
                        </div>
                    </div>
                    <div id="praise-list" class="list-item">

                    </div>
                    <div class="list-item region-list">
                        <div id="region-list">
                            <div class="region-list-title">附近</div>
                            <div class="region-list-item">
                                <div v-for="item in regionList" @click="getMore(3,item,'region-list')">{{item.name}}</div>
                            </div>
                        </div>
                    </div>

                    <div class="list-item noopsyche-list">
                        <div id="noopsyche-list">
                            <div class="noopsyche-list-title">智能排序</div>
                            <div class="noopsyche-list-item">
                                <div v-for="item in noopsycheList" @click="getMore(1,item,'noopsyche-list')">{{item.name}}</div>
                            </div>
                        </div>
                    </div>
                </div>
                <div id="bubble-arrow"></div>
                <div id="bubble-arrow-inner"></div>
            </div>
            <div v-for="scList in schoolListPage">
                <div class="row" v-for="school in scList" style="margin-bottom:4px;padding-top: 5px;" @click="gotoInfo(school)">
                    <div class="col-xs-12">
                        <div class="row fs-share-row">
                            <div class="col-xs-3 fs-share-row" style="text-align:center;padding-top: 5px;">
                                <img :src="img_url+school.drivingSchoolPhoto" style="width: 58px;height: 58px;">
                            </div>
                            <div class="col-xs-7 fs-share-row">
                                <div class="row fs-share-row">
                                    {{school.drivingSchoolName}}
                                </div>
                                <div class="row fs-share-row">
                                    <div v-for="stars in 5 ">
                                        <img v-if="stars <= school.drivingSchoolScore" src="../images/lightstar.png"
                                             class="satrimg">
                                        <img v-else src="../images/graystar.png" class="satrimg">
                                    </div>
                                    <div style="display: flex;white-space: nowrap;">&nbsp;¥{{school.drivingCheapPrice}}起</div>
                                </div>
                                <div class="row fs-share-row" style="margin-top: 1px;display: flex;float: left;">
                                    <div class="col-xs-7 fontstyle1" v-for="screeningRelateds in school.screeningRelateds">
                                        {{getScreenNameByCode(screeningRelateds.screeningId)}}
                                    </div>
                                </div>
                            </div>
                            <div class="col-xs-2 fs-share-row" style="position: relative;text-align: right;padding: 74px 12px 0px 0px;">
                                <span class="distance">{{getDistance(school.distanc)}}</span>
                            </div>
                        </div>
                        <div class="row">
                            <div class="col-xs-3"></div>
                            <div class="col-xs-9" style="border: solid 1px #f1f1f1;margin-top: 6px;text-align: right;float:right;">

                            </div>
                        </div>
                        <div class="row fs-share-row">
                            <div class="col-xs-3 fs-share-row"></div>
                            <div class="col-xs-7 fs-share-row">
                                <div class="row fs-share-row">
                                    <div v-if="school.checkStatue===1" style="padding-top: 2px;padding-bottom: 4px;">
                                        <img src="../images/learncard.png" class="school_photo"/>&nbsp;&nbsp;
                                        <span>约学车认证</span>
                                    </div>
                                    <div v-if="school.studyDeposit===1" style="padding-bottom: 7px;">
                                        <img src="../images/learndeposit.png" class="school_photo"/>&nbsp;&nbsp;
                                        <span>学车担保金</span>
                                    </div>
                                </div>
                            </div>
                            <div class="col-xs-2"></div>
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
                        <span>加载中..</span><img  src="../images/loadingPage.gif" style="height: 40px;width: 40px;">
                    </div>
                    <div v-if="showNull" style="padding-top: 8px;">
                        <span>无更多信息</span>
                    </div>
                </div>
            </div>
            <div class="row">
                <div class="col-xs-12" style="height:60px">

                </div>
            </div>
            <div>
                <component is='footerPage' :isSelected='2'></component>
            </div>
            <div id="dialogs">
                <div class="js_dialog" id="findSchoolDialog" style="display: none;">
                    <div class="weui-mask"></div>
                    <div class="weui-dialog">
                        <div class="weui-dialog__bd">最多选择4项</div>
                        <div class="weui-dialog__ft">
                            <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
</template>
<style lang="less">
    .satrimg{
        flex: 1;
        display: flex;
        float: left;
        width:19px;
        padding: 1px;
    }

    .fs-share-row{
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

    .cancle{
        color:#F43530;
        display:none;
        white-space: nowrap;
    }

    .school_photo{
        width: 10%;
    }

    .screening-flex {
        display: flex;
        display: -webkit-box;
        display: -webkit-flex;
        text-align: center;
        border-top: solid 10px #f0efed;
        border-bottom: solid 1px #f0efed;
        height: 55px;
        position: relative;
    }

    .screening-flex .screening-flex-item {
        flex: 1;
        height: 44px;
        line-height: 44px;
    }

    .screening-flex .screening-flex-item img {
        width: 15px;
        height: 10px;
    }

    .list-item {
        position: absolute;
        display: none;
        top: 50px;
        left: 0;
        z-index: 600;
        width: 100%;
        background-color: white;
    }

    #screening-list {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        text-align: center;
    }

    #screening-list .screening-list-flex {
        width: 25%;
        margin-bottom: 20px;
        padding: 0 5px;
    }

    #screening-list .screening-list-flex div {
        border: solid 1px #d7d7d7;
        border-radius: 3px;
        padding: 5px;
        background-color: white;
        font-size: 1.5rem;
        overflow: hidden;
        white-space: nowrap;
    }

    .sereening-selected-tag {
        border-color: #ff6634 !important;
        background-color: #fef0ed !important;
        color: #ff6634 !important;
    }

    #screening-list .fun-but {
        width: 50%;
        padding: 0 5px;
    }

    #screening-list .fun-but div {
        display: inline-block;
        border: solid 1px #d7d7d7;
        border-radius: 3px;
        padding: 5px;
        font-size: 1rem;
        background-color: white;
        text-align: center;
        width: 100%;
    }

    .color-blue {
        border-color: #00a0ea !important;
        color: #00a0ea !important;
    }

    #region-list {
        width: 100%;
    }

    #region-list .region-list-title {
        padding: 0 10px 15px 10px;
        text-align: left;
    }

    #region-list .region-list-item {
        height: 100%;
        width: 100%;
        text-align: left;
        background-color: #f4f4f4;
    }

    #region-list .region-list-item div {
        padding: 10px 10px;
    }

    #noopsyche-list{
        width: 100%;
    }
    #noopsyche-list .noopsyche-list-title{
        color:#ff7f00;
        padding: 0 10px 15px 10px;
        text-align: left;
        border-bottom:solid 1px #ff7f00;
    }
    #noopsyche-list .noopsyche-list-item{
        height: 100%;
        width: 100%;
        text-align: left;
    }
    #noopsyche-list .noopsyche-list-item div{
        padding: 10px 10px;
        border-bottom:solid 1px #d7d7d7;
    }

    .bubble-arrow-screening-list {
        border-color:transparent transparent #f0efed transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 34px;
        left: 8%;
    }
    .bubble-arrow-inner-screening-list {
        border-color:transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 36px;
        left: 8%;
    }

    .bubble-arrow-region-list {
        border-color:transparent transparent #f0efed transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 34px;
        left: 58%;
    }
    .bubble-arrow-inner-region-list {
        border-color:transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 36px;
        left: 58%;
    }

    .bubble-arrow-noopsyche-list {
        border-color:transparent transparent #f0efed transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 34px;
        left: 83%;
    }
    .bubble-arrow-inner-noopsyche-list {
        border-color:transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 36px;
        left: 83%;
    }

</style>
<script>
    var headerPage=require('../components/headerPage.vue');
    var indexslider=require('../components/indexslider.vue');
    var footerPage=require('../components/footerPage.vue');
    import * as types from "../store/mutation-types";
    import * as storage from "../store/localStorage"
    export default{
        data(){
            return{
                 loading: true,
                 schoolList:[],
                 schoolListPage:[],
                 previousTag: "",
                 screeningList:[],
                 regionList:[],
                 screeningName:[],
                 screeningid:"",
                 screeningChooseList:[],
                 noopsycheList :[{name:"离我最近"},{name:"人气最高"},{name:"评价最好"},{name:"价格最低"},{name:"下票最快"},{name:"服务最佳"},{name:"合格率最高"}],
                 screeningCount : 0,//标签筛选数量
                 seachSchollTypeName:null,//按名次搜索的name值
                 seachSchoolByArea:null,//按区域进行查询
                 seachSchoolByScreening:null,//按筛选进行查询
                 pageInt:1,
                 city:"沈阳市",
                 //scrollHeight: $(document.body)[0].clientHeight,
                 //windowHeight:0,
                 isDataNull:false,//判断list是否还有值
                 domId:null,//给div-id
                 showPage:false,
                 showmore:true,
                 showNull:false,
                 lngM:storage.getStrLocalStorageItem(types.LNG),
                 latM:storage.getStrLocalStorageItem(types.LAT),
                 count:0,
                 pageStart:0,
                 pageSize:6,
                 img_url:types.IMG_URL
            }
        },
        created:function(){
            this.getAllScreening();
            this.getArea();
            this.getAllDrivingSchool();
            //this.lngM = ;
            //this.latM = ;
            //alert(storage.getStrLocalStorageItem(types.LNG));
            //alert(storage.getStrLocalStorageItem(types.LAT));
        },
        components:{
             headerPage,
             indexslider,
             footerPage,
             'loading':require('../components/loading.vue')
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
                if(this.seachSchollTypeName){
                    if(this.seachSchollTypeName.name=="离我最近"){
                        this.getMore(8,this.seachSchollTypeName,this.domId);
                    }else{
                        this.getMore(2,this.seachSchollTypeName,this.domId);
                    }
                }else if(this.seachSchoolByArea){
                    this.getMore(4,this.seachSchoolByArea,this.domId);
                }else if(this.seachSchoolByScreening){
                    this.getMore(6,"","");
                }else{
                    this.getMore(7,"","");
                }
            },
            getMore: function (schoolType,{name},id) {
                this.domId=id;
                switch(schoolType){
                    case 0:
                    console.log("case 0");
                    this.clearAllSchoolList();
                    this.getAllDrivingSchool();
                    break;
                    case 7:
                    console.log("case 7");
                    this.getAllDrivingSchool();
                    break;
                    case 1:
                    console.log("case 1");
                    this.clickTag(id);
                    this.clearAllSchoolList();
                    this.brainOrder({name},id);
                    break;
                    case 2:
                    console.log("case 2");
                    this.brainOrder({name},id);
                    break;
                    case 3:
                    console.log("case 3");
                    this.clickTag(id);
                    this.clearAllSchoolList();
                    this.getAreaName({name},id);
                    break;
                    case 4:
                    console.log("case 4");
                    this.getAreaName({name},id);
                    break;
                    case 5:
                    console.log("case 5");
                    this.clearAllSchoolList();
                    this.clickTag(id);
                    this.getDrivingSchoolbyLable();
                    break;
                    case 6:
                    console.log("case 6");
                    this.getDrivingSchoolbyLable();
                    break;
                    case 8:
                    console.log("case 8");
                    this.count++;
                    this.pageStart = this.count * this.pageSize;
                    this.getDateByDistance();
                    break;
                }
            },
            //关闭选择栏
            clickTag: function (id) {
                if (this.previousTag != "" && this.previousTag != id) {
                    $("." + this.previousTag).hide();
                    $("#bubble-arrow").removeClass("bubble-arrow-" + this.previousTag);
                    $("#bubble-arrow-inner").removeClass("bubble-arrow-inner-" + this.previousTag);
                }
                $("#bubble-arrow").toggleClass("bubble-arrow-" + id);
                $("#bubble-arrow-inner").toggleClass("bubble-arrow-inner-" + id);
                $("." + id).toggle();
                this.previousTag = id;
            },
            //清空所有
            clearAllSchoolList:function(){
                this.pageInt = 1;
                this.schoolListPage=[];
                this.isDataNull = false;
                this.seachSchoolByArea=null;
                this.seachSchoolByScreening=null;
                this.seachSchollTypeName=null;
                this.domId=null;
            },
            //离我最近分页查询
            getDateByDistance:function(){
                 //前台分页
                if(this.pageSize<6){
                    this.showPage=false;
                    this.showmore=false;
                    this.showNull=true;
                    this.isDataNull=true;
                    return;
                }
                var sum = this.schoolList.length;
                var result=[];
                console.log(this.pageStart);
                if(sum-this.pageStart<this.pageSize){
                    this.pageSize = sum-this.pageStart;
                }
                for(var i=this.pageStart;i<(this.pageStart+this.pageSize);i++){
                    result.push(this.schoolList[i]);
                }
                this.schoolListPage.push(result);
                this.showPage=false;
                this.showmore=true;
                this.showNull=false;
            },
            //根据智能列表筛选
            brainOrder:function({name},id){
                if({name}.name){
                    this.seachSchollTypeName = {name};
                }
                if({name}.name==="离我最近"){
                    this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByDistanc',
                    {cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.state==1){
                                this.count=0,
                                this.pageStart=0,
                                this.pageSize=6,
                                this.schoolListPage=[];
                                this.isDataNull=false;
                                this.schoolList = response.data.list;
                                this.getDateByDistance();
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
                }
                if({name}.name==="人气最高"){
                    this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByBuzz',
                    {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
                }
                if({name}.name==="评价最好"){
                     this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByScore',
                     {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
                }
                if({name}.name==="下票最快"){
                     this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByLicence',
                     {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
                }
                if({name}.name==="合格率最高"){
                     this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByRate',
                     {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                         if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
                }
                 if({name}.name==="服务最佳"){
                     this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByService',
                     {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
                }
                if({name}.name==="价格最低"){
                    this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByPrice',
                    {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
                }
            },
            selectedTag: function (e,{id}) {
                var el = e.currentTarget;
                if(this.screeningCount == 4 && !el.getAttribute("class", "sereening-selected-tag")){
                    var $iosDialog2 = $('#findSchoolDialog');
                    $iosDialog2.fadeIn(200);
                } else {
                    if (el.getAttribute("class", "sereening-selected-tag")) {
                        el.removeAttribute("class", "sereening-selected-tag");
                        this.screeningCount--;
                        Array.prototype.indexOf = function(val) {
                                for (var i = 0; i < this.length; i++) {
                                        if (this[i] == val) return i;
                                }
                                return -1;
                         };
                         Array.prototype.remove = function(val) {
                                var index = this.indexOf(val);
                                if (index > -1) {
                                    this.splice(index, 1);
                                }
                         };
                        this.screeningChooseList.remove({id}.id);
                    } else {
                        el.setAttribute("class", "sereening-selected-tag");
                        this.screeningCount++;
                        this.screeningChooseList.push({id}.id);
                    }
                }
//                el.getAttribute("class", "sereening-selected-tag") ? el.removeAttribute("class", "sereening-selected-tag") : el.setAttribute("class", "sereening-selected-tag");
            },
             closeDialog: function () {
                 $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
             },
             //点击跳转到详情页
             gotoInfo:function({drivingSchoolId}){
                   this.$router.push({path:"/schoolInfoPage",name:"schoolInfoPage",params: { paramId: {drivingSchoolId}.drivingSchoolId }});
             },
             //查询所有驾校
             getAllDrivingSchool: function () {
                this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolWeb',
                {pageIndex:this.pageInt,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                            this.schoolList = response.data.list;
                            this.schoolListPage.push(this.schoolList);
                            this.showPage=false;
                            this.showmore=true;
                            this.showNull=false;
                            this.loading=false;
                        }else{
                            this.isDataNull=true;
                            this.showPage=false;
                            this.showmore=false;
                            this.showNull=true;
                            this.loading=false;
                        }
                 })
            },

            //查询所有筛选标签
            getAllScreening: function () {
                this.$http.post(types.HTTP_URL+'/screening/getAllScreening',{emulateJSON:true}).then(function(response){
                            this.screeningList = response.data.screeningList;
                 })
            },
            //对筛选标签id进行转码
            getScreenNameByCode:function(code){
                for(var i=0;i<this.screeningList.length;i++)
                {
                    if(code == this.screeningList[i].id){
                        return this.screeningList[i].name;
                    }
                }
            },
            //获取市所对区
            getArea: function () {
                 this.$http.post(types.HTTP_URL+'/districtInfo/getDistrictByCity',{pid:50},{emulateJSON:true}).then(function(response){
                            this.regionList = response.data.list;
                 })
            },
            //根据区域查询
            getAreaName:function({name},classid){
                 if({name}.name){
                    this.seachSchoolByArea = {name};
                }
                 this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByArea',
                 {pageIndex:this.pageInt,cityName:this.city,districtName:{name}.name,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                           if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
            //筛选查询
            getDrivingSchoolbyLable:function(){
                if(this.screeningChooseList){
                    this.seachSchoolByScreening = this.screeningChooseList;
                }
                this.screeningid = this.screeningChooseList.join(",");
                this.$http.post(types.HTTP_URL+'/drivingSchool/getAllDrivingSchoolByScreening',
                {pageIndex:this.pageInt,screeningArry:this.screeningid,cityName:this.city,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        console.log(response.data.status);
                        if(response.data.status==1){
                                this.schoolList = response.data.list;
                                this.schoolListPage.push(this.schoolList);
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
            getDistance:function(distance){
                if(distance < 1000){
                    return distance+"m";
                }
                else if(distance > 1000){
                    return (Math.round(distance/100)/10).toFixed(1) + "km"
                }
            }
        }
    }
</script>
