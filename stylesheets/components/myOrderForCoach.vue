<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
        <div class="row concern-header-line">
            <div style="margin-top: 9px;">
                <strong>我的订单</strong>
            </div>
        </div>
        <div>
            <div class="row concern-share-row">
                <div class="col-xs-12">
                    <div class="row concern-share-row" style="padding-top: 6px;padding-bottom: 6px;">
                        <img :src="order.studentsOrder.userPhoto" class="concern-photo-two">&nbsp;&nbsp;&nbsp;<strong>{{getdecodeUTF8(order.studentsOrder.userName)}}</strong>
                    </div>
                    <div class="row class-share-row">
                        <div><strong>{{order.classtypeName}}</strong></div>
                        <div style="color: #00a2ea;font-size: 9px">{{order.classtypeIntroduction}}</div>
                        <div style="color: #fb7572;font-size: 9px">¥{{order.classtypePrice}}</div>
                    </div>
                    <div class="row concern-share-row" style="text-align: right;color:#ff6531;font-size:10px;padding-right:15px;">
                        定金¥{{order.orderPrice}}&nbsp;<span v-if="order.orderStatue==2">已成交</span>
                    </div>
                </div>
            </div>
        </div>
            <div class="row" style="height: 5px;background: transparent;"></div>
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
    .myorder-fontcolor{
        font-size: 9px;
        padding-left: 10px;
        color: #9f9f9f;
    }
</style>
<script>
import * as types from '../store/mutation-types'
    export default{
        data(){
            return{
                order:[],
                loading: true,
                 year:"",
                month:"",
                day:"",
                number:"",
                img_url:types.IMG_URL
            }
        },
        created:function(){
            this.getOrderByCoachId();
        },
        components:{
             'loading':require('../components/loading.vue')
        },
        methods:{
            //教练完成学员的订单详细
            getOrderByCoachId:function(){
                this.$http.post(types.HTTP_URL+'/orderTab/getUserInfo',{orderId:this.$route.params.orderId},
                    {emulateJSON:true}).then(function(response){
                        console.log();
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
             getdecodeUTF8:function(arr){
                  return decodeURI(arr,"utf-8")
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
