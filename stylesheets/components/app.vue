<template>
    <div>
        <loading v-if="loading"></loading>
        <div v-if="!loading">
            <component is='headerpage'></component>
            <component is='indexslider' :sliderType="1"></component>
            <component is="indexgrids"></component>
            <component is="indextopdrivingschool"></component>
            <component is="indextopcoachcommend"></component>
            <component is='footerpage' :isSelected='1'></component>
        </div>
        <iframe id="geoPage" width=0 height=0 frameborder=0 style="display:none;" scrolling="no"
                src="https://apis.map.qq.com/tools/geolocation?key=IEPBZ-VSL6J-NUBFZ-FMQOW-WQSL3-QNF7D&referer=约学车">
        </iframe>
    </div>
</template>
<style>
    body {
    }
</style>
<script>
    require("../css/news.css")
    import * as types from "../store/mutation-types"
    import * as storage from "../store/localStorage"
    export default{
        data(){
            return {
                msg: 'hello vue',
                loading: true,
                loc:{//默认沈阳定位
                    lng :123.432253,
                    lat :41.805865
                }
            }
        },
        created: function () {
            storage.removeLocalStorageItem(types.LAT);
            storage.removeLocalStorageItem(types.LNG);
            this.getLonAndLat();
            this.waitLoading();
        },
        methods: {
            waitLoading: function () {
                var self = this;
                setTimeout(self.setLocalStor, 3000)
            },
            getLonAndLat: function () {
                var self = this;
                window.addEventListener('message', function (event) {
                    // 接收位置信息
                    var loc = event.data;
                    console.log("loc =" + JSON.stringify(loc));
                    if (loc && loc.lat && loc.lng) {
                        self.loc = loc;
                    }
                }, false)
            },
            setLocalStor: function () {
                this.loading = false;
                storage.setStrLocalStorageItem(types.LNG, this.loc.lng);
                storage.setStrLocalStorageItem(types.LAT, this.loc.lat);
               // alert("lat =" + storage.getStrLocalStorageItem(types.LAT));
               // alert("lng =" + storage.getStrLocalStorageItem(types.LNG));
            }
        },
        components: {
            'indexslider': require('../components/indexslider.vue'),
            'indexgrids': require('../components/indexgrids.vue'),
            'indextopdrivingschool': require('../components/indexTopDrivingSchool.vue'),
            'indextopcoachcommend': require('../components/indexTopCoachCommend.vue'),
            'footerpage': require('../components/footerPage.vue'),
            'headerpage': require('../components/headerPage.vue'),
            'loading': require('../components/loading.vue')
        }
    }
</script>
