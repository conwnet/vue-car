<template>
    <div>
        <div class="row" style="margin-bottom: 4px;padding-top: 5px;">
            <div class="col-xs-12">
                <div class="row school-share-row">
                    <div class="col-xs-3 school-share-row" style="text-align:center;">
                        <img :src="img_url + drivingSchool.drivingSchoolPhoto" style="width: 60px;">
                    </div>
                    <div class="col-xs-7 school-share-row">
                        <div class="row school-share-row">
                            {{drivingSchool.drivingSchoolName}}
                        </div>
                        <div class="row school-share-row">
                            <div v-for="stars in 5 ">
                                <img v-if="stars <= drivingSchool.drivingSchoolScore" src="../images/lightstar.png"
                                     class="satrimg">
                                <img v-else src="../images/graystar.png" class="satrimg">
                            </div>
                            <div style="display: flex;white-space: nowrap;">
                                &nbsp;¥{{drivingSchool.drivingCheapPrice}}起
                            </div>
                        </div>
                        <div class="row school-share-row" style="margin-top: 1px;">
                            <div class="fontstyle1" v-for="screeningRelateds in drivingSchool.screeningRelateds">
                                {{getScreenNameByCode(screeningRelateds.screeningId)}}
                            </div>
                        </div>
                    </div>

                </div>
                <div class="row">
                    <div class="col-xs-3"></div>
                    <div class="col-xs-9" style="border: solid 1px #f1f1f1;margin-top: 6px;text-align: right;">
                        <!--<span class="distance">{{drivingSchool.distanc}}</span>-->
                    </div>
                </div>
                <div class="row school-share-row">
                    <div class="col-xs-3 school-share-row"></div>
                    <div class="col-xs-7 school-share-row">
                        <div class="row school-share-row">
                            <div v-if="drivingSchool.checkStatue===1">
                                <img src="../images/learncard.png" class="school_photo"/>&nbsp;&nbsp;<span>约学车认证</span>
                            </div>
                            <div v-if="drivingSchool.studyDeposit===1">
                                <img src="../images/learndeposit.png"
                                     class="school_photo"/>&nbsp;&nbsp;<span>学车担保金</span>
                            </div>
                        </div>
                    </div>
                    <div class="col-xs-2"></div>
                </div>

            </div>
        </div>
    </div>
</template>
<style>
    .satrimg {
        flex: 1;
        display: flex;
        float: left;
        width: 19px;
        padding: 1px;
    }

    .school-share-row {
        padding-left: 5px;
        padding-right: 5px;
    }

    .fontstyle1 {
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

    .distance {
        position: absolute;
        margin: -29px 0px 0px -34px;
    }

    .schoolcancle {
        color: #F43530;
        white-space: nowrap;
    }

    .school_photo {
        width: 10%;
    }
</style>
<script>
    import * as types from "../store/mutation-types"
    export default{
        data(){
            return {
                screeningList: [],
                img_url: types.IMG_URL
            }
        },
        created: function () {
            this.getAllScreening();
        },
        props: ['drivingSchool'],
        methods: {
            getAllScreening: function () {
                this.$http.post(types.HTTP_URL + '/screening/getAllScreening', {emulateJSON: true}).then(function (response) {
                    this.screeningList = response.data.screeningList;
                })
            },
            getScreenNameByCode: function (code) {
                for (var i = 0; i < this.screeningList.length; i++) {
                    if (code == this.screeningList[i].id) {
                        return this.screeningList[i].name;
                    }
                }
            },
        }

    }
</script>
