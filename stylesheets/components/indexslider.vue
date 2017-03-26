<template>
        <div class="row" style="margin-top:55px;">
                <!-- swiper -->
            <swiper :options="swiperOption">
                <swiper-slide v-for="swer in swipers">
                    <img :src='img_url+swer.sliderUrl' class="swiper-item" @click="goToHref(swer.sliderLink)">
                </swiper-slide>
                <div class="swiper-pagination" slot="pagination"></div>
            </swiper>

        </div>
</template>
<style>
.swiper-item{
    width:inherit;
    height:auto;
}
</style>
<script>
import * as types from "../store/mutation-types"
require("../css/swiper.css");
export default {
  data(){
    return {
        swiperOption: {
            pagination: '.swiper-pagination',
            paginationClickable: true,
            autoHeight: true
        },
        //swipers:{status:1,
        //        data:[{ID:1,Img:"2017-02-13/58a14a48ea807.jpg",URL:"2017-02-13/58a14a48ea807.jpg",swer_src:"/20160209/one.png",swer_link:"111"},
        //        {ID:1,swer_name:"twoimg",swer_src:"/20160209/sfdsa.png",swer_link:"222"}
        //]}
        swipers:{},
        img_url:types.IMG_URL
    }
  },
  created:function(){
            this.initview();
            console.log()
  },
  props:["sliderType"],
  methods:{
    initview:function(){
                    this.$http.post(types.HTTP_URL+'/sliderInfo/selectByType',{sliderType:this.sliderType},
                    {   emulateJSON:true}).then(function(response){
                            console.log(response.data)
                            if(response.data.state==1){
                            console.log(response.data.list)
                            this.swipers=response.data.list;
                            }else{
                            console.log("this is  null");
                            }
                     })
    },
    goToHref:function(link){
        location.href="http://www.baidu.com";
    }
  }
}
</script>
