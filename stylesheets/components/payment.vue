<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="row topbanner text-center " >
                <div class="col-xs-12">
                    确认支付
                </div>
            </div>
            <div class="row" style="margin-top:44px;">

                <div class="weui-cells  coach-cells" style="margin-top:0px;">
                    <div class="weui-cell">
                        <div class="weui-cell__hd">
                            <label class="weui-label" style="font-size:17px;">{{payforInfo.classtypeName}}</label>
                            <div class="base-color" style="float:left;">
                                {{payforInfo.classtypeIntroduction}}
                            </div>
                            <div class="payment_price">
                                <span>￥</span>{{payforInfo.classtypePrice}}
                            </div>
                            <div class="weui-cell__hd_class_price">
                                <div class="payment_nu">￥</div>
                                <div class="payment_count">{{payforInfo.orderPrice}}</div>
                                <div class="payment_text">定金</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row margintop" >
                <div class="weui-cells  coach-cells" style="margin-top:0px">
                    <div class="weui-cell weui-cell_switch">
                        <div class="weui-cell__bd" style="font-weight:700;">使用约学车平台优惠券</div>
                        <div class="weui-cell__ft">
                            <input class="weui-switch" type="checkbox" name="yhq"  v-model="toggle"  @click="selectYh">
                        </div>
                    </div>
                </div>
            </div>
            <div class="row margintop">
                <div class="weui-cells coach-cells weui-cells_checkbox weui-cells_checkbox_payment" style="margin-top:0px">
                    <label class="weui-cell weui-check__label" for="x12">
                        <div class="weui-cell__hd">
                            <img src="../images/weixinpayment.png" alt="" style="width:20px;margin-right:5px;display:block">
                        </div>
                        <div class="weui-cell__bd" style="padding-left: 15px;">
                            <span>微信支付</span><br/>
                            <span style="font-size: 12px;color: #aaaa9a;">推荐安装微信5.0及以上版本的用户使用</span>
                        </div>
                        <div class="weui-cell__ft">
                            <input type="radio" name="radio1" class="weui-check weui-check_payment" id="x12" checked="checked">
                            <span class="weui-icon-checked"></span>
                        </div>
                    </label>
                </div>
            </div>
            <div class="row margintop" >
            </div>
            <div class="row margintop" >
            </div>
            <div class="text-center"  v-if="couponShow">
                <span>优惠券-￥</span><span>{{coupon_price}}</span>
            </div>
            <a @click="paySuccess" class="weui-btn  weui-btn_org">
                <div class="payment_sure_text">确认支付  </div>
                <div class="payment_sure_price">￥{{payforInfo.orderPrice}}</div>
            </a>
        </div>
    </div>
</template>
<style>
.weui-cells_checkbox_payment .weui-check_payment:checked+.weui-icon-checked:before{
color:orangered;
}
    .payment_price{
        position: absolute;
        color: orangered;
        right: 15px;
    }
    .payment_nu{
        float: left;
        font-size: 18px;
        transform: scale(0.5,1.2);
        -ms-transform: scale(0.5,1.2);	/* IE 9 */
        -webkit-transform: scale(0.5,1.2);	/* Safari 和 Chrome */
        -o-transform: scale(0.5,1.2);	/* Opera */
        -moz-transform: scale(0.5,1.2);
        line-height: 1.57432;
    }
    .payment_count{
        float: left;
        font-size: 20px;
    }
    .payment_text{
        float: left;
        padding: 4px 0px 0px 0px;
        transform: scale(.7,0.8);
        -ms-transform: scale(.7,0.8);	/* IE 9 */
        -webkit-transform: scale(.7,0.8);	/* Safari 和 Chrome */
        -o-transform: scale(.7,0.8);	/* Opera */
        -moz-transform: scale(.7,0.8);
    }
    .payment_sure_text{
        float:left;
        width:50%;
        text-align:right;
    }
    .payment_sure_price{
        float:left;
        text-align:left;
         transform: scale(1.3,1.4);
        -ms-transform: scale(1.3,1.4);	/* IE 9 */
        -webkit-transform: scale(1.3,1.4);	/* Safari 和 Chrome */
        -o-transform: scale(1.3,1.4);	/* Opera */
        -moz-transform: scale(1.3,1.4);
        padding-left: 3%;
    }
