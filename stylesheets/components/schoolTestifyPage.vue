<style>
    .testify-line {
        border-top: solid 5px #f0f0f0;
        padding-top: 5px;
        padding-bottom: 5px;
        position: relative;
    }

    .testify-line-title h4 {
        border-bottom: solid 1px #f0f0f0;
    }

    .testify-line-content {
        margin-top: 10px;
        color: #a5a5a5;
    }

    .testify-line-content p {
        text-indent: 2em;
    }

    .testify-line-personnel, .testify-line-personnel p {
        vertical-align: middle;
    }

    .testify-line-personnel img {
        border-radius: 50%;
        width: 68px;
        height: 68px;
    }

    .testify-line-personnel p {
        display: inline-block;
        margin-left: 10px;
    }

    .testify-line-personnel .span-gray {
        font-size: 1rem;
        color: #bbbbbb;
    }

    .testify-line-eligiblity-icon {
        position: absolute;
        top: 35%;
        right: 30%;
        width: 20%;
        height: auto;
        transform: rotate(10deg);
        -ms-transform: rotate(10deg);
        -moz-transform: rotate(10deg);
        -webkit-transform: rotate(10deg);
        -o-transform: rotate(10deg);
    }

    .testify-line-personnel .testify-line-img-list {
        float: right;
        position: relative;
    }

    .testify-line-personnel .testify-line-img-list img {
        width: 60px;
        height: 60px;
        border-radius: 0 !important;
    }

    .testify-line-personnel .testify-line-img-list div {
        height: 20px;
        width: 60px;
        text-align: center;
        font-size: .5rem;
        background: rgba(0, 0, 0, 0.5);
        color: white;
        position: absolute;
        bottom: 0;
        left: 0;
    }
</style>
<template>
    <div>
        <div class="row testify-line">
            <div class="col-xs-12 testify-line-title"><h4>约学车《驾校资质证明》</h4></div>
            <div class="col-xs-12 testify-line-personnel"><img :src="img_url + checkInfo.adminPhoto">
                <p><span>{{checkInfo.adminName}}</span><br><span class="span-gray">约学车资质审查员</span></p>
                <div class="testify-line-img-list"><img src="../images/car-img.png" alt="">
                    <div>更多图片</div>
                </div>
            </div>
            <div class="col-xs-12 testify-line-content"><p>
                经鉴定该驾校符合规定要求什么什么经鉴定该驾校符合规定要求什么什么经鉴定该驾校符合规定要求什么什么经鉴定该驾校符合规定要求什么什么</p></div>
            <img class="testify-line-eligiblity-icon" src="../images/eligibility-icon.png">
        </div>
    </div>
</template>
<script>
    import * as types from '../store/mutation-types'
    export default {
        date(){
           return{
               img_url:types.IMG_URL
           }
        },
        props: ["checkInfo"]
    }
</script>
