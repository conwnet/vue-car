<style>
    .desc-line {
        border-top: solid 5px #f0f0f0;
        padding-top: 5px;
        padding-bottom: 5px;
    }

    .desc-line-title h4{
        border-bottom: solid 1px #f0f0f0;
    }

    .desc-line-content {
        color: #a5a5a5;
    }
    .desc-line-content p{
        text-indent: 2em;
    }
</style>
<template>
    <div>
        <div class="row desc-line">
            <div class="col-xs-12 desc-line-title"><h4>{{Title}}</h4></div>
            <div class="col-xs-12 desc-line-content"><p>{{Desc}}</p></div>
        </div>
    </div>
</template>
<script>
    module.exports = {
        data: function () {
            return {}
        },
        props: ["Desc","Title"],
        created:function(){
            console.log(this.Title);
        }
    }
</script>