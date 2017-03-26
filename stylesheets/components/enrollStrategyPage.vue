<style>
    .swiper-item {
        width: inherit;
        height: auto;
    }

    .strategy-list {
        margin-top: 2px;
        margin-bottom: 2px;
    }

    .strategy-list a {
        color: #0d0d0d;
    }

    .strategy-list .strategy-list-title {
        font-weight: bold;
        font-size: 1.6rem;
    }
    .strategy-list a .col-xs-4,.strategy-list a .strategy-list-title{
        padding-top: 15px;
        padding-bottom: 15px;
    }
    .strategy-list a .strategy-list-title{
        padding-left: 0;
    }
</style>
<template>
    <div>
        <div class="row page-title">报名攻略</div>
        <div class="row">
            <!-- swiper -->
            <swiper :options="swiperOption">
                <swiper-slide v-for="swer in swipers">
                    <img :src="imageUrl + swer.sliderUrl" class="swiper-item"
                         @click="showthis(swer.sliderLink)">
                </swiper-slide>
                <div class="swiper-pagination" slot="pagination"></div>
            </swiper>
        </div>
        <div class="row strategy-list" v-for="item in strategyList">
            <a :href="item.link">
                <div class="col-xs-4"><img class="img-responsive" src="imageUrl + item.image"
                                           alt=""></div>
                <div class="col-xs-8 strategy-list-title">{{item.title}}</div>
            </a>
        </div>
    </div>
</template>

<script>
    import * as types from "../store/mutation-types";
    module.exports = {
        data: function () {
            return {
                swiperOption: {
                    pagination: '.swiper-pagination',
                    paginationClickable: true,
                    autoHeight: true
                },
                swipers: {},
                strategyList:{},
                sliderType : 5,
                imageUrl :types.IMG_URL
            }
        },
        created: function () {
            //this.getCode();
            this.initview();
            this.getList();
        },
        methods: {
            showthis: function (s) {
                alert(s);
            },
            initview: function () {
                //获取轮播
                this.$http.post(types.HTTP_URL+'/sliderInfo/selectByType',{sliderType:this.sliderType},
                        {emulateJSON:true}).then(function(response){
                        this.swipers = response.data.list;
                })
            },
            getList : function(){
                //获取数据
                this.$http.post(types.HTTP_URL+'/enrollStrategy/getEnrollStrategyShow',{},
                        {emulateJSON:true}).then(function(response){
                    this.strategyList = response.data.list;
                })
            }
        },
        components: {}
    }
</script>