</style>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    export default{
        data(){
            return{
                msg:'hello vue',
                couponShow:false,
                payforInfo:null,
                order_price:300,
                coupon_price:20,
                order_id:"1",
                loading:true
            }
        },
        created:function(){
            this.initClass();
        },
        methods:{
            initClass:function(){
            this.payforInfo=this.$store.state.orderstore.order;
             if(this.payforInfo){
                    this.loading=false
                }else{
                    alert("订单信息获取失败，请重新添加");
                }
           // this.classinfo=this.$route.params.validatedate;
            },
            selectYh:function(){
                if(this.toggle){
                    this.couponShow=true;
                    this.payforInfo.orderPrice=this.payforInfo.orderPrice-this.coupon_price;
                }else{
                    this.couponShow=false;
                    this.payforInfo.orderPrice=this.payforInfo.orderPrice+this.coupon_price;
                }
            },
            paySuccess:function(){
                var order={};
                if(this.payforInfo.orderId){
                    order["orderId"]=this.payforInfo.orderId;
                }else if(this.payforInfo.drivingSchoolId){
                    order["drivingSchoolId"]=this.payforInfo.drivingSchoolId;
                }
                order["orderPrice"]=this.payforInfo.orderPrice;
                order["coachId"]=this.payforInfo.coachId;
                order["payName"]=this.payforInfo.payName;
                order["payPhone"]=this.payforInfo.payPhone;
                order["payProvince"]=this.payforInfo.payProvince;
                order["payCity"]=this.payforInfo.payCity;
                order["payDistrict"]=this.payforInfo.payDistrict;
                order["userId"]=this.payforInfo.userId
                order["classtypeName"]=this.payforInfo.classtypeName;
                order["classtypeIntroduction"]=this.payforInfo.classtypeIntroduction;
                order["classtypePrice"]=this.payforInfo.classtypePrice;
                order["classtypeType"]=this.payforInfo.classtypeType;
                order["userOpenId"]=storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO).userOpenId;
                this.$http.post(types.HTTP_URL+'/orderTab/updateOrderPayed',order,{emulateJSON:true}).then(function(response){
                        console.log(response.data);
                        if(response.data.status==0){
                            this.dialogsInfo="支付失败"
                            var $iosDialog2 = $('#iosDialog2');
                            $iosDialog2.fadeIn(200);
                        }else if(response.data.status==1){//订单插入成功
                            var appid=response.data.appid;
                            var packageValue=response.data.packageValue;
                            var timeStamp=response.data.timeStamp;
                            var paySign=response.data.paySign;
                            var signType=response.data.signType;
                            var nonceStr=response.data.nonceStr;
                            var orderId=response.data.data;
                            WeixinJSBridge.invoke(
                                'getBrandWCPayRequest', {
                                    "appId" : appid,     //公众号名称，由商户传入
                                    "timeStamp":timeStamp,         //时间戳，自1970年以来的秒数
                                    "nonceStr" : nonceStr, //随机串
                                    "package" : packageValue,
                                    "signType" : "MD5",         //微信签名方式：
                                    "paySign" : signType //微信签名
                                },
                                function(res){
                                    if(res.err_msg == "get_brand_wcpay_request：ok" ) { // 使用以上方式判断前端返回,微信团队郑重提示：res.err_msg将在用户支付成功后返回    ok，但并不保证它绝对可靠。
                                        alert("支付成功")
                                        this.$router.push({name:"paySuccess",params:{order:order}});
                                    } else{
                                        alert("支付失败")
                                    }
                                }
                            );
                            //相关跳转
                        }
                    })

            }
        },
        components: {
            'loading':require('../components/loading.vue')
        }
    }
</script>
