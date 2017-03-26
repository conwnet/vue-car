<template>
    <div>
        <div class="row order-header-line">
            <div style="margin-top: 9px;">
                <strong>订单</strong>
            </div>
        </div>
        <div  class="row" style="background-color:#eeeeee;">
            <div style="text-align: center;padding:40px;">
                        <div class="row" style="background: transparent;"> <img src="../images/no-order.png" style="width: 100px;"/></div>
                        <div class="row" style="margin-top: 15px;color:#3d4245;background: transparent;"><strong>您还没有相关订单</strong></div>
                        <div class="row" style="background: transparent;">
                            <div class="col-xs-2"></div>
                            <div class="col-xs-4">
                                <div class="find_coach_school" @click="finddriningschool">
                                    找驾校
                                </div>

                            </div>
                            <div class="col-xs-4">
                                <div class="find_coach_school" @click="findcoach">
                                    找教练
                                </div>
                            </div>
                            <div class="col-xs-2"></div>
                        </div>
            </div>
        </div>
        <component is="indextopdrivingschool"></component>
        <component is="indextopcoachcommend"></component>
    </div>
</template>
<style>
    .order-header-line{
       height:44px;
       text-align:center;
       background-color: #29a1f7;
       color:#ffffff;
    }
    .find_coach_school{
        border:solid 1px #d2d2d4;
        margin-top: 2px;
        border-radius: 3px;
        color:#999999;
    }
</style>
<script>

    export default{
        data(){
            return{

            }
        },
         components:{
            'indextopdrivingschool':require('../components/indexTopDrivingSchool.vue'),
            'indextopcoachcommend':require('../components/indexTopCoachCommend.vue')
        },
        methods:{
            finddriningschool:function(){
                this.$router.push({path: 'findschool'});
            },
            findcoach:function(){
                this.$router.push({path: 'findcoach'});
            }
        }
    }

</script>
