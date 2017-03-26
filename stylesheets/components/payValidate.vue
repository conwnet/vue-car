<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="row topbanner text-center " >
                <div class="col-xs-2" v-on:click="goback">
                    X
                </div>
                <div class="col-xs-9">
                    支付
                </div>
            </div>
            <div class="row margintop" style="margin-top:44px;">
                <div class="weui-cells  coach-cells" style="margin-top: 0px;">
                    <div class="weui-cell">
                        <div class="weui-cell__hd"><label class="weui-label">姓名</label></div>
                        <div class=" weui-cell__bd">
                            <input class="weui-input" type="text" v-model="payName" placeholder="输入你的姓名">
                        </div>
                    </div>
                    <div class="weui-cell">
                        <div class="weui-cell__hd"><label class="weui-label">手机</label></div>
                        <div class=" weui-cell__bd">
                            <input class="weui-input" type="number" v-model="validatePhone"  placeholder="输入你的手机号码">
                        </div>
                    </div>
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">验证码</label></div>
                        <div class=" weui-cell__bd">
                            <input class="weui-input" type="number" v-model="validateCode" placeholder="请输入验证码">
                        </div>
                        <div class=" weui-cell__ft">
                            <a @click="validatePhoneNumber"  class="weui-btn weui-btn_mini weui-btn_default">
                                <span v-show="validatesec>0">({{validatesec}}s)</span>获取验证码
                            </a>
                        </div>
                    </div>
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label" style="width: 80px;">城市</label></div>
                        <div class="weui-cell__bd weui-cell__ft">
                            <select v-model='pvselect' class="weui-select" name="select1">
                                <option  v-for="pl in coachCodes.provinceList" :value="pl">{{pl.name}}</option>
                            </select>
                        </div>
                        <div class="weui-cell__bd weui-cell__ft">
                            <select v-model='ctselect' class="weui-select" name="select2">
                                <option  v-for="cl in bindctselects" :value="cl">{{cl.name}}</option>
                            </select>
                        </div>
                        <div class="weui-cell__bd weui-cell__ft">
                            <select v-model='dtselect' class="weui-select" name="select3">
                                <option  v-for="dl in binddtselects" :value="dl">{{dl.name}}</option>
                            </select>
                        </div>
                    </div>
                </div>
            </div>
            <div class="row margintop" style="margin-top:10px;">
                <div class="weui-cells  coach-cells">
                    <div class="weui-cell">
                        <div class="weui-cell__hd"><label class="weui-label" >班型介绍</label></div>
                        <div class=" weui-cell__bd">
                        </div>
                    </div>
                    <div class="weui-cell">
                        <div class="weui-cell__hd">
                            <label class="weui-label" style="font-size:20px;">{{payforInfo.payforName}}</label>
                            <div class="payvali_color">
                                <img v-for="start in payforInfo.coachScore" src="../images/lightstar.png" alt=""
                                     class="star-icon"><img
                                    v-for="start in 5-payforInfo.coachScore" src="../images/graystar.png" alt=""
                                    class="star-icon">
                            </div>
                            <div class="weui-cell__hd_class_name">
                                {{payforInfo.classtypeName}}
                            </div>
                            <div class="base-color">
                                {{payforInfo.classtypeIntroduction}}
                            </div>
                            <div class="weui-cell__hd_class_price">
                                <div class="weui-cell__hd_class_price_price_symbol">￥</div>
                                <div class="weui-cell__hd_class_price_number">{{payforInfo.classtypePrice}}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <a  class="weui-btn  weui-btn_blue margintop" @click="paymentConfirm">确认支付</a>
            <div id="dialogs">
                <div class="js_dialog" id="iosDialog2" style="display: none;">
                    <div class="weui-mask"></div>
                    <div class="weui-dialog">
                        <div class="weui-dialog__bd">{{dialogsInfo}}</div>
                        <div class="weui-dialog__ft">
                            <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>

    </div>
