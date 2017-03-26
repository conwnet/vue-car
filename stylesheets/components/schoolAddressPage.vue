<style>
    .address-line {
        border-top: solid 5px #f0f0f0;
        padding-top: 5px;
        padding-bottom: 5px;
    }

    .address-line .addr-icon img, .address-line .tel-icon img {
        vertical-align: middle;
    }

    .address-line .addr-icon img {
        width: 10%;
        height: auto;
    }
    .address-line .tel-icon{
        text-align: right;
    }
    .address-line .tel-icon img{
        width : 85%;
        height: auto;
    }
</style>
<template>
    <div>
        <div class="row address-line">
            <div class="col-xs-8 addr-icon"><img src="../images/loaction-icon.png" alt="">地址:{{Address}}</div>
            <div class="col-xs-4 tel-icon"><img src="../images/tel-info-icom.png" alt=""></div>
        </div>
    </div>
</template>
<script>
    module.exports = {
        data: function () {
            return {}
        },
        props: ["Address"]
    }
</script>