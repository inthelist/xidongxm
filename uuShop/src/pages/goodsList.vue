<template>
  <div>
    <!-- 导航栏 -->
    <van-nav-bar title="商品列表" left-arrow @click-left="$router.back()" />
    <!-- 商品列表 -->
    <van-list v-if="goodsList.length > 0">
      <van-card
        v-for="item in goodsList"
        :key="item.id"
        :price="item.price.toFixed(2)"
        desc="描述信息"
        :title="item.goodsname"
        :thumb="
          item.img ? $imgUrl + item.img : 'https://img.yzcdn.cn/vant/ipad.jpeg'
        "
        @click="goDetail(item.id)"
      />
    </van-list>
    <!-- 无商品列表 -->
    <van-list v-else><van-empty description="暂无商品"/></van-list>
  </div>
</template>

<script>
//引入接口
import { getGoods } from "../util/axios";
export default {
  data() {
    return {
      goodsList: []
    };
  },
  mounted() {
    getGoods({
      fid: this.$route.query.id
    }).then(res => {
      if (res.code == 200) {
        this.goodsList = res.list ? res.list : [];
      }
    });
  },
  methods: {
    //封装一个跳转详情事件
    goDetail(id){
      this.$router.push({
        path:'/detail',
        query:{
          id
        }
      })
    }
  },
};
</script>

<style lang="" scoped></style>
