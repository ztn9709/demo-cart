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
      :state="item.goods_state"
      @change-state="getNewState"
    >
      <Counter
        :count="item.goods_count"
        @count-change="getNewCount(item, $event)"
      ></Counter>
    </Good>
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
import Counter from "@/components/Counter/Counter.vue";

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
    Footer,
    Counter
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
    getNewCount(item, e) {
      item.goods_count = e;
    },
    fullSelect(val) {
      this.list.forEach(item => {
        item.goods_state = val;
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
