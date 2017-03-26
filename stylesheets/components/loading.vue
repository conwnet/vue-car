<template>
    <div style="text-align:center;">
        <div class="back_div">
            <div class="weiqi_div">
            </div>
            <img src="../images/jejloading1.png" class="loading_img">
        </div>
    </div>
</template>
<style>
      .back_div{
        position: absolute;
        top: 18%;
        left: 36%;
        width: 120px;
        height: 80px;
      }
      .weiqi_div{
        width: 44px;
        height: inherit;
        position: absolute;
        right: 0;
        background: #f1f1f1;
        -webkit-animation: audio_playing 1s infinite;
      }
      .loading_img{
            width: inherit;
            height: inherit;
      }
    .loading_imgs {
            vertical-align: middle;
            display: inline-block;
            -webkit-background-size: contain;
            background-size: contain;
            -webkit-animation: audio_playing 0.4s infinite;
            background-position: 1px center;
            position: relative;
            bottom: 59%;
    }
    @-webkit-keyframes audio_playing {
    25% {
        width:44px;
    }
    26% {
        width:33px;
    }
    51%{
        width:33px;
    }
    51.5%{
        width:20px;
    }
    71.5%{
        width:20px;
    }
    72%{
        width:0px;
    }
    100% {
        width:0px;
    }
}
    body{
        /*background-color:#fff;*/
    }
</style>
<script>

</script>
