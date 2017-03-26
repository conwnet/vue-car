<style>
    .weui-tabbar{
        position: fixed !important;
    }
    .weui-tabbar__item.weui-bar__item_on .weui-tabbar__label{
        color: #29a1f7 !important;
    }
    .weui-tabbar a{
        text-decoration:none;
    }
</style>
<template>
    <div class="weui-tabbar" style="left: 0;">
        <a v-if="isSelected == 1" href="javascript:;" class="weui-tabbar__item weui-bar__item_on" @click="jumpPage(1)">
            <img src="../images/home-icon-blue.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">首页</p>
        </a>
        <a v-else href="javascript:;" class="weui-tabbar__item" @click="jumpPage(1)">
            <img src="../images/home-icon-gray.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">首页</p>
        </a>
        <a v-if="isSelected == 2" href="javascript:;" class="weui-tabbar__item weui-bar__item_on" @click="jumpPage(2)">
            <img src="../images/driverSchool-icon-blue.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">找驾校</p>
        </a>
        <a v-else href="javascript:;" class="weui-tabbar__item" @click="jumpPage(2)">
            <img src="../images/driverSchool-icon-gray.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">找驾校</p>
        </a>
        <a v-if="isSelected == 3" href="javascript:;" class="weui-tabbar__item weui-bar__item_on" @click="jumpPage(3)">
            <img src="../images/coach-icon-blue.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">找教练</p>
        </a>
        <a v-else href="javascript:;" class="weui-tabbar__item" @click="jumpPage(3)">
            <img src="../images/coach-icon-gray.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">找教练</p>
        </a>
        <a v-if="isSelected == 4" href="javascript:;" class="weui-tabbar__item weui-bar__item_on" @click="jumpPage(4)">
            <img src="../images/activity-icon-blue.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">活动</p>
        </a>
        <a v-else href="javascript:;" class="weui-tabbar__item" @click="jumpPage(4)">
            <img src="../images/activity-icon-gray.png" alt="" class="weui-tabbar__icon">
            <p class="weui-tabbar__label">活动</p>
        </a>
    </div>
</template>
<script>
    module.exports = {
        data: function () {
           return{
           }
        },
        props:["isSelected"],
        methods : {
            jumpPage: function(i){
                switch (i){
                    case 1:
                        this.$router.push("/");
                        break;
                    case 2:
                        this.$router.push("/findschool");
                        break;
                    case 3:
                        this.$router.push("/findcoach");
                        break;
                    case 4:
                        this.$router.push("/activityPage");
                        break;
                }
            }
        }
    }
</script>