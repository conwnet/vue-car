<style>
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

    .header-line .flex-1 {
        -webkit-box-flex: 1;
        -webkit-flex: 1;
        flex: 1;
        margin: auto;
        color: white;
    }

    .header-line .flex-3 {
        -webkit-box-flex: 3;
        -webkit-flex: 3;
        flex: 3;
        margin: auto;
        position: relative;
    }

    .header-line .flex-3 .seach-input {
        width: 100%;
        border: 0;
        border-radius: 25px;
        padding: 5px 5px 5px 40px;
        outline: none;
    }

    .header-line .flex-3 .seach-icon {
        position: absolute;
        left: 6px;
        top: 6px;
        width: 22px;
        height: 22px;
    }

    .header-line .flex-1 .user-icon {
        width: 25px;
        height: 25px;
    }

    .header-line .flex-1 .arrow-icon {
        margin-left: 5px;
        width: 10px;
        height: 10px;
    }

    .choices-city-line {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        position: fixed;
        z-index: 600;
        width: 100%;
        top: 55px;
        left: 0;
        background: rgba(102, 102, 102, 0.2);
        text-align: center;
        height: 100%;
        display: none;
    }

    .choices-city-line .selected-city {
        background-color: white;
        width: 100%;
        height: 53px;
        text-align: left;
        padding: 15px;
    }

    .choices-city-line .selected-city p {
        color: black;
    }

    .choices-city-line .selected-city p span {
        color: #959595;
        margin-left: 10px;
    }

    .choices-city-line .city-list {
        height: 40%;
        width: 100%;
        background-color: #f1f1f1;
    }

    .choices-city-line .city-list .title {
        color: #999999;
        text-align: left;
        padding: 10px 20px;
    }

    .choices-city-line .city-list .c-list {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        padding: 0 15px;
        height: 80%;
        max-height: 80%;
        overflow-y: auto;
        text-align: center;
    }

    .choices-city-line .city-list .c-list .flex-1 {
        width: 33%;
        margin-bottom: 20px;
        padding: 0 5px;
    }

    .choices-city-line .city-list .c-list .flex-1 div {
        border: solid 1px #d7d7d7;
        border-radius: 3px;
        padding: 5px;
        background-color: white;
        font-size: 1.5rem;
    }

    .seach-list {
        z-index: 600;
        background-color: #f2f2f2;
        position: absolute;
        /*top: 55px;*/
        left: 0;
        display: none;
        width: 100%;
        overflow-y: auto;
    }
    .seach-list .no-seach-content{
        text-align: center;
        font-weight: bold;
    }
    .seach-list .seach-list-item{
        font-weight: bold;
        border-bottom: solid 1px #a9a9a9;
        padding: 10px 0;
        margin-left: 15px;
        margin-right: 15px;
    }
    .seach-list .seach-list-item .col-xs-12{
        padding: 0;
    }
    .seach-list-item:last-child{
        border: 0;
    }
    #coachSeachList , #schoolSeachList{
        border-top: solid 1px #a9a9a9;
        border-bottom: solid 1px #a9a9a9;
        background-color: white;
    }
    .seach-list .padding-15{
        padding: 15px;
    }
</style>
<template>
    <div>
        <div class="header-line">
            <div class="flex-1" @click="selectCity">{{titleCity}}<img class="arrow-icon"
                                                                      src="../images/arrow-icon.png">
            </div>
            <div class="flex-3"><input class="seach-input" placeholder="查找教练,驾校" v-model="wholeSeachValue"
                                       @keyup.enter="wholeSeach"><img class="seach-icon"
                                                                      src="../images/seach-icon.png">
            </div>
            <div class="flex-1"><img class="user-icon" src="../images/user-icon.png" @click="gotoRegister">
            </div>
        </div>
        <div class="choices-city-line">
            <div class="selected-city"><p>{{titleCity}}<span>GPS定位</span></p></div>
            <div class="city-list">
                <div class="title"><span>城市</span></div>
                <div class="c-list">
                    <div v-for="item in cityList" class="flex-1" @click="cliclCity(item.city)">
                        <div>{{item.city}}</div>
                    </div>
                </div>
            </div>
        </div>
        <!--<div style="margin-top: 500px;"><h1>asdfjkasdfjsdlakfjdklsafjaklsdhfkjsdahkjlasdghkjsdahgkjsdahfkajds</h1></div>-->
        <!--<div style="margin-top: 500px;"><h1>asdfjkasdfjsdlakfjdklsafjaklsdhfkjsdahkjlasdghkjsdahgkjsdahfkajds</h1></div>-->
        <div id="dialogs">
            <div class="js_dialog" id="iosDialog2" style="display: none;">
                <div class="weui-mask"></div>
                <div class="weui-dialog">
                    <div class="weui-dialog__bd">区域未开放</div>
                    <div class="weui-dialog__ft">
                        <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                    </div>
                </div>
            </div>
        </div>
        <div class="seach-list">
            <div style="padding-left: 15px;padding-top: 15px;"><a @click="closeSerch">取消</a></div>
            <div v-if="coachSeachList.length == 0 && schoolSeachList.length == 0" class="padding-15">
                <div class="no-seach-content">没有匹配</div>
                <div class="no-seach-content">输入更详细信息</div>
            </div>
            <div v-else>
                <div class="padding-15">最佳匹配教练</div>
                <div id="coachSeachList">
                    <div v-if="coachSeachList.length == 0">
                        <div class="no-seach-content">没有匹配</div>
                        <div class="no-seach-content">输入更详细信息</div>
                    </div>
                    <div v-else v-for="coach in coachSeachList" class="seach-list-item row">
                        <div class="col-xs-12" @click="gotoCoachInfo(coach.coachId)">{{coach.coachName}}</div>
                    </div>
                </div>
                <div class="padding-15">最佳匹配驾校</div>
                <div id="schoolSeachList">
                    <div v-if="schoolSeachList.length == 0">
                        <div class="no-seach-content">没有匹配</div>
                        <div class="no-seach-content">输入更详细信息</div>
                    </div>
                    <div v-else v-for="school in schoolSeachList" class="seach-list-item row">
                        <div class="col-xs-12" @click="gotoSchoolInfo(school.drivingSchoolId)">{{school.drivingSchoolName}}</div>
                    </div>
                </div>
            </div>
            <div class="padding-15" v-if="coachSeachList.length == 10 || schoolSeachList.length == 10" style="text-align: center;">
                请输入更详细信息
            </div>
        </div>
    </div>
