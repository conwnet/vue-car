<style>
    .pay-footer {
        position: fixed;
        left: 0;
        bottom: 0px;
        height: 80px;
        background-color: #ff4200;
        color: white;
        width: 100%;
        text-align: center;
        padding: 0 15px;
        display: flex;
        z-index: 500;
        white-space: nowrap;
    }
    /*.pay-footer .flex1{*/
        /*flex: 1;*/
    /*}*/
    /*.pay-footer .flex2{*/
        /*flex: 2;*/
    /*}*/
    /*.pay-footer .flex3{*/
        /*flex: 3;*/
    /*}*/
    .pay-footer .span-big{
        font-size: 4rem;
        color: white;
        background-color: #ff4200;
    }
    .pay-footer .span-small{
        font-size: 1.5rem;
        color: white;
        background-color: #ff4200;

    }
    .pay-footer .but {
        color: #ff4200;
        background-color: white;
        border-radius: 3px;
        font-size: 1.5rem;
    }
    .pay-footer span{
        padding: 5px;
    }
</style>
<template>
    <div>
        <div class="pay-footer">
            <div style="margin: auto;"><span class="span-small">¥</span><span class="span-big">{{payfooter.classtypePrice}}</span><span class="span-small">定金100顶300</span><span class="but" v-if="role_type!=2" @click="judgmentLogin(payfooter)">立即支付</span></div>
            <!--<div class="inner">¥2799定金100顶300</div><div class="but">立即支付</div>-->
        </div>
    </div>
</template>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    export default{
        data(){
            return {
            role_type:null
            }
        },
        props:["payfooter","drivingSchoolId","drivingSchoolName","drivingSchoolScore","deposit"],
        created:function(){
            this.isShow();
        },
        methods:{
            isShow:function(){
                if(storage.getStrLocalStorageItem(types.CACHE_ROLE_STATUS) == 1)
                {
                    this.role_type=2;
                }
            },
            judgmentLogin:function(item){
                if(storage.getStrLocalStorageItem(types.CACHE_STUDENT_INFO)){
                        this.addOrder(item);
                }else{
                    this.routerPush();
                }
            },
            routerPush:function(){
                    types.ADD_ROUTER_PATH("/schoolInfoPage","schoolInfoPage",this.drivingSchoolId);
                    this.$router.push({path:"/register"});
            },
            addOrder:function(classtype){
                var user=storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)
                var order={};
                order["orderPrice"]=300;
                order["drivingSchoolId"]=this.drivingSchoolId;
                order["orderStatue"]=0;
                order["classtypeName"]=classtype.classtypeName;
                order["classtypeIntroduction"]=classtype.classtypeIntroduction;
                order["classtypePrice"]=classtype.classtypePrice;
                order["classtypeType"]=classtype.classType;
                order["payforName"]=this.drivingSchoolName;
                order["coachScore"]=this.drivingSchoolScore;
                order["deposit"]=this.deposit;
                order["payType"]=1;//0教练 1驾校
                order["user"]=user;
                this.$store.dispatch('add_order',order);
                this.$router.push({path:"/payValidate"});
            },
        }
    }
</script>