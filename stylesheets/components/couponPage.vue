<style>
    .coupon-number-line {
        background-color: #e5e4e9;
        padding-top: 15px;
        padding-bottom: 15px;
    }

    .coupon-number-line .confirm-but {
        color: white;
        margin-left: 15px;
        border: 0;
    }

    .coupon-number-line .coupon-entry-line {
        vertical-align: middle;
        padding-top: 15px;
    }

    .clean_padding {
        padding: 0;
    }

    .coupon-city-title {
        margin-right: -15px;
        margin-left: -15px;
        padding: 15px;
    }

    .coupon-model {
        margin: 10px 0;
        border: solid 1px #dad9de;
        border-radius: 5px;
        box-shadow: 0 0 3px #DAD9DE;
        padding-top: 15px;
    }

    .coupon-content {
        font-size: 2.5rem;
        font-weight: bold;

    }

    .coupon-content div {
        display: inline-block;
    }

    .coupon-content .coupon_number {
        font-size: 1rem;
        color: #9e9e9e;
        margin-left: 5px;
    }

    .coupon-model .coupon-content-list {
        border-bottom: dashed 1px #dad9de;
        padding-left: 10px;
    }

    .school-name .is_use, .school-name .s-name {
        display: inline-block;
        width: 48%;
    }

    .school-name .is_use {
        text-align: right;
    }

    .school-name .is_use .is_use_but {
        color: #ffcc3b;
        border: solid 1px #ffcc3b;
        border-radius: 20px;
        padding: 0 10px;
    }

    .coupon-banner {
        padding: 5px 0 5px 10px;
    }

    .coupon-banner div {
        display: inline-block;
        color: #dad9de;
        width: 48%;
    }
    .order-city-coupon{
        margin-right: -15px;
        margin-left: -15px;
        padding: 15px;
        text-align: center;
        text-decoration: none;
    }
</style>
<template>
    <div>
        <div class="row page-title">优惠券</div>
        <div class="row coupon-number-line">
            <div class="col-xs-12">手动输入优惠券</div>
            <div class="col-xs-12 coupon-entry-line">
                <div class="col-xs-7 clean_padding"><input class="form-control" type="text" placeholder="请输入优惠券的券码">
                </div>
                <div class="col-xs-5 clean_padding">
                    <div class="weui-btn_org btn confirm-but">确定</div>
                </div>
            </div>
        </div>
        <div class="coupon-city-title">沈阳市</div>
        <div class="row coupon-model" v-for="item in couponList">
            <div class="col-xs-12">
                <div class="col-xs-2 clean_padding">
                    <img v-if="item.is_use == 1" class="img-responsive"
                         src="../images/coupon-icon-1.png" alt="">
                    <img v-else class="img-responsive"
                         src="../images/coupon-icon-2.png" alt=""></div>
                <div class="col-xs-10 clean_padding coupon-content-list">
                    <div class="school-name">
                        <div class="s-name">{{item.school_name}}</div>
                        <div class="is_use" v-if="item.is_use == 1">
                            <div class="is_use_but btn">可使用</div>
                        </div>
                    </div>
                    <div class="coupon-content">
                        <div>{{item.content}}</div>
                        <div v-if="item.coupon_number > 1" class="coupon_number">x{{item.coupon_number}}</div>
                    </div>
                </div>

                <div class="col-xs-10 col-xs-offset-2 coupon-banner clean_padding">
                    <div style="text-align: left;">{{item.app_name}}</div>
                    <div style="text-align: right;">{{item.area}}</div>
                </div>
            </div>
        </div>
        <div class="order-city-coupon"><a href="#">查看其他城市优惠券</a></div>
    </div>
</template>
<script>
    module.exports = {
        data: function () {
            return {
                couponList: [{
                    school_name: "欣祥和驾校",
                    is_use: 1,
                    content: "0.1元代金券",
                    app_name: "约学车平台",
                    coupon_number: 1,
                    area: "大东区",
                    image: "url"
                }, {
                    school_name: "欣祥和驾校",
                    is_use: 0,
                    content: "减20元",
                    app_name: "约学车平台",
                    coupon_number: 2,
                    area: "沈阳市",
                    image: "url"
                }]
            }
        }
    }
</script>