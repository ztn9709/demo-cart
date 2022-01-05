<template>
  <div class="app-container">
    <Header title="我的购物车"></Header>
    <Good
      v-for="item in list"
      :key="item.id"
      :id="item.id"
      :title="item.goods_name"
      :pic="item.goods_img"
      :price="item.goods_price"
      :count="item.goods_count"
      :state="item.goods_state"
      @change-state="getNewState"
      @listAdd="add"
      @listMinus="minus"
    ></Good>
    <Footer
      :isFull="fullState"
      :summary="SUM"
      :total="TOT"
      @is-full-select="fullSelect"
    ></Footer>
  </div>
</template>

<script>
import axios from "axios";
import Header from "@/components/Header/Header.vue";
import Good from "@/components/Goods/Goods.vue";
import Footer from "@/components/Footer/Footer.vue";

export default {
  data() {
    return {
      list: []
    };
  },
  computed: {
    fullState() {
      return this.list.every(item => {
        return item.goods_state;
      });
    },
    SUM() {
      let sum = 0;
      this.list.forEach(item => {
        if (item.goods_state === true) {
          sum += item.goods_price * item.goods_count;
        }
      });
      return sum;
    },
    TOT() {
      let tot = 0;
      this.list.forEach(item => {
        if (item.goods_state === true) {
          tot += item.goods_count;
        }
      });
      return tot;
    }
  },
  components: {
    Header,
    Good,
    Footer
  },
  methods: {
    async getCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      if (res.status === 200) {
        this.list = res.list;
      }
    },
    getNewState(e) {
      this.list.some((item, index) => {
        if (item.id === e.id) {
          item.goods_state = e.value;
          return true;
        }
      });
    },
    fullSelect(val) {
      this.list.forEach(item => {
        item.goods_state = val;
      });
    },
    add(obj) {
      this.list.some((item, index) => {
        if (item.id === obj.id) {
          item.goods_count = obj.value;
          return true;
        }
      });
    },
    minus(objm) {
      this.list.some((item, index) => {
        if (item.id === objm.id) {
          item.goods_count = objm.value;
          return true;
        }
      });
    }
  },
  created() {
    this.getCartList();
  }
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
