<template>
    <div>
        <loading v-if="loading" ></loading>
        <div v-if="!loading">
            <div class="row topbanner text-center " >
                <div class="col-xs-12">
                    订单详情
                </div>
            </div>
            <div class="row orderdetail_back" style="margin-top:44px;">
                <div class="orderdetail_back_orderdetail_text">
                    <span>定金</span>
                </div>
                <div class="orderdetail_back_orderdetail_number">
                    <span>{{order.orderPrice}}</span>
                </div>
                <div >
                    <span>由驾校人员点击确定</span>
                </div>
                <div class="orderdetail_back_orderdetail_ts">
                    温馨提示: 学员误点
                </div>
                <a @click="divingSchool" class="weui-btn  weui-btn_org">驾校确认
                </a>
            </div>
        </div>
    </div>
</template>
<style lang="less">
    @base_order_color: orangered;
    @red_order_color:red;
    .orderdetail_back{
         background-image: url(../images/order_back.png);
         background-color: transparent;
         background-size: 100% 100%;
         height: 23.23213rem;
         div{
             position: relative;
             text-align: center;
             color:@base_order_color;
         }
         a{
             margin-top: 4%;
             width: 70%;
         }
         &_orderdetail_text{
             margin-top: 8%;
             padding: 1.2rem 0px 0px 0px;
         }
         &_orderdetail_number{
            font-size:2.1em;
         }
         &_orderdetail_ts{
             margin-top:3%;
             color:@red_order_color !important;
         }
    }

</style>
<script>
import * as types from "../store/mutation-types"
    export default{
        data(){
            return{
                loading:true,
                order:null
            }
        },
        created:function(){
            this.getOrderDetail();
        },
        methods:{
          getOrderDetail:function(){
            this.$http.post(types.HTTP_URL+'/orderTab/selectOrderById',{orderId:this.$route.params.order},{emulateJSON:true}).then(function(response){
                        if(response.data.status==0){
                            alert("订单数据异常，请返回订单列表查看")
                        }else if(response.data.status==1){//订单查询
                            this.order=response.data.data
                            console.log(this.order);
                            this.$store.dispatch('add_order',this.order);
                            this.loading=false;
                            }
                            //相关跳转
            })
          },
          divingSchool:function(){
            this.$router.push({name:"orderDrivingSchool"});
          }
        },
        components:{
            'loading':require('../components/loading.vue')
        }
    }
</script>