</template>
<style lang="less">
    .weui-cell__hd{
        &_class_name{
            font-size:17px;
            font-weight: 700;
        }
        &_class_price{
            font-size:17px;
            font-weight: 700;
            color:orangered;
            &_price_symbol{
                float: left;
                transform: scale(.5,.5);
                -ms-transform: scale(.5,.5);	/* IE 9 */
                -webkit-transform: scale(.5,.5);	/* Safari 和 Chrome */
                -o-transform: scale(.5,.5);	/* Opera */
                -moz-transform: scale(.5,.5);
            }
            &_number{
                float:left;
            }
        }
    }
    .payvali_color{
     color:#aaa9a9;
    }
</style>
<script>
import * as types from "../store/mutation-types"
import * as storage from "../store/localStorage"
    export default{
        data(){
            return{
                postValidateCode:"",
                validateCode:"",
                validatePhone:"",
                validateres:false,
                validatesec:0,
                Interval:null,
                payforInfo:null,
                loading:true,
                payName:null,
                pvselect:null,
                ctselect:null,
                dtselect:null,
                bindctselects:[],
                binddtselects:[],
                coachCodes:null,
                dialogsInfo:"请输入正确格式的手机号"
            }
        },
        created:function(){
            this.initView();
        },
        watch:{
            pvselect:function(){
                this.bindctselects=[];
                for(var i=0;i<this.coachCodes.cityList.length;i++){
                 if(this.pvselect.id == this.coachCodes.cityList[i].pid){
                    this.bindctselects.push(this.coachCodes.cityList[i]);
                 }
                }
                this.binddtselects=[];
                return this.bindctselects;
            },
            ctselect:function(){
                this.binddtselects=[];
                for(var i=0;i<this.coachCodes.districtList.length;i++){
                 if(this.ctselect.id == this.coachCodes.districtList[i].pid){
                    this.binddtselects.push(this.coachCodes.districtList[i]);
                 }
                }
                return this.binddtselects;
            }
        },
        methods:{
            initView:function(){
                this.payforInfo=this.$store.state.orderstore.order;
                if(this.payforInfo){
                        this.getCoachCodeList();
                }else{
                    alert("订单信息获取失败，请重新添加");
                }
            },
            getCoachCodeList:function(){
                this.$http.post(types.HTTP_URL+'/coach/selectRegisterCoachWithCode',{},{emulateJSON:true}).then(function(response){
                        if(response.data.status==0){
                            alert("this data  is null");
                        }else{//未注册
                            this.coachCodes=response.data;
                            this.loading=false
                        }
                    })
            },
            getPhoneCode:function(userPhone){
                    this.$http.post(types.HTTP_URL+'/sendsms/sendTempSMS',{userPhone:userPhone},{emulateJSON:true}).then(function(response){
                                if(response.data.state==1){
                                    this.postValidateCode=response.data.code;
                                }
                        })
            },
            goback:function(){
                this.$router.go(-1);
            },
            commitValidate:function(){
                if(!this.payName){
                    this.dialogsInfo="请输入姓名"
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
                if(!this.pvselect){
                    this.dialogsInfo="请选择省份"
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
               if(!this.ctselect){
                    this.dialogsInfo="请选择城市"
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
                if(!this.dtselect){
                    this.dialogsInfo="请选择区域"
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
              // if(!this.validateres){
              //          this.dialogsInfo="请获取手机验证码!";
              //           var $iosDialog2 = $('#iosDialog2');
              //           $iosDialog2.fadeIn(200);
              //           return false;
              // }
              // if(!this.validateCode){
              //           this.dialogsInfo="请填写六位验证码!";
              //           var $iosDialog2 = $('#iosDialog2');
              //           $iosDialog2.fadeIn(200);
              //           return false ;
              // }
              // if(this.validateCode!=this.postValidateCode){
              //          this.dialogsInfo="验证码有误，请仔细核对!";
               //          var $iosDialog2 = $('#iosDialog2');
               //          $iosDialog2.fadeIn(200);
               //          return false;
              // }
                return true;
            },
            updatesec:function(){
                if(this.validatesec==0)
                {
                    this.validatesec=60
                }else{
                    this.validatesec--;
                    console.log(this.validatesec);
                    if(this.validatesec==0){
                        clearInterval(this.Interval);
                    }
                }
            },
            validatePhoneNumber:function(){
                if( !(/^1[34578]\d{9}$/.test(this.validatePhone))){
                    this.dialogsInfo="请输入正确格式的手机号"
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                }else{
                    this.validateres=true;
                    if(this.validatesec>0){
                        return;
                    }
                this.Interval = setInterval(this.updatesec,1000);
                this.getPhoneCode(this.validatePhone);
                }
            },
            paymentConfirm:function(){
                    if(!this.commitValidate()){return ;}
                    //教练订单
                    if(this.payforInfo.payType==0){
                        this.coachOrder();
                    }else if(this.payforInfo.payType==1){
                        this.drivingSchoolOrder();
                    }
            },
            drivingSchoolOrder:function(){//驾校订单
                var order={};
                order["drivingSchoolId"]=this.payforInfo.drivingSchoolId;
                order["payName"]=this.payName;
                order["payPhone"]=this.validatePhone;
                order["payProvince"]=this.pvselect.name;
                order["payCity"]=this.ctselect.name;
                order["payDistrict"]=this.dtselect.name;
                order["orderPrice"]=this.payforInfo.deposit;
                order["userId"]=this.payforInfo.user.userId
                //order["userId"]="8d6df9c6-f818-4f9e-b39b-01923c74920b";
                order["orderStatue"]=0;
                order["classtypeName"]=this.payforInfo.classtypeName;
                order["classtypeIntroduction"]=this.payforInfo.classtypeIntroduction;
                order["classtypePrice"]=this.payforInfo.classtypePrice;
                order["classtypeType"]=this.payforInfo.classtypeType;
                console.log(order)
                clearInterval(this.Interval);
                this.$http.post(types.HTTP_URL+'/orderTab/insertOrder',order,{emulateJSON:true}).then(function(response){
                        console.log(response.data);
                        if(response.data.status==0){
                            this.dialogsInfo="支付失败"
                            var $iosDialog2 = $('#iosDialog2');
                            $iosDialog2.fadeIn(200);
                        }else if(response.data.status==1){//订单插入成功
                            this.$store.dispatch('add_order',response.data.data);
                            this.$router.push({ name: 'payMent'})
                            //相关跳转
                        }else if(response.data.status==2){//
                            this.dialogsInfo="您已报名同班型信息。"
                            var $iosDialog2 = $('#iosDialog2');
                            $iosDialog2.fadeIn(200);
                        }
                    })
            },
            coachOrder:function(){//教练订单
                    var order={};
                    order["coachId"]=this.payforInfo.coachId;
                    order["payName"]=this.payName;
                    order["payPhone"]=this.validatePhone;
                    order["payProvince"]=this.pvselect.name;
                    order["payCity"]=this.ctselect.name;
                    order["payDistrict"]=this.dtselect.name;
                    order["orderPrice"]=this.payforInfo.deposit;
                    order["userId"]=this.payforInfo.user.userId
                    //order["userId"]="8d6df9c6-f818-4f9e-b39b-01923c74920b";
                    order["orderStatue"]=0;
                    order["classtypeName"]=this.payforInfo.classtypeName;
                    order["classtypeIntroduction"]=this.payforInfo.classtypeIntroduction;
                    order["classtypePrice"]=this.payforInfo.classtypePrice;
                     order["classtypeType"]=this.payforInfo.classtypeType;
                console.log(order)
                    clearInterval(this.Interval);
                    this.$http.post(types.HTTP_URL+'/orderTab/insertOrder',order,{emulateJSON:true}).then(function(response){
                        console.log(response.data);
                        if(response.data.status==0){
                            this.dialogsInfo="支付失败"
                            var $iosDialog2 = $('#iosDialog2');
                            $iosDialog2.fadeIn(200);
                        }else if(response.data.status==1){//订单插入成功
                            this.$store.dispatch('add_order',response.data.data);
                            this.$router.push({ name: 'payMent'})
                            //相关跳转
                        }
                    })
            },
            closeDialog : function(){
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            }
        },
        components: {
            'loading':require('../components/loading.vue')
        }
    }
</script>
