<style>
    .self-row {
        margin-right: -15px;
        margin-left: -15px;
        padding: 15px;
    }

    /*.screening-list-release {*/
    /*position: relative;*/
    /*top: 15px;*/
    /*background-color: #f1f1f1;*/
    /*}*/

    #starLine {
        padding: 0 5px;
        display: inline-block;
    }

    #starLine img {
        padding: 0 5px;
    }

    .active-content {
        padding: 15px;
        columns: 5;
        height: 100px;
    }

    .btn-default {
        width: 100%;
        padding: 0 15px;
        background-color: #29a1f7;
        height: 40px;
        color: white;
    }
</style>
<template>
    <div>
        <div class="page-title row">评论</div>
        <div class="evaluate-name self-row">{{name}}</div>
        <div class="self-row" style="background-color: white;">
            评分:
            <div id="starLine"><img v-for="item in lightStar" @click="lightStar = item"
                                    src="../images/lightstar.png"
                                    alt=""><img v-for="item in 5-lightStar" src="../images/graystar.png" alt=""
                                                @click="lightStar += item">
            </div>
        </div>
        <!--<div class="list-item screening-list screening-list-release" style="display: block;">-->
        <!--<div id="screening-list">-->
        <!--<div class="screening-list-flex" v-for="item in screeningList">-->
        <!--<div @click="selectedTag($event)">{{item.name}}</div>-->
        <!--</div>-->
        <!--</div>-->
        <!--</div>-->
        <div class="self-row" style="padding-left: 0;padding-right: 0;">
            <textarea placeholder="亲,教练专业吗?环境怎么样,服务满意吗?" class="active-content" v-model="commentContent"> </textarea>
        </div>
        <div class="self-row" style="text-align: center">
            <button type="button" class="btn btn-default" @click="addComment">发表</button>
        </div>

        <div id="dialogs">
            <div class="js_dialog" id="dialogs_comment" style="display: none;">
                <div class="weui-mask"></div>
                <div class="weui-dialog">
                    <div class="weui-dialog__bd">{{dialosText}}</div>
                    <div class="weui-dialog__ft">
                        <a href="javascript:;" class="weui-dialog__btn weui-dialog__btn_primary" @click="closeDialog">好的</a>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
    import * as types from '../store/mutation-types'
    import * as storage from '../store/localStorage'
    module.exports = {
        data: function () {
            return {
                pid: "",
                name: "",
                type: "",
                commentContent : "",
//                screeningList: [{name: "脾气好"}, {name: "夜间培训"}, {name: "一人一车"}, {name: "美女教练"}, {name: "不吸烟"}, {name: "资深教练"}, {name: "一票到底"}, {name: "包接送"}, {name: "离市区近"}, {name: "一对一"}, {name: "免补考费"}, {name: "吃饭AA"}],
//                screeningCount: 0,
                lightStar: 0,
                params : {},
                dialosText : ""
            }
        },
        created: function () {
            this.pid = this.$route.params.pid;//驾校或教练id
            this.name = this.$route.params.name;//驾校或教练名字
            this.type = this.$route.params.type;//1驾校 2教练
        },
        methods: {
//            selectedTag: function (e) {
//                var el = e.currentTarget;
//                if (this.screeningCount == 4 && !el.getAttribute("class", "sereening-selected-tag")) {
//                    alert("最多选4个");
//                } else {
//                    if (el.getAttribute("class", "sereening-selected-tag")) {
//                        el.removeAttribute("class", "sereening-selected-tag");
//                        this.screeningCount--;
//                    } else {
//                        el.setAttribute("class", "sereening-selected-tag");
//                        this.screeningCount++;
//                    }
//                }
//            }
            addComment: function () {
                if($.trim(this.commentContent) == ""){//验证评论
                   this.setDialos("请输入评论内容");
                    return false;
                }
                if(this.lightStar == 0)this.lightStar = 5;//评分默认5分
                this.params['commentContent'] = this.commentContent;
                this.params['commentScore'] = this.lightStar;
                if(this.type == 1){//1 学校 2教练
                    this.params['drivingSchoolId'] = this.pid;
                } else {
                    this.params['coachId'] = this.pid;
                }
//                this.params['commentUserId'] = storage.getJsonLocalStorageItem(types.CACHE_STUDENT_INFO).userId;//获取用户id
                this.params['commentUserId'] = "8d6df9c6-f818-4f9e-b39b-01923c74920b";
                this.$http.post(types.HTTP_URL+'/comment/addCommentInfo',this.params,
                        {emulateJSON:true}).then(function(response){
                        console.log( response.data.state);
                        if(response.data.state == 1){
                            this.setDialos("评论成功");
                        } else if(response.data.state == 2){
                            this.setDialos("已经发布过评论,无法重复发布");
                        } else {
                            this.setDialos("服务器忙,请重试");
                        }
                })
            },
            setDialos : function(text){
                this.dialosText = text;
                var $dialogs_comment = $('#dialogs_comment');
                $dialogs_comment.fadeIn(200);
            },
            closeDialog : function(){//关闭alert
                $(".weui-dialog__btn").parents('.js_dialog').fadeOut(200);
                 if(this.type==1){
                    this.$router.push({path:"/schoolInfoPage",name:"schoolInfoPage",params: { paramId:this.pid}});
                }else{
                    this.$router.push({path:"/coachInfoPage",name:"coachInfoPage",params: { paramId:this.pid}});
                }
            },
        }
    }
</script>