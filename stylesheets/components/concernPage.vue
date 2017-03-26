<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
        <div class="row concern-header-line">
            <div style="margin-top: 9px;">
                <strong>订单</strong>
            </div>
        </div>
        <div id="allOrder" style="height:50%;">
            <div class="concern-font"><strong>全部订单</strong></div>
            <div class="row concern-share-row" v-for="order in orderListPage">
                <div class="col-xs-12"  @click="goSuccessOrderInfo(order.orderId)">
                    <div class="row concern-share-row" style="padding-top: 6px;padding-bottom: 6px;">
                        <img :src="img_url+order.studentsOrder.userPhoto" class="concern-photo-two">&nbsp;&nbsp;&nbsp;<strong>{{getdecodeUTF8(order.studentsOrder.userName)}}</strong>
                    </div>
                    <div class="row class-share-row">
                        <div><strong>{{order.classtypeName}}</strong></div>
                        <div style="color: #00a2ea;font-size: 9px">{{order.classtypeIntroduction}}</div>
                        <div style="color: #fb7572;font-size: 9px">¥{{order.classtypePrice}}</div>
                    </div>
                    <div class="row concern-share-row" style="text-align: right;color:#ff6531;font-size:10px;padding-right:15px;">
                        定金¥{{order.orderPrice}}&nbsp;已成交
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
        <div id="allConcern">
            <div class="concern-font"><strong>全部关注</strong></div>
                <div v-for="concernstu in concernPageList">
                    <div class="row" v-for="item in concernstu.students" id="concernDocument">
                        <div class="col-xs-12">
                            <div class="col-xs-2" style="padding: 5px 0px 0px 0px;">
                                <img :src="img_url+item.userPhoto" class="concern-photo">
                            </div>
                            <div class="col-xs-7" style="padding: 13px 0px 0px 20px;">
                                <strong>{{getdecodeUTF8(item.userName)}}</strong>
                            </div>
                            <div class="col-xs-3" style="text-align: center;margin-top: 12px;">
                                <div>
                                    <img src="../images/already-concern.png" class="concern-img">
                                </div>
                                <div class="concern-already-font">已关注</div>
                            </div>
                        </div>
                    </div>
                </div>
            <div class="row">
                <div class="col-xs-12" style="height:40px;text-align: center;" @click="concernnextPage">
                    <div v-if="concernshowmore" style="padding-top: 8px;">
                        <span>点击加载更多</span>
                    </div>
                    <div v-if="concernshowPage">
                        <span>加载中...</span><img  src="../images/loadingPage.gif" style="height: 40px;width: 40px;">
                    </div>
                    <div v-if="concernshowNull" style="padding-top: 8px;">
                        <span>无更多信息</span>
                    </div>
                </div>
            </div>
        </div>
        </div>
    </div>
</template>
<style>
    .concern-header-line{
       height:44px;
       text-align:center;
       background-color: #29a1f7;
       color:#ffffff;
    }

    .concern-font{
       color:#828282;
    }

    .concern-share-row{
        padding-left:0px;
        padding-right:0px;
    }

    .class-share-row{
        background-color: #f1f1f1;
        padding-left: 20px;
    }
    .concern-img{
        width: 30px;
    }
    .concern-photo-two{
        border-radius: 50%;
        width: 40px;
        height: 40px;
        margin-left: 20px;
    }
    .concern-photo{
        border-radius: 50%;
        width: 60px;
        height: 60px;
    }

    .concern-already-font{
        color:#333333;
        font-size:9px;
    }

    .concern-dull{
        font-size: 12px;
        color: #43c002;
        text-align: center;
        padding-top: 14px;
    }
</style>
<script>
import * as types from '../store/mutation-types'
    export default{
        data(){
            return{
                loading: true,
                concernPageList:[],
                orderListPage:[],
                coachId:null,
                UserName:[],
                orderisDataNull:false,
                concernisDataNull:false,
                orderpageInt:1,
                coachpageInt:1,
                ordershowPage:false,
                ordershowmore:true,
                concernshowmore:true,
                concernshowPage:false,
                ordershowNull:false,
                concernshowNull:false,
                img_url:types.IMG_URL
            }
        },
         created:function(){
            this.coachId = this.$route.params.coachId;
            this.getConcernByCoachId();
            this.getOrderByCoachId();
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
                this.getOrderByCoachId();
            },
            concernnextPage:function(){
                if(this.concernisDataNull){return;}
                this.coachpageInt++;
                this.concernshowPage=true;
                this.concernshowmore=false;
                this.concernshowNull = false;
                this.getConcernByCoachId();
            },
            getConcernByCoachId:function(){
                this.$http.post(types.HTTP_URL+'/concern/getAllConcernByCoachId',{pageIndex:this.coachpageInt,coachId:this.$route.params.coachId},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                            var concernList = response.data.list;
                            this.concernPageList=this.concernPageList.concat(concernList);
                            this.concernshowPage=false;
                            this.concernshowmore=true;
                            this.concernshowNull = false;
                            this.loading = false;
                        }else{
                            this.concernisDataNull=true;
                            this.concernshowPage=false;
                            this.concernshowmore=false;
                            this.concernshowNull = true;
                            this.loading = false;
                        }
                 })
             },

             getOrderByCoachId:function(){
                this.$http.post(types.HTTP_URL+'/orderTab/getAllOrderByCoachId',{pageIndex:this.orderpageInt,coachId:this.$route.params.coachId},
                    {emulateJSON:true}).then(function(response){
                        if(response.data.status==1){
                            var orderList = response.data.list;
                            this.orderListPage=this.orderListPage.concat(orderList);
                            console.log(this.orderListPage);
                            this.ordershowPage=false;
                            this.ordershowmore=true;
                            this.ordershowNull=false;
                            this.loading = false;
                        }else{
                            this.orderisDataNull=true;
                            this.ordershowPage=false;
                            this.ordershowmore=false;
                            this.ordershowNull=true;
                            this.loading = false;
                        }
                 })
             },
             getdecodeUTF8:function(arr){
                  return decodeURI(arr,"utf-8");
             },
             goSuccessOrderInfo:function(id){
                 this.$router.push({path:"/myOrderForCoach",name:"myOrderForCoach",params: { orderId: id}});
             }
        }
    }
</script>
