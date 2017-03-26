<style>
    .screening-flex {
        display: flex;
        display: -webkit-box;
        display: -webkit-flex;
        text-align: center;
        border-top: solid 10px #f0efed;
        border-bottom: solid 1px #f0efed;
        height: 55px;
        position: relative;
    }

    .screening-flex .screening-flex-item {
        flex: 1;
        height: 44px;
        line-height: 44px;
    }

    .screening-flex .screening-flex-item img {
        width: 15px;
        height: 10px;
    }

    .list-item {
        position: absolute;
        display: none;
        top: 55px;
        left: 0;
        z-index: 600;
        width: 100%;
    }

    #screening-list {
        display: -webkit-box;
        display: -webkit-flex;
        display: flex;
        flex-direction: row;
        flex-wrap: wrap;
        text-align: center;
    }

    #screening-list .screening-list-flex {
        width: 25%;
        margin-bottom: 20px;
        padding: 0 5px;
    }

    #screening-list .screening-list-flex div {
        border: solid 1px #d7d7d7;
        border-radius: 3px;
        padding: 5px;
        background-color: white;
        font-size: 1.5rem;
        overflow: hidden;
        white-space: nowrap;
    }

    .sereening-selected-tag {
        border-color: #ff6634 !important;
        background-color: #fef0ed !important;
        color: #ff6634 !important;
    }

    #screening-list .fun-but {
        width: 50%;
        padding: 0 5px;
    }

    #screening-list .fun-but div {
        display: inline-block;
        border: solid 1px #d7d7d7;
        border-radius: 3px;
        padding: 5px;
        font-size: 1rem;
        background-color: white;
        text-align: center;
        width: 100%;
    }

    .color-blue {
        border-color: #00a0ea !important;
        color: #00a0ea !important;
    }

    #region-list {
        width: 100%;
    }

    #region-list .region-list-title {
        padding: 0 10px 15px 10px;
        text-align: left;
    }

    #region-list .region-list-item {
        height: 100%;
        width: 100%;
        text-align: left;
        background-color: #f4f4f4;
    }

    #region-list .region-list-item div {
        padding: 10px 10px;
    }

    #noopsyche-list{
        width: 100%;
    }
    #noopsyche-list .noopsyche-list-title{
        color:#ff7f00;
        padding: 0 10px 15px 10px;
        text-align: left;
        border-bottom:solid 1px #ff7f00;
    }
    #noopsyche-list .noopsyche-list-item{
        height: 100%;
        width: 100%;
        text-align: left;
    }
    #noopsyche-list .noopsyche-list-item div{
        padding: 10px 10px;
        border-bottom:solid 1px #d7d7d7;
    }

    .bubble-arrow-screening-list {
        border-color:transparent transparent #f0efed transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 34px;
        left: 8%;
    }
    .bubble-arrow-inner-screening-list {
        border-color:transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 36px;
        left: 8%;
    }

    .bubble-arrow-region-list {
        border-color:transparent transparent #f0efed transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 34px;
        left: 58%;
    }
    .bubble-arrow-inner-region-list {
        border-color:transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 36px;
        left: 58%;
    }

    .bubble-arrow-noopsyche-list {
        border-color:transparent transparent #f0efed transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 34px;
        left: 83%;
    }
    .bubble-arrow-inner-noopsyche-list {
        border-color:transparent transparent #FFFFFF transparent;
        border-width: 10px;
        border-style: solid;
        width: 0;
        height: 0;
        position: absolute;
        top: 36px;
        left: 83%;
    }
