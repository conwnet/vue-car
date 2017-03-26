<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="row concern-header-line">
                <div style="margin-top: 9px;">
                    <strong>学员订单详情</strong>
                </div>
            </div>
            <div>
                <div class="row concern-share-row">
                    <div class="col-xs-12">
                        <div class="row concern-share-row" style="padding-top: 6px;padding-bottom: 6px;">
                            <div v-if="order.coach==null">
                                <div style="margin-bottom: 4px;padding-top: 5px;">
                                    <div class="col-xs-12">
                                        <div class="row school-share-row">
                                            <div class="col-xs-3 school-share-row" style="text-align:center;">
                                                <img :src="img_url + order.drivingSchool.drivingSchoolPhoto" style="width: 60px;height: 60px;">
                                            </div>
                                            <div class="col-xs-7 school-share-row">
                                                <div class="row school-share-row">
                                                    {{order.drivingSchool.drivingSchoolName}}
                                                </div>
                                                <div class="row school-share-row">
                                                    <div v-for="stars in 5 ">
                                                        <img v-if="stars <= order.drivingSchool.drivingSchoolScore" src="../images/lightstar.png"
                                                             class="satrimg">
                                                        <img v-else src="../images/graystar.png" class="satrimg">
                                                    </div>
                                                    <div style="display: flex;white-space: nowrap;">&nbsp;¥{{order.drivingSchool.drivingCheapPrice}}起</div>
                                                </div>
                                                <div class="row school-share-row" style="margin-top: 1px;">
                                                    <div class="fontstyle1" v-for="screeningRelateds in order.drivingSchool.screeningRelateds">
                                                        {{getScreenNameByCode(screeningRelateds.screeningId)}}
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                        <div class="row">
                                            <div class="col-xs-3"></div>
                                            <div class="col-xs-9" style="border: solid 1px #f1f1f1;margin-top: 6px;text-align: right;">
                                                <!--<span class="distance">{{school.distance}}m</span>-->
                                            </div>
                                        </div>
                                        <div class="row school-share-row">
                                            <div class="col-xs-3 school-share-row"></div>
                                            <div class="col-xs-7 school-share-row">
                                                <div class="row school-share-row">
                                                    <div v-if="order.drivingSchool.checkStatue===1">
                                                        <img src="../images/learncard.png" class="school_photo"/>&nbsp;&nbsp;<span>约学车认证</span>
                                                    </div>
                                                    <div v-if="order.drivingSchool.studyDeposit===1">
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
                                <div style="margin-bottom: 4px;padding-top: 5px;">
                                    <div class="col-xs-12">
                                        <div class="row coach-share-row">
                                            <div class="col-xs-3 coach-share-row" style="text-align:center;">
                                                <img :src="get_image_path(order.coach.coachPhoto)" style="width: 60px;height: 60px;">
                                            </div>
                                            <div class="col-xs-7 coach-share-row">
                                                <div class="row coach-share-row">
                                                    {{order.coach.coachName}}
                                                </div>
                                                <div class="row coach-share-row">
                                                    <div v-for="stars in 5 ">
                                                        <img v-if="stars <= order.coach.coachScore" src="../images/lightstar.png"
                                                             class="satrimg">
                                                        <img v-else src="../images/graystar.png" class="satrimg">
                                                    </div>
                                                    <div style="display: flex;white-space: nowrap;">&nbsp;¥{{order.coach.coachCheapPrice}}起</div>
                                                </div>
                                                <div class="row coach-share-row" style="margin-top: 1px;">
                                                    <div class="fontstyle1" v-for="screeningRelateds in order.coach.screeningRelateds">
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
                                                    <div v-if="order.coach.checkStatue===1">
                                                        <img src="../images/learncard.png" class="coach_photo"/>&nbsp;&nbsp;<span>约学车认证</span>
                                                    </div>
                                                    <div v-if="order.coach.studyDeposit===1">
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
                        <div class="row class-share-row">
                            <div><strong>{{order.classtypeName}}</strong></div>
                            <div style="color: #00a2ea;font-size: 9px">{{order.classtypeIntroduction}}</div>
                            <div style="color: #fb7572;font-size: 9px">¥{{order.classtypePrice}}</div>
                        </div>
                        <div class="row concern-share-row" style="text-align: right;color:#ff6531;font-size:10px;padding-right:15px;">
                            定金¥{{order.orderPrice}}&nbsp;<span v-if="order.orderStatue==2">已完成</span><span v-if="order.orderStatue==0">未支付</span>
                            <span v-if="order.orderStatue==1">已支付</span>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row" style="height: 2px;background: transparent;"></div>
            <div class="row concern-share-row" style="text-align: right;padding-right: 14px;padding-top: 3px;height: 26px;">
                <img src="../images/tel-info-icom.png" alt="" style="width: 60px;height: 20px;">
            </div>
            <div class="row" style="height: 20px;background: transparent;"></div>
            <div class="row concern-share-row myorder-fontcolor">
                <div style="padding-top: 7px;">
                    订单编号：61{{year}}{{month}}{{day}}{{number}}
                </div>
                <div style="padding-top: 7px;padding-bottom: 7px;">
                    下单时间：{{order.orderRiseTime}}
                </div>
            </div>
        </div>
    </div>
</template>
<style>

</style>
<script>
import * as types from '../store/mutation-types'
import * as storage from "../store/localStorage"
    export default{
        data(){
            return{
                order:[],
                loading: true,
                year:"",
                month:"",
                day:"",
                number:"",
                screeningList:[],
                img_url:types.IMG_URL,
                lngM:storage.getStrLocalStorageItem(types.LNG),
                latM:storage.getStrLocalStorageItem(types.LAT),
                get_image_path : types.GET_IMAGE_PATH
            }
        },
        created:function(){
            this.getOrderByOrderId();
            this.getAllScreening();
        },
        components:{
             'loading':require('../components/loading.vue')
        },
        methods:{
            //教练完成学员的订单详细
            getOrderByOrderId:function(){
                this.$http.post(types.HTTP_URL+'/orderTab/getUserOrderInfo',
                {orderId:this.$route.params.orderId,lat:this.latM,lng:this.lngM},
                    {emulateJSON:true}).then(function(response){
                        console.log(response.data.object);
                        if(response.data.state==1){
                            this.order = response.data.object;
                            this.loading = false;
                            this.getOrderTime();
                            this.getNum(this.order.orderNum,3);
                        }else{
                            this.loading = false;
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
            getOrderTime:function(){
                var orderTime = this.order.orderRiseTime;
                var gettime = orderTime.substring(0, orderTime.indexOf(' '));
                var getDate = gettime.split("-");
                this.year = getDate[0];
                this.month = getDate[1];
                this.day = getDate[2];
            },
            getNum:function(num,size){
                if (num >= Math.pow(10, size)) { //如果num本身位数不小于size位
                    this.number =  num.toString();
                } else {
                    var _str = Array(size + 1).join('0') + num;
                    this.number = _str.slice(_str.length - size);
                }
            }
        }
    }
</script>
