<template>
  <div>
    <!-- 二级路由出口 -->
    <router-view></router-view>
    <!--  
        底部导航栏 
        router  当前是路由模式 符合vue-router path地址要和to属性去匹配
        
        -->
    <van-tabbar router v-model="active" active-color="#ee0a24" inactive-color="#000">
      <van-tabbar-item to='/home' icon="wap-home-o">首页</van-tabbar-item>
      <van-tabbar-item to='/cate' icon="apps-o">分类</van-tabbar-item>
      <van-tabbar-item to='/cart' icon="cart-o">购物车</van-tabbar-item>
      <van-tabbar-item to='/mine' icon="contact">个人中心</van-tabbar-item>
    </van-tabbar>
  </div>
</template>

<script>
export default {
  data() {
    return {
        active:0
    };
  }
};
</script>

<style lang="" scoped></style>
