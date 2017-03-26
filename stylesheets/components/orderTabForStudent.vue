<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
        <div class="row concern-header-line">
            <div style="margin-top: 9px;">
                <strong>学员订单</strong>
            </div>
        </div>
            <div class="row concern-share-row" v-for="order in orderListPage"  @click="goOrderInfo(order)">
                <div class="col-xs-12">
                    <div v-if="order.coach==null" class="row concern-share-row" style="padding-top: 6px;padding-bottom: 6px;padding-left: 20px;">
                        <strong>{{order.drivingSchool.drivingSchoolName}}</strong>
                    </div>
                    <div v-else class="row concern-share-row" style="padding-top: 6px;padding-bottom: 6px;padding-left: 20px;">
                        <strong>{{order.coach.coachName}}</strong>
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
                <div>
                    <div class="col-xs-12" style="height:5px;background: #f1f1f1;">

                    </div>
                </div>

            </div>
        <div class="row">
            <div class="col-xs-12" style="height:40px;text-align: center;" @click="ordernextPage">
                <div v-if="ordershowmore"style="padding-top: 8px;">
                    <span>点击加载更多</span>
                </div>
                <div v-if="ordershowPage">
                    <span>加载中...</span><img  src="../images/loadingPage.gif" style="height: 40px;width: 40px;">
                </div>
                <div v-if="ordershowNull" style="padding-top: 8px;">
                    <span>无更多信息</span>
                </div>
            </div>
        </div>
    </div>
</template>
<style>

</style>
<script>
    import * as types from '../store/mutation-types'
    export default{
        data(){
            return{
                orderListPage:[],
                orderisDataNull:false,
                orderpageInt:1,
                ordershowPage:false,
                ordershowmore:true,
                ordershowNull:false,
                loading: true,
            }
        },
        created:function(){
            this.getOrderByUserId();
        },
        components:{
             'loading':require('../components/loading.vue')
        },
        methods:{
            ordernextPage:function(){
                if(this.orderisDataNull){return;}
                this.orderpageInt++;
                this.ordershowPage=true;
                this.ordershowmore=false;
                this.ordershowNull=false,
                this.getOrderByUserId();
            },
             getOrderByUserId:function(){
                this.$http.post(types.HTTP_URL+'/orderTab/getUserOrderList',{pageIndex:this.orderpageInt,userId:this.$route.params.userId},
                    {emulateJSON:true}).then(function(response){
                        console.log(response.data.list);
                        if(response.data.state==1){
                            var order = response.data.list;
                            this.orderListPage=this.orderListPage.concat(order);
                            this.ordershowPage=false;
                            this.ordershowmore=true;
                            this.ordershowNull=false;
                            this.loading = false;
                        }else{
                            this.orderisDataNull = true;
                            this.ordershowPage=false;
                            this.ordershowmore=false;
                            this.ordershowNull=true;
                            this.loading = false;
                        }
                 })
             },
             goOrderInfo:function(order){
                this.$router.push({path:"/studentOrderInfo",name:"studentOrderInfo",params:{orderId:order.orderId}});
             },
        }
    }
</script>