</template>
<script>
    import * as types from "../store/mutation-types";
    import * as storage from '../store/localStorage'
    module.exports = {
        data: function () {
            return {
                cityList: [{city: "沈阳"}, {city: "北京"}, {city: "天津"}, {city: "上海"}, {city: "沈阳"}, {city: "北京"}, {city: "天津"}, {city: "上海"}, {city: "沈阳"}, {city: "北京"}, {city: "天津"}, {city: "上海"}, {city: "沈阳"}, {city: "北京"}, {city: "天津"}, {city: "上海"}, {city: "沈阳"}, {city: "北京"}, {city: "天津"}, {city: "上海"}, {city: "沈阳"}, {city: "北京"}, {city: "天津"}, {city: "上海"}],
                titleCity: "沈阳",
                wholeSeachValue: "",
                coachSeachList: [],
                schoolSeachList: [],
                showSeachList : false
            }
        },
        created: function () {
        },
        methods: {
            selectCity: function () {
                $(".choices-city-line").toggle();
                if ($(".choices-city-line").css("display") == "block") {
                   this.hiddenOverflowY();
                } else {
                    this.showOverflowY();
                }
            },
            cliclCity: function (city) {
                //this.titleCity = city;
                //$(".choices-city-line").hide();
                //  $("body").css("overflow-y", "auto");
                var $iosDialog2 = $('#iosDialog2');
                $iosDialog2.fadeIn(200);
            },
            closeDialog: function () {//关闭弹出层
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            },
            gotoRegister: function () {
                // this.$http.get('http://localhost/chouweichao/index.php/Cars/Index/index/{id}',{id:"11111",res:"thisrestu"},{emulateJSON:true}).then(function(response){
                //    console.log(response.data);
                //this.$data.user=response.data.user;
                // })
                //this.$router.push({path: 'register'});
               if(storage.getJsonLocalStorageItem(types.CACHE_COACH_INFO)){
                    this.showOverflowY();
                    this.$router.push({path: 'coachPerson'});
                }else if(storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)){
                    this.showOverflowY();
                    this.$router.push({path: 'studenPerson'});
                }else{
                    this.$router.push({path: 'register'});
                }
            },
            wholeSeach: function () {//全局查询
               $(".seach-list").css("height", ($(document.body)[0].clientHeight - 55) + "px");
               $(".seach-list").css("top", ($(window).scrollTop() + 55));
                this.getSchoolAndCoach();
                $(".seach-list").show();
                this.hiddenOverflowY();
                console.log("全局搜索内容:" + this.wholeSeachValue);
            },
            closeSerch: function () {
                $(".seach-list").hide();
                this.showOverflowY();
            },
            gotoSchoolInfo : function(id){
                this.showOverflowY();
                this.$router.push({path:"/schoolInfoPage",name:"schoolInfoPage",params: {paramId: id}});
            },
            gotoCoachInfo : function(id){
                this.showOverflowY();
                this.$router.push({path:"/coachInfoPage",name:"coachInfoPage",params: { paramId: id}});
            },
            getSchoolAndCoach : function(){//获取查询数据
                this.$http.post(types.HTTP_URL+'/headerPage/seachSchoolAndCoach',{seachCount :this.wholeSeachValue },
                        {emulateJSON:true}).then(function(response){
                    console.log("res =" + JSON.stringify(response.data))
                    if(response.data.schoolState == 0){
                        this.schoolSeachList = [];
                    } else {
                        this.schoolSeachList = response.data.schoolList;
                    }
                    if(response.data.coachState == 0){
                        this.coachSeachList = [];
                    } else {
                        this.coachSeachList = response.data.coachList;
                    }
                })
            },
            showOverflowY : function(){//显示滚动条
                $("body").css("overflow-y", "auto");
            },
            hiddenOverflowY : function(){//隐藏滚动条
                $("body").css("overflow-y", "hidden");
            }
        }
    }
</script>