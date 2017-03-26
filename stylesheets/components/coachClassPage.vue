<style>
    .class-line {
        border-top: solid 5px #f0f0f0;
        border-bottom: solid 1px #f0f0f0;
        padding-top: 5px;
        padding-bottom: 5px;
        position: relative;
    }

    .class-line-title {
        padding: 0 15px;
        text-align: left;
    }

    .class-line .class-line-tag {
        display: flex;
        text-align: center;
        padding: 10px;
        border-bottom: solid 1px #f0f0f0;
    }

    .class-line .class-line-tag .tag-flex {
        width: 33%;
    }

    .class-line .class-line-tag .tag-flex img {
        margin-left: 5px;
        width: 15%;
    }

    .c1-list-item .c1-list-item-line {
        padding: 15px;
        display: flex;
    }

    .c1-list-item .c1-list-item-line .flex-1 {
        flex: 1;
        margin: auto;
    }

    .c1-list-item .c1-list-item-line .flex-3 {
        flex: 3;
    }

    .c1-list-item .c1-list-item-line .flex-3 .hot {
        display: inline-block;
        vertical-align: top;
        font-size: .1rem;
    }

    .c1-list-item .c1-list-item-line .flex-3 .blue {
        color: #009fe9;
    }

    .c1-list-item .c1-list-item-line .flex-3 .reg {
        color: #fa4645;
    }

    .c1-list-item .c1-list-item-line .enroll-but {
        color: #009fe9;
        border: solid 2px #009fe9;
        text-align: center;
        padding: 5px;
        white-space: nowrap;
    }

    .bubble-arrow-c1-list {
        border-color: transparent transparent #f0f0f0 transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 50px;
        left: 16%;
    }

    .bubble-arrow-inner-c1-list {
        border-color: transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 52px;
        left: 16%;
    }

    .bubble-arrow-b1-list {
        border-color: transparent transparent #f0f0f0 transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 50px;
        left: 47%;
    }

    .bubble-arrow-inner-b1-list {
        border-color: transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 52px;
        left: 47%;
    }

    .bubble-arrow-vip-list {
        border-color: transparent transparent #f0f0f0 transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 50px;
        left: 78%;
    }

    .bubble-arrow-inner-vip-list {
        border-color: transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 52px;
        left: 78%;
    }
    .b1-list,.vip-list{
        display: none;
    }
</style>
<template>
    <div class="row class-line">
        <div class="class-line-title">班型简介</div>
        <div class="class-line-tag">
            <div class="tag-flex" @click="clickClassTag('c1-list')">C1<img src="../images/arrow-down-icon.png">
            </div>
            <div class="tag-flex" @click="clickClassTag('b1-list')">B1<img src="../images/arrow-down-icon.png"></div>
            <div class="tag-flex" @click="clickClassTag('vip-list')">VIP<img src="../images/arrow-down-icon.png"></div>
        </div>
        <div class="class-list-item c1-list">
            <div id="c1-list">
                <div class="c1-list-item">
                    <div v-if="ClassTypes.c1List.length===0" style="height: 50px;text-align: center;padding-top: 17px;">
                        <span>无班型信息</span>
                    </div>
                    <div v-else>
                        <div class="c1-list-item-line" v-for="item in ClassTypes.c1List">
                            <div class="flex-3">
                                <div>{{item.classtypeName}}
                                    <div class="reg hot">HOT</div>
                                </div>
                                <div class="blue">{{item.classtypeIntroduction}}</div>
                                <div class="reg">¥{{item.classtypePrice}}</div>
                            </div>
                            <div class="flex-1">
                                <div class="enroll-but" v-if="role_type!=2" @click="judgmentLogin(item)">立即报名</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="class-list-item b1-list">
            <div id="b1-list">
                <div class="c1-list-item">
                    <div v-if="ClassTypes.b1List.length===0" style="height: 50px;text-align: center;padding-top: 17px;">
                        <span>无班型信息</span>
                    </div>
                    <div v-else>
                        <div class="c1-list-item-line" v-for="item in ClassTypes.b1List">
                            <div class="flex-3">
                                <div>{{item.classtypeName}}
                                    <div class="reg hot">HOT</div>
                                </div>
                                <div class="blue">{{item.classtypeIntroduction}}</div>
                                <div class="reg">¥{{item.classtypePrice}}</div>
                            </div>
                            <div class="flex-1">
                                <div class="enroll-but" v-if="role_type!=2"  @click="judgmentLogin(item)">立即报名</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>

        <div class="class-list-item vip-list">
            <div id="vip-list">
                <div class="c1-list-item">
                    <div v-if="ClassTypes.vipList.length===0"  style="height: 50px;text-align: center;padding-top: 17px;">
                        <span>无班型信息</span>
                    </div>
                    <div v-else>
                        <div class="c1-list-item-line" v-for="item in ClassTypes.vipList">
                            <div class="flex-3">
                                <div>{{item.classtypeName}}
                                    <div class="reg hot">HOT</div>
                                </div>
                                <div class="blue">{{item.classtypeIntroduction}}</div>
                                <div class="reg">¥{{item.classtypePrice}}</div>
                            </div>
                            <div class="flex-1">
                                <div class="enroll-but" v-if="role_type!=2"  @click="judgmentLogin(item)">立即报名</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div id="school-class-arrow" class="bubble-arrow-c1-list"></div>
        <div id="school-class-inner" class="bubble-arrow-inner-c1-list"></div>
    </div>
</template>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    module.exports = {
        data: function () {
            return {
                previousClassTag : 'c1-list',
                role_type:null
            }
        },
        created:function(){
            this.isShow();
        },
        props:["ClassTypes","coachId","coachName","coachScore","deposit"],
        methods: {
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
                    types.ADD_ROUTER_PATH("/coachInfoPage","coachInfoPage",this.coachId);
                    this.$router.push({path:"/register"});
            },
            addOrder:function(classtype){
                var user=storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO)
                var order={};
                order["coachId"]=this.coachId;
                order["orderStatue"]=0;
                order["classtypeName"]=classtype.classtypeName;
                order["classtypeIntroduction"]=classtype.classtypeIntroduction;
                order["classtypePrice"]=classtype.classtypePrice;
                order["classtypeType"]=classtype.classType;
                order["payforName"]=this.coachName;
                order["coachScore"]=this.coachScore;
                order["deposit"]=this.deposit;
                order["payType"]=0;//0教练 1驾校
                order["user"]=user;
                this.$store.dispatch('add_order',order);
                this.$router.push({path:"/payValidate"});
            },
            clickClassTag: function (id) {
                if (this.previousClassTag != "" && this.previousClassTag != id) {
                    $("." + this.previousClassTag).hide();
                    $("#school-class-arrow").removeClass("bubble-arrow-" + this.previousClassTag);
                    $("#school-class-inner").removeClass("bubble-arrow-inner-" + this.previousClassTag);
                }
                $("#school-class-arrow").toggleClass("bubble-arrow-" + id);
                $("#school-class-inner").toggleClass("bubble-arrow-inner-" + id);
                $("." + id).toggle();
                this.previousClassTag = id;
            }
        }
    }
</script>