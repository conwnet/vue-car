<template>
    <div>
        <div class="row">
            <div class="col-xs-12">
                <div class="weui-grids">
                            <a href="javascript:;" class="weui-grid weui_grid_padding">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-bmlc.png" alt="">
                                </div>
                                <p class="weui-grid__label">报名流程</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding" @click="jumpTagPage(2)">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-bmgl.png" alt="">
                                </div>
                                <p class="weui-grid__label">报名攻略</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-cjwt.png" alt="">
                                </div>
                                <p class="weui-grid__label">常见问题</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding" @click="jumpTagPage(4)">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-yhq.png" alt="">
                                </div>
                                <p class="weui-grid__label">优惠券</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-bgx.png" alt="">
                                </div>
                                <p class="weui-grid__label">保过险</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-fqxc.png" alt="">
                                </div>
                                <p class="weui-grid__label">分期学车</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-jfsc.png" alt="">
                                </div>
                                <p class="weui-grid__label">积分商城</p>
                            </a>
                            <a href="javascript:;" class="weui-grid weui_grid_padding">
                                <div class="weui-grid__icon">
                                    <img src="../images/index-jlzj.png" alt="">
                                </div>
                                <p class="weui-grid__label">教练之家</p>
                            </a>
                        </div>
                </div>
        </div>
        <div class="row">
            <div class="col-xs-12 height-line">

            </div>
        </div>
    </div>


</template>
<style>
    .weui_grid_padding{
     padding:20px 7px;
    }
    .weui-grid{
        width:25%;
    }
    .weui-grids:after{
    border-left:none
    }
    .weui-grid:before{
    border-right:none
    }
</style>
<script>
require("../css/common.css")
    export default{
        data(){
            return{
                msg:'hello vue'
            }
        },
        methods:{
            jumpTagPage:function(i){
                switch (i){
                    case 1:
                        break;
                    case 2:
                        this.$router.push("/enrollStrategyPage");
                        break;
                    case 3:
                        break;
                    case 4:
                        this.$router.push("/couponPage");
                        break;
                }
            }
        }

    }
</script>
