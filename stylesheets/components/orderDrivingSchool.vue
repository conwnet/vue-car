<template>
    <div>
        <loading v-if="loading"></loading>
        <div  v-if="!loading">
            <div class="row topbanner text-center " >
                <div class="col-xs-12">
                    订单详情
                </div>
            </div>
            <div class="row orderds_back" style="margin-top:44px;">
                <div class="orderds_back_orderdetail_text">
                    <span>定金</span>
                </div>
                <div class="orderds_back_orderdetail_number">
                    <span>{{orderInfo.orderPrice}}</span>
                </div>
                <div >
                    <span>定金券</span>
                </div>
            </div>
            <component v-if="coachInfo" is="coachTable" :coachInfo="coachInfo"></component>
            <component v-if="drivingSchool" is="schoolTable" :drivingSchool="drivingSchool"></component>
            <div class="row" style="background:transparent;">
                <div id="c1-list" style="background: #e8e7e7;">
                    <div class="c1-list-item">
                        <div class="c1-list-item-line" >
                            <div class="flex-1">
                            </div>
                            <div class="flex-3">
                                <div>{{orderInfo.classtypeName}}
                                    <!--<div class="reg hot">HOT</div>-->
                                </div>
                                <div class="blue">{{orderInfo.classtypeIntroduction}}</div>
                                <div class="reg">¥{{orderInfo.classtypePrice}}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row" style="background: inherit;padding-top: 25px;">
                <div class="order_inputcode_list">
                    <div class="order_inputcode_list_item_1">
                    </div>
                    <div class="order_inputcode_list_item">
                        <div>
                            <input type="number" v-model="oneNum">
                        </div>
                        <div>
                            <input type="number" v-model="twoNum">
                        </div>
                        <div>
                            <input type="number" v-model="threeNum">
                        </div>
                        <div>
                            <input type="number" v-model="fourNum">
                        </div>
                    </div>
                    <div class="order_inputcode_list_item_1">
                    </div>
                </div>
            </div>
            <div class="margintop">
                <a  class="weui-btn  weui-btn_org" @click="completeOrder">驾校确认
                </a>
            </div>
            <div class="margintop">
                <div class="orderdetail_back_orderdetail_ts text-center">
                    温馨提示: 学员误点
                </div>
            </div>
            <div id="dialogs">
                <div class="js_dialog" id="iosDialog2" style="display: none;">
                    <div class="weui-mask"></div>
                    <div class="weui-dialog">
                        <div class="weui-dialog__bd">{{dialog_info}}</div>
                        <div class="weui-dialog__ft">
                            <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="less">
    @base_order_color: orangered;
    @red_order_color:red;
    .orderds_back{
        background-image: url(../images/order_back.png);
        background-color: transparent;
        background-size: 100% 100%;
        height: 14.23213rem;
        div{
            position: relative;
            text-align: center;
            color:@base_order_color;
        }
        &_orderdetail_text{
            margin-top: 6%;
            padding: 1.2rem 0px 0px 0px;
        }
        &_orderdetail_number{
            font-size:2.1em;
        }
    }
    .orderds_classtype_padding{
        padding:15px 0px 15px 0px !important;
    }
    .order_inputcode_list{
        display: flex;
        &_item{
            flex:4;
            div{
                float: left;
                display: block;
                width:25%;
                    input{
                        display: block;
                        width: 38px;
                        height: 38px;
                        border-radius: 5px;
                        border: 1px solid #d9d9d9;
                        background: #f9f9f9;
                        margin: auto;
                    }
            }

        }
        &_item_1{
            flex: 2;
        }
    }

</style>
<script>
import * as types from '../store/mutation-types'
import * as storage from "../store/localStorage"
    export default{
        data(){
            return{
                loading:true,
                coachInfo:null,
                orderInfo:null,
                drivingSchool:null,
                dialog_info:"请填写完整的确认码",
                lngM:storage.getStrLocalStorageItem(types.LNG),
                latM:storage.getStrLocalStorageItem(types.LAT),
            }
        },
        created:function(){
            this.initOrderInfo();
        },
        methods:{
            initOrderInfo:function(){
                 this.orderInfo=this.$store.state.orderstore.order;
                 if(!this.orderInfo){
                    this.dialog_info="获取订单失败";
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                 }
                 if(this.orderInfo.coachId){
                    this.$http.post(types.HTTP_URL+'/coach/getCoachByCoachId',
                    {coachId:this.orderInfo.coachId,lat:this.latM,lng:this.lngM},{emulateJSON:true}).then(function(response){
                            if(response.data.status==0){
                                this.dialog_info="教练信息异常，请返回重试";
                                var $iosDialog2 = $('#iosDialog2');
                                $iosDialog2.fadeIn(200);
                            }else{//
                                this.coachInfo=response.data.data;
                                console.log(this.coachInfo)
                                this.loading=false;
                            }
                  })
                 }
                  else if(this.orderInfo.drivingSchoolId){
                    this.$http.post(types.HTTP_URL+'/drivingSchool/getDrivingSchoolDetail',
                    {drivingSchoolId:this.orderInfo.drivingSchoolId,lat:this.latM,lng:this.lngM},{emulateJSON:true}).then(function(response){
                            if(response.data.status==0){
                                this.dialog_info="驾校信息异常，请返回重试";
                                var $iosDialog2 = $('#iosDialog2');
                                $iosDialog2.fadeIn(200);
                            }else{//
                                this.drivingSchool=response.data.data;
                                console.log(this.drivingSchool)
                                this.loading=false;
                            }
                  })
                  }
            },
            commitValidate:function(){
                if(!this.oneNum || !this.twoNum || !this.threeNum || !this.fourNum)
                {
                    this.dialog_info="请填写完整确认码";
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
                if(!this.coachInfo.coachOrderCode)
                {
                    this.dialog_info="个人或驾校确认码异常，请仔细核对";
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
                var userOrderCode=this.oneNum*1000+this.twoNum*100+this.threeNum*10+this.fourNum;
                if(userOrderCode != this.coachInfo.coachOrderCode)
                {
                     this.dialog_info="确认码不正确，请仔细核对";
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
                return true;
            },
            closeDialog : function(){
                    $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            },
            completeOrder:function(){
                if(!this.commitValidate()){
                    return;
                }
                this.$http.post(types.HTTP_URL+'/orderTab/updateOrderCompleted',{orderId:this.orderInfo.orderId},{emulateJSON:true}).then(function(response){
                        if(response.data.status==0){
                            this.dialog_info="订单更新失败";
                            var $iosDialog2 = $('#iosDialog2');
                            $iosDialog2.fadeIn(200);
                        }else if(response.data.status==1){//订单查询
                            console.log(response.data.data);
                            }
                            //相关跳转
                })
            }
        },
        components:{
         'schoolTable':require('../components/schoolTable.vue'),
         'coachTable':require('../components/coachTable.vue'),
         'loading':require('../components/loading.vue')
        }
    }
</script>
