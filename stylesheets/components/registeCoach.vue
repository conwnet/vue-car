<template>
    <div>
        <div class="row topbanner text-center ">
            <div class="col-xs-2" v-on:click="goback">
                X
            </div>
            <div class="col-xs-9">
                注册
            </div>
        </div>
         <div class="row rowtranp_color margintop" style="margin-top:44px;">
                 <div class="weui-cells  coach-cells">
                     <div class="weui-cell weui-cell_access">
                         <div class="weui-cell__hd"><label class="weui-label">上传头像</label></div>
                         <div class="weui-cell__ft weui-cell__bd">
                             <img :src="coach.coachPhoto" class="memberimg">
                         </div>
                     </div>
                     <div class="weui-cell weui-cell_access">
                         <div class="weui-cell__hd"><label class="weui-label">填写姓名</label></div>
                         <div class="weui-cell__ft weui-cell__bd">
                             <input class="weui-input input_text_right" v-model="coach.coachName" type="text" placeholder="填写真实姓名,方便学员找到你(必填)">
                         </div>
                     </div>
                     <div class="weui-cell weui-cell_access">
                         <div class="weui-cell__hd"><label class="weui-label">输入电话</label></div>
                         <div class="weui-cell__ft weui-cell__bd">
                             <input class="weui-input input_text_right" v-model="coach.coachPhone" type="number" placeholder="你的电话号码(必填)">
                         </div>
                     </div>
                     <div class="weui-cell">
                         <div class="weui-cell__hd"><label class="weui-label" >验证码</label></div>
                         <div class=" weui-cell__bd">
                             <input class="weui-input" type="number" v-model="validateCode" placeholder="" maxlength="6">
                         </div>
                         <div class="weui-cell__ft">
                             <a  @click="validatePhoneNumber" class="weui-btn weui-btn_mini weui-btn_default">
                                 <span v-show="validatesec>0">({{validatesec}}s)</span>获取验证码
                             </a>
                         </div>
                     </div>
                     <div class="weui-cell weui-cell_access">
                         <div class="weui-cell__hd"><label class="weui-label">所属驾校</label></div>
                         <div class="weui-cell__ft weui-cell__bd">
                             <input class="weui-input input_text_right" type="text" v-model="coach.drivingSchoolName" placeholder="你所在的驾校">
                         </div>
                     </div>
                     <div class="weui-cell weui-cell_access">
                         <div class="weui-cell__hd"><label class="weui-label">填写驾龄</label></div>
                         <div class="weui-cell__ft weui-cell__bd">
                             <input class="weui-input input_text_right" type="number" v-model="coach.drivingAge" placeholder="填写数字">
                         </div>
                     </div>
                     <div class="weui-cell ">
                         <div class="weui-cell-hd"><label class="weui-label">个人简介</label></div>
                         <div class="weui-cell__ft weui-cell__bd">
                             <textarea class="weui-textarea" placeholder="最多200字" rows="3" v-model="coach.coachIntroduction"></textarea>
                             <div class="weui-textarea-counter"><span>0</span>/200</div>
                         </div>
                     </div>
                 </div>
         </div>
        <div class="row rowtranp_color margintop">
                <div class="weui-cells  coach-cells ">
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">毕业学员</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <input class="weui-input input_text_right" type="number" v-model="coach.graduationedStudentsCount" placeholder="填写已毕业学员数量">
                        </div>
                    </div>
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">拿证时间</label></div>
                        <div class="weui-cell__bd">
                            <input class="weui-input input_text_right" v-model="coach.drivingLicenceTime" type="number" placeholder="平均每个学员拿证时间">
                        </div>
                        <div class="weui-cell__ft">
                            天
                        </div>
                    </div>
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">科二通过率</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <input class="weui-input input_text_right" v-model="coach.subjectTwoPassRate" type="text" placeholder="填写数字%(本地区平均为56%)">
                        </div>
                        <div class="weui-cell__ft">
                            %
                        </div>
                    </div>
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">科三通过率</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <input class="weui-input input_text_right" v-model="coach.subjectThreePassRate" type="text" placeholder="填写数字%(本地区平均为56%)">
                        </div>
                        <div class="weui-cell__ft">
                            %
                        </div>
                    </div>
                </div>
        </div>
        <div class="row rowtranp_color margintop">
                <div class="weui-cells  coach-cells ">
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">班级信息</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                        </div>
                    </div>
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label">班级种类</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            C1
                        </div>
                    </div>
                    <div v-for="(cclass,index) in cclasses">
                        <div class="weui-cells  coach-cells ">
                            <div class="weui-cell">
                                <div class="weui-cell__hd"><label class="weui-label">班级名称</label></div>
                                <div class=" weui-cell__bd">
                                    <input class="weui-input" type="text" placeholder="" v-model="cclass.classtypeName">
                                </div>
                                <div class="weui-cell__ft">
                                    <a v-if="index!=0"  @click="removeC1Class(index)" class="weui-btn weui-btn_mini weui-btn_default">
                                        删除
                                    </a>
                                </div>
                            </div>
                            <div class="weui-cell">
                                <div class="weui-cell__hd"><label class="weui-label">班级描述</label></div>
                                <div class="weui-cell__ft weui-cell__bd">
                                    <input class="weui-input" type="text" placeholder="" v-model="cclass.classtypeIntroduction">
                                </div>
                            </div>
                            <div class="weui-cell ">
                                <div class="weui-cell__hd"><label class="weui-label">班级价格</label></div>
                                <div class="weui-cell__ft weui-cell__bd">
                                    <input class="weui-input" type="number" placeholder="" v-model="cclass.classtypePrice">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="weui-cells" style="margin-top:0px;">
                    <div class="weui-cell">
                        <div class="weui-cell__hd"></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <a  @click="addC1" class="weui-btn weui-btn_mini weui-btn_default">
                                添加
                            </a>
                        </div>
                    </div>
                </div>
        </div>
        <div class="row rowtranp_color margintop">
                <div class="weui-cells  coach-cells ">
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label">班级种类</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            B1
                        </div>
                    </div>
                    <div v-for="(bclass,index) in bclasses">
                        <div class="weui-cells  coach-cells ">
                            <div class="weui-cell">
                                <div class="weui-cell__hd"><label class="weui-label">班级名称</label></div>
                                <div class=" weui-cell__bd">
                                    <input class="weui-input" type="text" placeholder="" v-model="bclass.classtypeName">
                                </div>
                                <div class="weui-cell__ft">
                                    <a v-if="index!=0"  @click="removeB1Class(index)" class="weui-btn weui-btn_mini weui-btn_default">
                                        删除
                                    </a>
                                </div>
                            </div>
                            <div class="weui-cell">
                                <div class="weui-cell__hd"><label class="weui-label">班级描述</label></div>
                                <div class="weui-cell__ft weui-cell__bd">
                                    <input class="weui-input" type="text" placeholder="" v-model="bclass.classtypeIntroduction">
                                </div>
                            </div>
                            <div class="weui-cell ">
                                <div class="weui-cell__hd"><label class="weui-label">班级价格</label></div>
                                <div class="weui-cell__ft weui-cell__bd">
                                    <input class="weui-input" type="number" placeholder="" v-model="bclass.classtypePrice">
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="weui-cells" style="margin-top:0px;">
                    <div class="weui-cell">
                        <div class="weui-cell__hd"></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <a  @click="addB1" class="weui-btn weui-btn_mini weui-btn_default">
                                添加
                            </a>
                        </div>
                    </div>
                </div>
        </div>
        <div class="row rowtranp_color margintop">
            <div class="weui-cells  coach-cells ">
                <div class="weui-cell weui-cell_access">
                    <div class="weui-cell__hd"><label class="weui-label">班级种类</label></div>
                    <div class="weui-cell__ft weui-cell__bd">
                        VIP
                    </div>
                </div>
                <div v-for="(vipclass,index) in vipclasses">
                    <div class="weui-cells  coach-cells ">
                        <div class="weui-cell">
                            <div class="weui-cell__hd"><label class="weui-label">班级名称</label></div>
                            <div class=" weui-cell__bd">
                                <input class="weui-input" type="text" placeholder="" v-model="vipclass.classtypeName">
                            </div>
                            <div class="weui-cell__ft">
                                <a v-if="index!=0"  @click="removeVIPClass(index)" class="weui-btn weui-btn_mini weui-btn_default">
                                    删除
                                </a>
                            </div>
                        </div>
                        <div class="weui-cell">
                            <div class="weui-cell__hd"><label class="weui-label">班级描述</label></div>
                            <div class="weui-cell__ft weui-cell__bd">
                                <input class="weui-input" type="text" placeholder="" v-model="vipclass.classtypeIntroduction">
                            </div>
                        </div>
                        <div class="weui-cell ">
                            <div class="weui-cell__hd"><label class="weui-label">班级价格</label></div>
                            <div class="weui-cell__ft weui-cell__bd">
                                <input class="weui-input" type="number" placeholder="" v-model="vipclass.classtypePrice">
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <div class="weui-cells" style="margin-top:0px;">
                <div class="weui-cell">
                    <div class="weui-cell__hd"></div>
                    <div class="weui-cell__ft weui-cell__bd">
                        <a  @click="addVIP" class="weui-btn weui-btn_mini weui-btn_default">
                            添加
                        </a>
                    </div>
                </div>
            </div>
        </div>
        <div class="row rowtranp_color margintop">
            <div class="weui-cells  coach-cells ">
                <div class="weui-cell ">
                    <div class="weui-cell__hd"><label class="weui-label">确认码</label></div>
                    <div class="weui-cell__ft weui-cell__bd">
                        <input class="weui-input input_text_right" v-model="coach.coachOrderCode"  type="number" placeholder="填写支付确认码">
                    </div>
                </div>
                <div class="weui-cell ">
                    <div class="weui-cell__hd"><label class="weui-label">支付定金</label></div>
                    <div class="weui-cell__ft weui-cell__bd">
                        <input class="weui-input input_text_right" v-model="coach.deposit"  type="number" placeholder="填写学员支付定金金额">
                    </div>
                </div>
            </div>
        </div>
        <div class="row rowtranp_color margintop">
                <div class="weui-cells  coach-cells ">
                    <div class="weui-cell ">
                        <div class="weui-cell__hd"><label class="weui-label">服务保证</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            最多可选4项(必填)
                        </div>
                    </div>
                    <div class="row" style="">
                        <div class="col-xs-4  label_div_top" v-for="item in coachCodes.screeningList">
                            <div  @click="selectedTag($event,item)">
                                {{item.name}}
                            </div>
                        </div>
                    </div>
                </div>
        </div>

        <div class="row rowtranp_color margintop">
                <div class="weui-cells  coach-cells ">
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label">招生范围</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <input class="weui-input input_text_right" type="text" v-model="coach.enrolRange" placeholder="请输入招生范围(必填)">
                        </div>
                    </div>
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label" style="width: 80px;">练车场地</label></div>
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
                    <div class="weui-cell weui-cell_access">
                        <div class="weui-cell__hd"><label class="weui-label">详细地址</label></div>
                        <div class="weui-cell__ft weui-cell__bd">
                            <input class="weui-input input_text_right" type="text" v-model="coach.learnSpace" placeholder="请输入练车详细地址(必填)">
                        </div>
                    </div>
                </div>
        </div>
        <div class="row rowtranp_color top-row-border bottom-row-border  margintop">
            <div class="weui-cell">
                <div class="weui-cell__hd">
                    <div class="weui-uploader">
                        <div class="weui-uploader__hd">
                            <div class="weui-uploader__info">0/2</div>
                        </div>
                        <div class="weui-uploader__bd">
                            <ul class="weui-uploader__files" id="uploaderFiles">
                                <li class="weui-uploader__file" style="background-image:url(./images/pic_160.png)"></li>
                                <li class="weui-uploader__file" style="background-image:url(./images/pic_160.png)"></li>
                                <li class="weui-uploader__file" style="background-image:url(./images/pic_160.png)"></li>
                            </ul>
                            <div class="weui-uploader__input-box">
                                <input id="uploaderInput" class="weui-uploader__input" type="file" accept="image/*" multiple="" >
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row margintop" >
            <div class="col-xs-12 text-center">
                <a  class="weui-btn  weui-btn_blue" @click="comitValidate">注册</a>
            </div>
        </div>
        <div class="row  rowtranp_color margintop">
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
.input_text_right{
 text-align:right;
}
.input_text_left{
 text-align:left;
}
.input_text_center{
 text-align:center;
}
.coach-cells{
font-size: inherit;
border-top:1px solid #d9d9d9;
border-bottom:1px solid #d9d9d9;
}
.rowtranp_color{
background:transparent;
}
.memberimg{
            margin: auto;
            position: relative;
            width: 48px;
            height: 48px;
            -moz-border-radius: 50%; /* Firefox */
            -webkit-border-radius: 50%; /* Safari 和 Chrome */
            border-radius: 50%; /* Opera 10.5+, 以及使用了IE-CSS3的IE浏览器 */
}
.label_div_top div{
        border: solid 1px #d7d7d7;
        border-radius: 3px;
        padding: 5px;
        background-color: white;
        font-size: 1.5rem;
        overflow: hidden;
        white-space: nowrap;
        margin: 5px;
        text-align: center;
}
</style>
    <script>
    import * as types from '../store/mutation-types'
    import * as storage from '../store/localStorage'
        export default{
            data(){
                return{
                    postValidateCode:"",
                    validateCode:"",
                    validateres:false,
                    validatesec:0,
                    Interval:null,
                    coach:null,
                    dialog_info:"请输入正确格式的手机号",
                    bclasses:[{classtypeName:"",classtypeIntroduction:"",classtypePrice:"",classType:2}],
                    cclasses:[{classtypeName:"",classtypeIntroduction:"",classtypePrice:"",classType:1}],
                    vipclasses:[{classtypeName:"",classtypeIntroduction:"",classtypePrice:"",classType:3}],
                    coachCodes:[],
                    screeningCount:0,
                    screen_labels:[],
                    pvselect:null,
                    ctselect:null,
                    dtselect:null,
                    bindctselects:[],
                    binddtselects:[],
                    img_url:types.IMG_URL
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
                },
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
            created:function(){
                this.getStoreCoach();
                this.getCoachCodeList();
            },
            methods:{
                getCoachCodeList:function(){
                    this.$http.post(types.HTTP_URL+'/coach/selectRegisterCoachWithCode',{},{emulateJSON:true}).then(function(response){
                            if(response.data.status==0){
                                alert("this data  is null");
                            }else{//未注册
                                this.coachCodes=response.data;
                            }
                        })
                },
                initUpdateClassTypes:function(classTypes){
                    var hasC1=false;
                    var hasB1=false;
                    var hasVip=false;
                    for(var i=0;i<classTypes.length;i++){
                            console.log(classTypes[i].classType);
                            switch(classTypes[i].classType){
                                    case 1 :
                                                if(!hasC1){ this.cclasses=[]}
                                                this.cclasses.push({classtypeName:classTypes[i].classtypeName,classtypeIntroduction:classTypes[i].classtypeIntroduction,classtypePrice:classTypes[i].classtypePrice,classType:1});
                                                hasC1=true;
                                                break;
                                    case 2 :
                                                if( !hasB1){ this.bclasses=[]}
                                                this.bclasses.push({classtypeName:classTypes[i].classtypeName,classtypeIntroduction:classTypes[i].classtypeIntroduction,classtypePrice:classTypes[i].classtypePrice,classType:2});
                                                hasB1=true;
                                                break;
                                    case 3 :
                                                if(!hasVip)(this.vipclasses=[])
                                                this.vipclasses.push({classtypeName:classTypes[i].classtypeName,classtypeIntroduction:classTypes[i].classtypeIntroduction,classtypePrice:classTypes[i].classtypePrice,classType:3});
                                                hasVip=true;
                                                break;
                                    }
                    }
                },
                initUpdateScreeningRelated:function(screenRelated){
                },
                getStoreCoach:function(){
                    if(this.$store.state.coachstore.coach)
                    {
                        this.coach=this.$store.state.coachstore.coach;
                        console.log(this.coach);
                                if(this.coach.classTypes){
                                    this.initUpdateClassTypes(this.coach.classTypes)
                                }
                    }else{
                        alert("获取微信信息失败，请返回重试!");
                    }
                },
                goback:function(){
                    this.$router.go(-1);
                },
                gotovalidate:function(){
                    this.$router.push({ path: 'phonevalidate' });
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
                    if( !(/^1[34578]\d{9}$/.test(this.coach.coachPhone))){
                        this.dialog_info="请输入正确格式的手机号";
                        var $iosDialog2 = $('#iosDialog2');
                        $iosDialog2.fadeIn(200);
                    }else{
                        this.validateres=true;
                        if(this.validatesec>0){
                            return;
                        }
                        this.Interval = setInterval(this.updatesec,1000);
                        this.getPhoneCode(this.coach.coachPhone);
                    }
                },
                getPhoneCode:function(userPhone){
                    this.$http.post(types.HTTP_URL+'/sendsms/sendTempSMS',{userPhone:userPhone},{emulateJSON:true}).then(function(response){
                                if(response.data.state==1){
                                    this.postValidateCode=response.data.code;
                                }
                        })
                },
                closeDialog : function(){
                    $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
                },
                addB1:function(){
                    if(!this.validateClassType(this.bclasses)){
                             this.dialog_info="请输入完整信息，再添加班级!";
                             var $iosDialog2 = $('#iosDialog2');
                             $iosDialog2.fadeIn(200);
                             return;
                    }
                    if(this.bclasses.length<3){
                        this.bclasses.push({classtypeName:"",classtypeIntroduction:"",classtypePrice:"",classType:2});
                        console.log(this.bclasses);
                    }
                },
                removeB1Class:function (index){
                  this.bclasses.splice(index, 1)
                },
                addC1:function(){
                    if(!this.validateClassType(this.cclasses)){
                             this.dialog_info="请输入完整信息，再添加班级!";
                             var $iosDialog2 = $('#iosDialog2');
                             $iosDialog2.fadeIn(200);
                             return;
                    }
                    if(this.cclasses.length<3){
                        this.cclasses.push({classtypeName:"",classtypeIntroduction:"",classtypePrice:"",classType:1});
                        console.log(this.cclasses);
                    }
                },
                removeC1Class:function (index){
                  this.cclasses.splice(index, 1)
                },
                addVIP:function(){
                if(!this.validateClassType(this.vipclasses)){
                             this.dialog_info="请输入完整信息，再添加班级!";
                             var $iosDialog2 = $('#iosDialog2');
                             $iosDialog2.fadeIn(200);
                             return;
                        }
                    if(this.vipclasses.length<3){
                        this.vipclasses.push({classtypeName:"",classtypeIntrodution:"",classtypePrice:"",classType:3});
                        console.log(this.vipclasses);
                    }
                },
                removeVIPClass:function (index){
                  this.vipclasses.splice(index, 1)
                },
                validateClassType:function(classtype){
                    var flag=true;
                    for(var i=0;i<classtype.length;i++){
                        if(!classtype[i].classtypeName || !classtype[i].classtypeIntroduction || !classtype[i].classtypePrice ){
                            flag=false;
                            break;
                        }
                    }
                    return flag;
                },
                selectedTag: function (e,item,index){
                    var el = e.currentTarget;
                    if(this.screeningCount == 4 && !el.getAttribute("class", "sereening-selected-tag")){
                             return;
                    } else {
                        if (el.getAttribute("class", "sereening-selected-tag")) {
                            for(var i=0;i<this.screen_labels.length;i++){
                                if(item.id==this.screen_labels[i].id){
                                    this.screen_labels.splice(i,1);
                                    el.removeAttribute("class", "sereening-selected-tag");
                                    this.screeningCount--;
                                    break;
                                }
                            }
                        } else {
                            el.setAttribute("class", "sereening-selected-tag");
                            this.screeningCount++;
                            this.screen_labels.push(item);
                        }
                    }
                },
                comitValidate:function(){
                    //姓名
                    if(!this.coach.coachName){
                         this.dialog_info="请输入姓名!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    //电话
                    if(!this.coach.coachPhone){
                         this.dialog_info="请输入电话!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    if(this.coach.coachOrderCode){
                       var len=(this.coach.coachOrderCode+"").length;
                       if(len!=4){
                        this.dialog_info="请输入4位确认码!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                       }
                    }
                    if(this.coach.deposit && this.coach.deposit>300){
                         this.dialog_info="支付定金不能超过300!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    //服务保证
                    if(!this.screen_labels){
                         this.dialog_info="请选择服务保证!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }

                    //招生范围
                    if(!this.coach.enrolRange){
                         this.dialog_info="请输入招生范围!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    //练车场地
                    if(!this.coach.learnSpace){
                         this.dialog_info="请输入练车场地!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    //省
                    if(!this.pvselect){
                         this.dialog_info="请选择省份!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    //市
                    if(!this.ctselect){
                         this.dialog_info="请选择市!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    //区
                    if(!this.dtselect){
                         this.dialog_info="请选择区域!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    if(!this.validateres){
                        this.dialog_info="请获取手机验证码!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    if(!this.validateCode){
                         this.dialog_info="请填写六位验证码!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    if(this.validateCode!=this.postValidateCode){
                        this.dialog_info="验证码有误，请仔细核对!";
                         var $iosDialog2 = $('#iosDialog2');
                         $iosDialog2.fadeIn(200);
                         return;
                    }
                    this.registerCoach();
                },
                registerCoach:function(){
                    this.coach.classTypes=[];
                    this.coach.classTypes=this.coach.classTypes.concat(this.bclasses);
                    this.coach.classTypes=this.coach.classTypes.concat(this.cclasses);
                    this.coach.classTypes=this.coach.classTypes.concat(this.vipclasses);
                    console.log(this.coach.classTypes);
                    this.coach.screeningRelateds=[];
                    var serviceLabel=0;
                    for(var l=0;l<this.screen_labels.length;l++)
                    {
                        this.coach.screeningRelateds.push({pid:this.coach.coachId,screeningId:this.screen_labels[l].id});
                        if(this.screen_labels[l].serviceLable==1){
                            serviceLabel++;
                        }
                    }
                    this.coach.serviceLabel=serviceLabel;
                    this.coach.province= this.pvselect.name;
                    this.coach.city=this.ctselect.name;
                    this.coach.district=this.dtselect.name;
                    var coach=this.coach;
                    this.$http.post(types.HTTP_URL+'/coach/registerCoach',JSON.stringify(coach),{emulateJSON:true}).then(function(response){
                            console.log(response.data);
                            if(response.data.status==0){
                                alert("注册失败");
                            }else if(response.data.status==1){//注册成功
                                var coach=response.data.data;
                                storage.removeLocalStorageItem(types.CACHE_STUDENT_INFO)
                                storage.setJsonLocalStorageItem(types.CACHE_COACH_INFO,coach)
                                storage.setStrLocalStorageItem(types.CACHE_ROLE_STATUS,1)
                                //相关跳转
                                if(storage.getStrLocalStorageItem(types.ROUTER_FROM_PATH_STATUS)){
                                     clearInterval(this.Interval);
                                    this.fromRouterPath();
                                }else
                                {
                                     clearInterval(this.Interval);
                                     this.$router.push({ path: 'coachPerson'});
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
                }
            }
        }
</script>
