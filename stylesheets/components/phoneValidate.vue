<template>
    <div>
        <div class="row topbanner text-center " >
            <div class="col-xs-2" v-on:click="goback">
                X
            </div>
            <div class="col-xs-9">
               学员注册
            </div>
        </div>
        <div class="row top-row-border bottom-row-border margintop" style="margin-top:44px;">
            <div class="col-xs-12">
                <div class="weui-cell weui-cell_select weui-cell_select-before" style="padding-right:0px;">
                    <div class="weui-cell__hd">
                        <select class="weui-select" name="select2" style="width:55px;padding-left: 0px;">
                            <option value="1">+86</option>
                        </select>
                    </div>
                    <div class="weui-cell__bd">
                        <input class="weui-input" v-model="validatePhone" type="number" pattern="[0-9]*" placeholder="请输入号码">
                    </div>
                    <div class="weui-cell__ft">
                        <a  @click="validatePhoneNumber" class="weui-btn weui-btn_mini weui-btn_default">
                            <span v-show="validatesec>0">({{validatesec}}s)</span>获取验证码
                        </a>
                    </div>
                </div>
            </div>
        </div>
        <div class="row  bottom-row-border" >
            <div class="col-xs-3 text-center">
                <span class="validatetext">验证码</span>
            </div>
            <div class="col-xs-9">
                <input  v-model="validateCode"  class="weui-input validatetext" type="number" style="padding-left: 15px;" placeholder="请输入验证码">
            </div>
        </div>
        <div class="row"  style="background: transparent;">
            <div class="col-xs-12 text-center">
                <span class="validatetext">未注册的手机号码验证后自动创建点评账户</span>
            </div>
        </div>
        <div class="row"   style="background: transparent;" >
            <div class="col-xs-12 text-center">
                <a  @click="registerSucess" v-bind:class="{'weui-btn_disabled':isValidate}" class="weui-btn  weui-btn_blue">注册</a>
            </div>
        </div>
        <div class="row"   style="background: transparent;">
            <div class="col-xs-12 text-right base-color">
                <span >账号密码登录</span>
            </div>
        </div>
        <div id="dialogs">
            <div class="js_dialog" id="iosDialog2" style="display: none;">
                <div class="weui-mask"></div>
                <div class="weui-dialog">
                    <div class="weui-dialog__bd">{{dialog_info}}</div>
                    <div class="weui-dialog__ft">
                        <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style>
    .validatetext{
        height:44px;
        line-height:44px;
    }
</style>
<script>
import * as types from '../store/mutation-types'
import * as storage from '../store/localStorage'
    export default{
        data(){
            return {
                isValidate:true,
                validateCode:"",
                validatePhone:"",
                validateres:false,
                validatesec:0,
                Interval:null,
                student:null,
                postValidateCode:"",
                dialog_info:"请输入正确格式的手机号"
            }
        },
        watch: {
            validateCode: function (val) {
             const codelen=(val+"").length;
               if(codelen == 6 && this.validateres){
                this.isValidate=false;
               }else{
                this.isValidate=true;
               }
            }
        },
        created:function(){
            this.getStoreStudent();
        },
        methods:{
            getStoreStudent:function(){
                if(this.$store.state.studentstore.student)
                {
                   this.student=this.$store.state.studentstore.student;
                   console.log(this.student);
                }else{
                    this.dialog_info="获取微信信息失败，请返回重试!";
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                }
            },
            goback:function(){
                this.$router.go(-1);
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
            getPhoneCode:function(userPhone){
                    this.$http.post(types.HTTP_URL+'/sendsms/sendTempSMS',{userPhone:userPhone},{emulateJSON:true}).then(function(response){
                                if(response.data.state==1){
                                    this.postValidateCode=response.data.code;
                                }
                        })
            },
            validatePhoneNumber:function(){
                if( !(/^1[34578]\d{9}$/.test(this.validatePhone))){
                    this.dialog_info="请输入正确格式的手机号!";
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                }else{
                    this.validateres=true;
                    if(this.validatesec>0){
                        return;
                    }
                this.Interval = setInterval(this.updatesec,1000);
                this.getPhoneCode(this.validatePhone)
                }
            },
            commitValidate:function(){
                if(!this.validateres){
                        this.dialog_info="请获取手机验证码!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return false;
                    }
                    if(!this.validateCode){
                         this.dialog_info="请填写六位验证码!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return false;
                    }
                    if(this.validateCode!=this.postValidateCode){
                        this.dialog_info="验证码有误，请仔细核对!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return false;
                    }
                    return true;
            },
            registerSucess:function(){
                    if(!this.commitValidate()){
                    return ;
                    }
                   clearInterval(this.Interval);
                        this.student.userPhone=this.validatePhone;
                        this.$http.post(types.HTTP_URL+'/students/registerStudent',this.student,{emulateJSON:true}).then(function(response){
                                console.log(response.data);
                                if(response.data.status==0){
                                    alert("注册失败");
                                }else if(response.data.status==1){//未注册
                                         //实名注册
                                        var student=response.data.data;
                                        storage.removeLocalStorageItem(types.CACHE_COACH_INFO)
                                        storage.setJsonLocalStorageItem(types.CACHE_STUDENT_INFO,student)
                                        storage.setStrLocalStorageItem(types.CACHE_ROLE_STATUS,2)
                                        //获取相应来源跳转
                                        if(storage.getStrLocalStorageItem(types.ROUTER_FROM_PATH_STATUS)){
                                            this.fromRouterPath();
                                        }else
                                        {
                                              this.$router.push({ path: 'studenPerson'});
                                        }
                                }
                        })
            },
             fromRouterPath:function(){
                        var router_path=storage.getStrLocalStorageItem(types.ROUTER_FROM_PATH)
                        var router_name=storage.getStrLocalStorageItem(types.ROUTER_FROM_PATH_NAME)
                        var router_param=storage.getStrLocalStorageItem(types.ROUTER_FROM_PATH_PARAM)
                        if(router_param){
                                this.$router.push({path:router_path,name:router_name,params: { paramId: router_param}});
                        }else{
                               this.$router.push({path:router_path,name:router_name});
                        }
                },
            closeDialog : function(){
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
            }
        }
  }
</script>
