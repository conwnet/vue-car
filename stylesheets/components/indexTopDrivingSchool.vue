<template>
    <div>
        <div class="row">
            <div class="col-xs-12 text-center">
                <img src="../images/top-jx.png" class="index-top-recommend-img">
            </div>
        </div>
        <div class="row">
            <div class="weui-flex" style="margin:15px;">
                <div class="weui-flex__item" v-for="(drvsl,index) in drivingSchool" @click="goSchoolInfo(drvsl.drivingSchoolId)">
                    <img :src="'../images/top'+(index+1)+'.png'" class="weui-flex__item_fimg">
                    <img :src="img_url+drvsl.drivingSchoolPhoto" class="weui-flex__item_simg">
                    <span class="weui-flex__item_jx">{{drvsl.drivingSchoolName}}</span>
                    <p class="weui-flex__item_pj">{{drvsl.favourableCount}}人好评</p>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="less">
    .weui-flex__item{
        &_fimg{
            position: absolute;
            margin: -5px 0px 0px 8px;
            width: 16%;
        }
        &_simg{
            margin: auto;
            width: 100%;
            padding:0px 0px 0px 5px;
        }
        &_jx{
            padding:0px 0px 0px 5px;
        }
        &_pj{
            color:red;
            padding:0px 0px 0px 5px;
        }
    }
</style>
<script>
import * as types from "../store/mutation-types"
    export default{
        data(){
            return{
             drivingSchool:null,
             img_url:types.IMG_URL
            }
        },
        created:function(){
            this.getIndexDrivingSchool();
        },
        methods:{
            getIndexDrivingSchool:function(){
                this.$http.post(types.HTTP_URL+'/drivingSchool/getIndexDrivingSchool',{},
                    {   emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                            this.drivingSchool=response.data.data;
                            }else{
                            console.log("this is  null");
                            }
                })
            },
            goSchoolInfo:function(id){
                this.$router.push({path:"/schoolInfoPage",name:"schoolInfoPage",params: { paramId: id}});
            },
        },
        components:{
        }
    }
</script>
