<template>
  <div class="body">
    <van-swipe class="my-swipe" :autoplay="3000" indicator-color="white">
      <van-swipe-item v-for="it in banner" :key="it.picUrl">
        <img :src="it.picUrl" alt="" />
      </van-swipe-item>
    </van-swipe>
    <p style="text-align: center">商品列表</p>

    <van-list
    :immediate-check="false"
      v-model:loading="loading"
      :offset="30"
      :finished="finished"
      finished-text="没有更多了"
      @load="onLoad"
    >
      <div class="box">
        <div class="item" v-for="it in shopdata" :key="it.id" @click="$router.push('/info?id='+it.id)">
          <img :src="it.pic" alt="" />
          <p class="val">{{ it.name }}</p>
          <div class="btn">
            <p>
              <span style="color: darkgreen">￥{{ it.numberFav }}</span>
              <s>￥{{ it.originalPrice }}</s>
            </p>
            <button>购买</button>
          </div>
        </div>
      </div>
    </van-list>

    <van-back-top bottom="10vh" />
  </div>
</template>
<script setup lang='ts'>
type banners = {
  picUrl: string;
};
import { reactive, ref } from "vue";
const banner = ref<banners[]>([]);
import { lunbo, shopApi } from "./utils/api";
lunbo().then((res) => {
  banner.value = res.data.data;
});
const shoplist = reactive({
  page: 1,
  pageSize: 10,
  num:0
});

const shopdata = ref<shops[]>([]);
type shops = {
  name: string;
  originalPrice: string;
  pic: string;
  numberFav: string;
  id: string;
};

function get() {
  shopApi(shoplist).then((res) => {
    shopdata.value = res.data.data.result;
    shoplist.num=res.data.data.totalRow
  });
}
get()

const loading = ref(false);
const finished = ref(false);

const onLoad = () => {
   console.log(1)
    if (shopdata.value.length>=shoplist.num) {
        finished.value=true
    }else{
        shoplist.pageSize+=5
        get()
    }
    loading.value = false;
};
</script>
<style scoped lang='scss'>
.body {
  width: 100%;
  margin-bottom: 100px;
}
.my-swipe img {
  width: 100%;
  height: 200px;
}

.box {
  width: 100%;
  display: flex;
  background-color: #ccc;
  flex-wrap: wrap;

  .item {
    padding: 10px;
    background-color: #fff;
    width: 50%;
    img {
      width: 100%;
    }
  }
  .btn {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
}
</style>