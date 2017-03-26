<template>
    <div>
        <div class="row">
            <div class="col-xs-12 text-center">
                <img src="../images/top-rq.png" class="index-top-recommend-img">
            </div>
        </div>
        <div class="row">
            <div class="weui-coach-flex" style="margin:15px;">
                <div class="weui-coach-flex__item" v-if="coachs[0]">
                    <img :src="get_image_path(coachs[0].coachPhoto)" class="weui-flex__item_simg"  @click="goCoachInfo(coachs[0].coachId)">
                    <div class="detail_div">
                        <div>{{coachs[0].coachName}}</div>
                        <span><img v-for="start in coachs[0].coachScore" src="../images/index-star.png" alt="" class="star-icon index-star-width"></span>
                    </div>
                </div>
                <div class="weui-coach-flex__item" >
                    <div class="weui-coach-flex__item_item_list" v-if="coachs[1]">
                        <img :src="get_image_path(coachs[1].coachPhoto)" class="weui-flex__item_simg"  @click="goCoachInfo(coachs[1].coachId)">
                        <div class="detail_div">
                            <div>{{coachs[1].coachName}}</div>
                            <span><img v-for="start in coachs[1].coachScore" src="../images/index-star.png" alt="" class="star-icon index-star-width"></span>
                        </div>
                    </div>
                    <div class="weui-coach-flex__item_item_list" v-if="coachs[2]">
                        <img :src="get_image_path(coachs[2].coachPhoto)" class="weui-coach-flex__item_item_list_fimg"  @click="goCoachInfo(coachs[2].coachId)">
                        <div class="detail_div">
                            <div>{{coachs[2].coachName}}</div>
                            <span><img v-for="start in coachs[2].coachScore" src="../images/index-star.png" alt="" class="star-icon index-star-width"></span>
                        </div>
                    </div>
                </div>
                <div class="weui-coach-flex__item" >
                    <div class="weui-coach-flex__item_item_list" v-if="coachs[3]">
                        <img :src="get_image_path(coachs[3].coachPhoto)" class="weui-flex__item_simg"  @click="goCoachInfo(coachs[3].coachId)">
                        <div class="detail_div">
                            <div>{{coachs[3].coachName}}</div>
                            <span><img v-for="start in coachs[3].coachScore" src="../images/index-star.png" alt="" class="star-icon index-star-width"></span>
                        </div>
                    </div>
                    <div class="weui-coach-flex__item_item_list" v-if="coachs[4]">
                        <img :src="get_image_path(coachs[4].coachPhoto)" class="weui-coach-flex__item_item_list_fimg" @click="goCoachInfo(coachs[4].coachId)">
                        <div class="detail_div">
                            <div>{{coachs[4].coachName}}</div>
                            <span>
                                <img v-for="start in coachs[4].coachScore" src="../images/index-star.png" alt="" class="star-icon index-star-width">
                            </span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        <div class="row">
            <div class="col-xs-12" style="height:60px">

            </div>
        </div>
    </div>
</template>
<style lang="less">
    .index-star-width{
    width:17% !important;
    }
    .weui-coach-flex{
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        margin:15px;
        &__item{
            -webkit-box-flex: 1;
            -webkit-flex: 1;
            flex: 1;
            display: -webkit-box;
            display: -webkit-flex;
            display: flex;
            flex-flow:column;
            &_item_list{
                -webkit-box-flex: 1;
                -webkit-flex: 1;
                flex: 1;
                &_fimg{
                    margin: auto;
                    width: 100%;
                    padding:4px 0px 0px 5px;
                }
            }
        }
    }
    .detail_div{
        position: relative;
        margin: -44px 0px 0px 5px;
        width: 73%;
        background: #000000;
        opacity: 0.7;
        color:#fff;
    }
    .detail_div span{
        color:#FFFFFF;
        padding:5px;
    }
</style>
<script>
import * as types from "../store/mutation-types"
     export default {
      data(){
        return {
            coachs:{status:1,
                data:[{coach_id:"1d",coach_name:"王教练",coach_photo:"../dist/images/jl-demo1.png",coach_score:"8"},
                {coach_id:"jiaolian2",coach_name:"王教练",coach_photo:"../images/jl-demo2.png",coach_score:"1"},
                {coach_id:"jiaolian3",coach_name:"王教练",coach_photo:"../images/jl-demo3.png",coach_score:"2"},
                {coach_id:"jiaolian4",coach_name:"王教练",coach_photo:"../images/jl-demo4.png",coach_score:"3"},
                {coach_id:"jiaolian5",coach_name:"王教练",coach_photo:"../images/jl-demo5.png",coach_score:"4"}
                ]
             },
             img_url:types.IMG_URL,
             get_image_path : types.GET_IMAGE_PATH
        }
      },
      created:function(){
        this.getIndexCoach();
      },
      methods:{
        getIndexCoach:function(){
            this.$http.post(types.HTTP_URL+'/coach/selectIndexCoach',{},
                    {   emulateJSON:true}).then(function(response){
                            if(response.data.status==1){
                            console.log(response.data.data);
                            this.coachs=response.data.data;
                            }else{
                            console.log("this is  null");
                            }
                })
        },
        goCoachInfo:function(id){
            this.$router.push({path:"/coachInfoPage",name:"coachInfoPage",params: { paramId:id}});
        },
      }
     }
</script>