</style>
<template>
    <div class="row" style="position: relative">
        <div class="screening-flex">
            <div class="screening-flex-item">
                <div class="screening-flex-tag" @click="clickTag('screening-list')">筛选<img
                        src="../images/arrow-down-icon.png"></div>
            </div>
            <div class="screening-flex-item">
                <div class="screening-flex-tag">口碑<img src="../images/arrow-down-icon.png"></div>
            </div>
            <div class="screening-flex-item">
                <div class="screening-flex-tag" @click="clickTag('region-list')">区域<img
                        src="../images/arrow-down-icon.png"></div>
            </div>
            <div class="screening-flex-item">
                <div class="screening-flex-tag" @click="clickTag('noopsyche-list')">智能<img src="../images/arrow-down-icon.png"></div>
            </div>
            <div class="list-item screening-list">
                <div id="screening-list">
                    <div class="screening-list-flex" v-for="item in screeningList">
                        <div @click="selectedTag($event)">{{item.name}}</div>
                    </div>
                    <div class="fun-but">
                        <div @click="clickTag('screening-list')">取消</div>
                    </div>
                    <div class="fun-but">
                        <div class="color-blue">确认</div>
                    </div>
                </div>
            </div>
            <div id="praise-list" class="list-item">

            </div>
            <div class="list-item region-list">
                <div id="region-list">
                    <div class="region-list-title">附近</div>
                    <div class="region-list-item">
                        <div v-for="item in regionList" @click="clickTag('region-list')">{{item.name}}</div>
                    </div>
                </div>
            </div>

            <div class="list-item noopsyche-list">
                <div id="noopsyche-list">
                    <div class="noopsyche-list-title">智能排序</div>
                    <div class="noopsyche-list-item">
                        <div v-for="item in noopsycheList" @click="clickTag('noopsyche-list')">{{item.name}}</div>
                    </div>
                </div>
            </div>
        </div>
        <div id="bubble-arrow"></div>
        <div id="bubble-arrow-inner"></div>
    </div>
</template>
<script>
    module.exports = {
        data: function () {
            return {
                previousTag: "",
                screeningList: [{name: "脾气好"}, {name: "夜间培训"}, {name: "一人一车"}, {name: "美女教练"}, {name: "不吸烟"}, {name: "资深教练"}, {name: "一票到底"}, {name: "包接送"}, {name: "离市区近"}, {name: "一对一"}, {name: "免补考费"}, {name: "吃饭AA"}],
                regionList: [{name: "热门商区"}, {name: "和平区"}, {name: "沈河区"}, {name: "铁西区"}, {name: "大东区"}, {name: "皇姑区"}, {name: "东陵区"}, {name: "于洪区"}, {name: "沈北新区"}, {name: "浑南新区"}, {name: "苏家屯区"}],
                noopsycheList :[{name:"离我最近"},{name:"人气最高"},{name:"评价最好"},{name:"价格最低"},{name:"下票最快"},{name:"服务最佳"},{name:"合格率最高"}],
                screeningCount:0
            }
        },
        created: function () {
        },
        methods: {
            clickTag: function (id) {
                if (this.previousTag != "" && this.previousTag != id) {
                    $("." + this.previousTag).hide();
                    $("#bubble-arrow").removeClass("bubble-arrow-" + this.previousTag);
                    $("#bubble-arrow-inner").removeClass("bubble-arrow-inner-" + this.previousTag);
                }
                $("#bubble-arrow").toggleClass("bubble-arrow-" + id);
                $("#bubble-arrow-inner").toggleClass("bubble-arrow-inner-" + id);
                $("." + id).toggle();
                this.previousTag = id;
            },
            selectedTag: function (e) {
                var el = e.currentTarget;
                if(this.screeningCount == 4 && !el.getAttribute("class", "sereening-selected-tag")){
                    alert("最多选4个");
                } else {
                    if (el.getAttribute("class", "sereening-selected-tag")) {
                        el.removeAttribute("class", "sereening-selected-tag");
                        this.screeningCount--;
                    } else {
                        el.setAttribute("class", "sereening-selected-tag");
                        this.screeningCount++;
                    }
                }
//                el.getAttribute("class", "sereening-selected-tag") ? el.removeAttribute("class", "sereening-selected-tag") : el.setAttribute("class", "sereening-selected-tag");
                }
            }
        }
    }
</script>