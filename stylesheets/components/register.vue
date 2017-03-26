<template>
    <div class="registervue_ground">
        <div class="row topbanner text-center " >
            <div class="col-xs-2" v-on:click="goback">
                X
            </div>
            <div class="col-xs-9">
                注册前登录
            </div>
        </div>
        <div class="row" style="background:#fff;margin-top:55px;" >
            <div class="col-xs-6">
                <div class="weui-cells weui-cells_checkbox">
                    <label class="weui-cell weui-check__label" for="s11">
                        <div class="weui-cell__hd">
                            <input type="radio" class="weui-check" value="1" v-model="checkedValues" name="checkbox1" id="s11" >
                            <i class="weui-icon-checked"></i>
                        </div>
                        <div class="weui-cell__bd">
                            <span>我是教练</span>
                        </div>
                    </label>
                </div>
            </div>
            <div class="col-xs-6">
                <div class="weui-cells weui-cells_checkbox">
                    <label class="weui-cell weui-check__label" for="s12">
                        <div class="weui-cell__hd">
                            <input type="radio" class="weui-check" value="2" v-model="checkedValues" name="checkbox1" id="s12" >
                            <i class="weui-icon-checked"></i>
                        </div>
                        <div class="weui-cell__bd">
                            <span>我是学员</span>
                        </div>
                    </label>
                </div>
            </div>
        </div>
        <div class="row" style="background:#fff; margin-top:15px;" >
            <div class="col-xs-12 text-center">
                <img  src="../images/wechat.png" style="width:43%"  @click="weiXinRegister">
                <div class="row">
                    <div class="col-xs-12">
                            微信登录
                    </div>
                </div>
            </div>
        </div>
        <div id="dialogs">
            <div class="js_dialog" id="iosDialog2" style="display: none;">
                <div class="weui-mask"></div>
                <div class="weui-dialog">
                    <div class="weui-dialog__bd">请选择注册角色</div>
                    <div class="weui-dialog__ft">
                        <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">知道了</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style>
.registervue_ground{
background:#d9d9d9;
}
    .weui-cells:before{
     border-top:none;
    }
    .weui-cells:after{
    border-bottom:none;
    }
    .weui-cells_checkbox .weui-icon-checked:before{
    content: "\EA06";
    color: #c9c9c9;
    font-size: 23px;
    display: block;
    }
    .weui-cells_checkbox .weui-check:checked+.weui-icon-checked:before {
    content: "\EA06";
    color: #00a0e9;
    }
</style>
<script>
import * as types from '../store/mutation-types'
import * as storage from '../store/localStorage'

  export default{
        data(){
            return {
                checkedValues:null
            }
        },
        created:function(){
            this.getLocation();
            console.log(this.$router);
        },
        methods:{
            getLocation:function(){
                //storage.cleanLocalStorage();
                this.$http.get(types.TXAPI_URL+'/?address=北京市海淀区彩和坊路海淀西大街74号&key=IEPBZ-VSL6J-NUBFZ-FMQOW-WQSL3-QNF7D',{},{emulateJSON:true}).then(function(response){
                  console.log(response.data)
                })
            },
            goback:function(){
                this.$router.go(-1);
            },
            weiXinRegister:function(){
                if(!this.checkedValues){
                    var $iosDialog2 = $('#iosDialog2');
                    $iosDialog2.fadeIn(200);
                    return false;
                }
                if(this.checkedValues==1){
                    this.$http.post(types.HTTP_URL+'/coach/getUserByCode',{},{emulateJSON:true}).then(function(response){
                        console.log(response.data);
                        if(response.data.status==0){
                            alert("获取用户信息失败，请联系管理");
                        }else if(response.data.status==1 || response.data.status==2){
                                //未注册或已有openid但未实名
                                var coach=response.data.data;
                                this.$store.dispatch('coach_login',coach);
                                this.$router.push({ path: 'registerCoach'});
                        }else if(response.data.status==3){
                                //实名注册
                                var coach=response.data.data;
                                storage.removeLocalStorageItem(types.CACHE_STUDENT_INFO)
                                storage.setJsonLocalStorageItem(types.CACHE_COACH_INFO,coach)
                                storage.setStrLocalStorageItem(types.CACHE_ROLE_STATUS,1)
                                //获取相应来源跳转
                                if(storage.getStrLocalStorageItem(types.ROUTER_FROM_PATH_STATUS)){
                                    this.fromRouterPath();
                                }else
                                {
                                    this.$router.push({ path: 'coachPerson'});
                                }

                        }
                    })
                }
                else{
                    this.$http.post(types.HTTP_URL+'/students/getUserByCode',{},{emulateJSON:true}).then(function(response){
                        console.log(response.data);
                        if(response.data.status==0){
                            alert("获取用户信息失败，请联系管理");
                        }else if(response.data.status==1 || response.data.status==2){
                                //未注册或已有openid但未实名
                                var student=response.data.data;
                                this.$store.dispatch('student_login',student);
                                this.$router.push({ path: 'phonevalidate'});
                        }else if(response.data.status==3){
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
                }
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
