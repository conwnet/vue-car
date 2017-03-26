<style>
    .activity-coach, .activity-image {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 15px 15px 5px 15px;
    }

    .activity-coach .activity-coach-item-flex {
        flex-flow: column;
        display: flex;
    }

    .activity-coach .activity-coach-item-flex img {
        height: 40px;
        width: 40px;
    }

    .activity-coach .activity-coach-item-flex-column {
        -webkit-box-flex: 1;
        -webkit-flex: 1;
        flex: 1;
    }

    .activity-coach .activity-coach-item-flex-2 {
        flex-flow: column;
        display: flex;
        flex: 3;
        padding-left: 10px;;
    }

    .activity-image {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 0 60px;
        flex-direction: row;
        flex-wrap: wrap;
    }

    .activity-image .activity-image-item {
        width: 33%;
    }

    .activity-image .activity-image-item img {
        width: 100%;
        height: auto;
        padding: 5px;
    }

    .activity-banner {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        padding: 0 60px;
    }

    .activity-banner .activity-banner-time, .activity-banner .activity-banner-icon {
        width: 50%;
        padding: 0 5px 5px 5px;
    }

    .activity-banner .activity-banner-time {
        text-align: left;
        color: #c3c3c3;
        font-size: 1rem;
    }

    .activity-banner .activity-banner-icon {
        text-align: right;
    }

    .activity-banner .activity-banner-icon img {
        width: 15%;
        margin-left: 10px;
    }

    .bottom-solid {
        border-bottom: solid 1px #e6e6e6;
    }

    .coach-name-color {
        color: #566b96;
    }

    .activity-content-font {
        font-weight: bold;
    }
</style>
<template>
    <div>
        <div v-for="activePage in coachListPage">
        <div class="row bottom-solid" v-for="item in activePage">
            <div class="activity-coach">
                <div class="activity-coach-item-flex"><img :src="get_image_path(coachImg)" alt=""
                                                           class="img-responsive"></div>
                <div class="activity-coach-item-flex-2">
                    <div class="activity-coach-item-flex-column coach-name-color">{{coachName}}<span style="font-size:5px;">{{drivingName}}</span></div>
                    <div class="activity-coach-item-flex-column activity-content-font">{{item.activityContent}}</div>
                </div>
            </div>
            <div class="activity-image">
                <div class="activity-image-item" v-for="img in item.activityImage"><img :src="img_url+img.url"></div>
            </div>
            <div class="activity-banner">
                <div class="activity-banner-time">{{item.releaseTime}}</div>
                <div class="activity-banner-icon">
                    <div v-if="item.praiseCount===0"><img src="../images/praise-count-icon1.png" @click="updateActivityById(item)"><img
                            src="../images/share-icon.png" @click="gotoshare(item)"></div>
                    <div v-else>
                        <img src="../images/praise-count-icon.png" @click="updateActivityById(item)" ><span>{{item.praiseCount}}</span><img
                            src="../images/share-icon.png" @click="gotoshare(item)">
                    </div>
                </div>
            </div>
        </div>
        </div>
        <div class="row">
            <div class="col-xs-12" style="height:40px;text-align: center;" @click="nextPage">
                <div v-if="showmore" style="padding-top: 8px;">
                    <span>点击加载更多</span>
                </div>
                <div v-if="showPage">
                    <span>加载中...</span><img  src="../images/loadingPage.gif" style="height: 40px;width: 40px;">
                </div>
                <div v-if="showNull" style="padding-top: 8px;">
                    <span>无更多信息</span>
                </div>
            </div>
        </div>
        <div class="row" style="height:74px;"></div>
    </div>
</template>
<script>
    import * as types from "../store/mutation-types"
    module.exports = {
        data: function () {
            return {
                coach: [],
                coachListPage: [],
                pageInt:1,
                isDataNull:false,
                showPage:false,
                showmore:true,
                showNull:false,
                img_url:types.IMG_URL,
                get_image_path : types.GET_IMAGE_PATH
            }
        },
        props:['coachImg','coachName','drivingName','coachId'],
        created: function () {
            this.getActivityByCoachId();
        },
        methods: {
            nextPage:function(){
                if(this.isDataNull){return;}
                this.pageInt++;
                this.showPage=true;
                this.showmore=false;
                this.showNull=false;
                this.getActivityByCoachId();
            },
            getActivityByCoachId:function(){
                this.$http.post(types.HTTP_URL+'/activity/getAllActivityByCoachId',{pageIndex:this.pageInt,coachId:this.coachId},
                {emulateJSON:true}).then(function(response){
                        console.log(response.data.status);
                       if(response.data.status==1){
                            this.coach = response.data.list;
                            this.coachListPage.push(this.coach);
                            this.showPage=false;
                            this.showmore=true;
                            this.showNull=false;
                       }else{
                            this.isDataNull=true;
                            this.showPage=false;
                            this.showmore=false;
                            this.showNull=true;
                       }
                })
            },
            updateActivityById:function(item){
                var praiseCount = item.praiseCount+1;
                var coachupdate={activityId:item.activityId,coachId:item.coachId,activityContent:item.activityContent,
                                       releaseTime:item.releaseTime,praiseCount:praiseCount};
                this.$http.post(types.HTTP_URL+'/activity/upActivity',coachupdate,
                {emulateJSON:true}).then(function(response){
                    if(response.data.statue===1){
                           item.praiseCount = praiseCount;
                    }else{
                        console.log("更新失败");
                    }
                })
            },
            gotoshare:function(item){
                 this.$router.push({path:"/coachSharePage",name:"coachSharePage",params:{coachId:item.coachId,activityId:item.activityId}});
            },
        }
    }
</script>