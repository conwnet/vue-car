<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <div class="row">
                <div class="col-xs-12" style="padding-left:20px">{{caochName}}</div>
                <div class="col-xs-12" style="padding-left:20px">{{caochActivity.activityContent}}</div>
                <!--<div class="col-xs-4  col-xs-before" v-for="img in imgList">-->
                <!--<img :src="img.url" class="img-responsive">-->
                <!--</div>-->
                <div class="activity-image" style="padding: 0px 1px 0px 13px;">
                    <div class="activity-image-item" v-for="img in caochActivity.activityImage">
                        <img :src="img_url+img.url"  class="img1">
                    </div>
                </div>
            </div>
            <!--<div class="row share-row">-->
            <!---->
            <!--</div>-->
        </div>
</template>
<style>
.img-size{
      width: 105px;
    height: 105px;
    margin: auto;
}
.col-xs-before{
padding: 5px 0px 3px 6px;
}
.share-row{
    padding-left: 15px;
    padding-right: 15px;
}

 .activity-image1 {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 0 20px;
        flex-direction: row;
        flex-wrap: wrap;
    }

    .activity-image1 .activity-image-item1 {
        width: 33%;
    }

    .img1 {
        width: 100%;
        height: auto;
        padding: 5px;
    }


</style>
<script>
import * as types from "../store/mutation-types"
    export default{
        data(){
            return{
                loading: true,
                pageInt:1,
                caochActivity:null,
                coachList:null,
                img_url:types.IMG_URL
            }
        },
        props:['caochName','coachId','activityId'],
        created:function(){
             this.getAllActivityByCoaIdAndActId();
             this.cons();
        },
        components:{
             'loading':require('../components/loading.vue')
        },
        methods:{
            cons:function(){
                console.log("activityId is "+this.activityId);
            },
            getAllActivityByCoaIdAndActId:function(){
                this.$http.post(types.HTTP_URL+'/activity/getAllActivityByCoaIdAndActId',{coachId:this.coachId,activityId:this.activityId},
                {emulateJSON:true}).then(function(response){
                       if(response.data.status==1){
                            this.caochActivity = response.data.data;
                            this.loading=false;
                            console.log(this.caochActivity);
                       }else{
                       }
                })
            },
        }
    }
</script>
