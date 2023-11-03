<template>
  <div>
    <img :src="data.pic" alt="">
    <p style="color: #63a48e; font-size: 19px">￥{{ data.amountSingle }}</p>
  </div>
</template>
<script setup lang='ts'>
import { reactive, ref } from "vue";
import { useRoute } from "vue-router";
const route = useRoute();
import {InfoApi} from '../utils/api'


const data=ref()


InfoApi({
  goodsId: route.query.id,
  page: 1,
  pageSize: 1
}).then(res=>{
    data.value=res.data.data.result[0].goods
})
</script>
<style scoped lang='scss'>
img{
    width: 100%;
}
</style>