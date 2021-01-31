<template>
  <div>
    <!-- 导航栏 -->
    <van-nav-bar title="购物车列表" left-arrow @click-left="$router.back()" />
    <!-- 商品列表 -->
    <van-list v-if="cartList.length > 0">
      <van-swipe-cell v-for="item in cartList" :key="item.id">
        <van-card
          :price="item.price.toFixed(2)"
          desc="描述信息"
          :title="item.goodsname"
          :num="item.num"
          :thumb="
            item.img
              ? $imgUrl + item.img
              : 'https://img.yzcdn.cn/vant/ipad.jpeg'
          "
        >
          <template #footer>
            <van-stepper
              v-model="item.num"
              theme="round"
              button-size="22"
              disable-input
            />
          </template>
        </van-card>
        <template #right>
          <van-button
            @click="delCart(item.id)"
            square
            text="删除"
            type="danger"
            class="delete-button"
          />
        </template>
      </van-swipe-cell>
    </van-list>
    <!-- 无商品列表 -->
    <van-list v-else
      ><van-empty description="购物车空空如也快去买买买！！！"
    /></van-list>
  </div>
</template>

<script>
//引入接口
import { getCartList, deleteCart } from "../util/axios";
//引入弹框
import { Dialog, Toast } from "vant";
export default {
  data() {
    return {
      cartList: [],
      num: 1 //是商品数量
    };
  },
  mounted() {
      this.getCartList()
  },
  methods: {
    //购物车列表
    getCartList() {
      /* 
      this.$route.query.uid || JSON.parse(sessionStorage.getItem('userInfo')).uid

      如果你是从商品详情跳转过来，取值取的是商品详情的id
      如果不是，从存储中取出uid
      */
      getCartList({
        uid:
          this.$route.query.uid ||
          JSON.parse(sessionStorage.getItem("userInfo")).uid
      }).then(res => {
        if (res.code == 200) {
          this.cartList = res.list ? res.list : [];
        }
      });
    },
    //封装一个购物车删除事件
    delCart(id) {
      deleteCart({
        id
      }).then(res => {
        if (res.code == 200) {
            this.getCartList()
          Toast.success(res.msg);
        } else {
          Toast.fail(res.msg);
        }
      });
    }
  },
  //进入组件前的组件守卫
  beforeRouteEnter(to, from, next) {
    console.log(to, "totototot");
    console.log(from, "fromfromfromfrom");
    console.log(sessionStorage.getItem("userInfo"), "会话存储");
    if (sessionStorage.getItem("userInfo")) {
      //如果已经登录，直接放行，跳转到购物车
      next();
    } else {
      Dialog.confirm({
        title: "未登录",
        message: "未登录不能查看购物车，快登录！！！"
      })
        .then(() => {
          // on confirm
          console.log("跳到登录");
          next("/login");
        })
        .catch(() => {
          //取消就回到上一页
          next(from.path);
        });
    }
  }
};
</script>

<style lang="" scoped></style>
