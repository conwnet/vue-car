<template>
    <div>
        <div class="row topbanner text-center " >
            <div class="col-xs-12">
                订单详情
            </div>
        </div>
        <div class="row orderdss_back" style="margin-top:44px;">
            <div class="orderdss_back_orderdetail_text">
                <span>定金</span>
            </div>
            <div class="orderdss_back_orderdetail_number">
                <span>300</span>
            </div>
            <div >
                <span>由驾校人员点击确定</span>
            </div>
            <div class="text-center" style="padding-top:2%;">
                <i class="weui-icon-success" style="font-size:28px;"></i>
            </div>
            <div class="text-center" style="padding-top:1%;">
                <span class="paysuccess_text">支付成功</span>
            </div>
        </div>
        <component is="schoolTable"></component>
        <div class="row" style="background:transparent;">
            <div id="c1-list" style="background: #e8e7e7;">
                <div class="c1-list-item">
                    <div class="c1-list-item-line" >
                        <div class="flex-1">
                        </div>
                        <div class="flex-3">
                            <div>C1特惠班
                                <div class="reg hot">HOT</div>
                            </div>
                            <div class="blue">一费到底 车接车送</div>
                            <div class="reg">¥2799</div>
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
    .orderdss_back{
        background-image: url(../images/order_back.png);
        background-color: transparent;
        background-size: 100% 100%;
        height: 22.23213rem;
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
                    text-align: center;
                }
            }

        }
        &_item_1{
            flex: 2;
        }
    }

</style>
<script>
    export default{
        data(){
            return{
                msg:'hello vue'
            }
        },
        components:{
        "schoolTable": require('../components/schoolTable.vue')
        }
    }
</script>
