<template>
  <div class="app-container">
    <Header title="我的购物车"></Header>
  </div>
</template>

<script>
import axios from "axios";
import Header from "@/components/Header/Header.vue";
import Good from "@/components/Goods/Goods.vue";

export default {
  data() {
    return {
      list: [],
    };
  },
  components: {
    Header,
    Good,
  },
  methods: {
    async getCartList() {
      const { data: res } = await axios.get("https://www.escook.cn/api/cart");
      if (res.status === 200) {
        this.list = res.list;
      }
    },
  },
  created() {
    this.getCartList();
  },
};
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
