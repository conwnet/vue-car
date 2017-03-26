<template>
    <div>
        <div class="row topbanner text-center " >
            <div class="col-xs-12">
                确认支付
            </div>
        </div>
        <div class="row" style="margin-top:44px;">
            <div class="weui-cells  coach-cells" style="margin-top:0px;">
                <div class="weui-cell">
                    <div class="weui-cell__hd">
                        <label class="weui-label" style="font-size:17px;">C1特惠班</label>
                        <div class="base-color" style="float:left;">
                            一费到底 自行前往 等等福利
                        </div>
                        <div class="payment_price">
                            <span>￥</span>2799
                        </div>
                        <div class="weui-cell__hd_class_price">
                            <div class="payment_nu">￥</div>
                            <div class="payment_count">100</div>
                            <div class="payment_text">定金</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row" >
            <div class="weui-cells coach-cells weui-cells_checkbox weui-cells_checkbox_payment">
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
        <div class="row margintop">
        </div>
        <div class="text-center" style="padding-top:12px;">
            <img src="../images/payfail.png" style="width:10%;">
        </div>
        <div class="text-center" style="padding-top:15px;">
            <span class="paysuccess_text" style="color:orangered;">支付失败</span>
        </div>
        <a href="javascript:;" class="weui-btn  weui-btn_org">
            <div class="payment_sure_text">重新支付  </div>
            <div class="payment_sure_price">￥100</div>
        </a>
    </div>
</template>
<style>
</style>
<script>
     export default{
        data(){
            return{
                msg:'hello vue'
            }
        },
        components:{
        }
    }
</script>
