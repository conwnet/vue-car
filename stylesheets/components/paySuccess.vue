<template>
    <div>
        <div class="row topbanner text-center " >
            <div class="col-xs-12">
                支付成功
            </div>
        </div>
        <div class="text-center" style="padding-top:120px;">
        <i class="weui-icon-success" style="font-size:40px;"></i>
        </div>
        <div class="text-center" style="padding-top:15px;">
            <span class="paysuccess_text">支付成功</span>
        </div>
        <a @click="orderDetail" class="weui-btn  weui-btn_org">订单详情
        </a>
    </div>
</template>
<style>
    .paysuccess_text{
    font-weight:700;
    color:#09bb07;
    font-size: 22px;
    }
</style>
<script>
    export default{
        data(){
            return{
                msg:'hello vue'
            }
        },
        methods:{
            orderDetail:function(){
                this.$router.push({name:"orderDetail",params:{order:this.$route.params.order}});
            }
        },
        components:{
        }
    }
</script>
