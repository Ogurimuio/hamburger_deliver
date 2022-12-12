<template>
  <div class="main">
    <Tab />
    <div class="container">
      <List @add="addHandler" />
      <Dish :list="chooseList" />
    </div>
    <Connect />
    <Total :price="price" @pay="payHandler"/>
  </div>
</template>

<script>
import Tab from './Tab';
import List from './List';
import Dish from './Dish';
import Connect from './Connect';
import Total from './Total';

export default {
  name: 'Main',
  components: {
    Tab,
    List,
    Dish,
    Connect,
    Total
  },
  data () {
    return {
      chooseList: [],
      price: 0,
    }
  },
  methods: {
    addHandler(item) {
      this.chooseList.push(item);
      this.price = this.price + item.price;
    },

    payHandler() {
      this.chooseList = [];
      this.price = 0;
    }
  }
}
</script>

<style scoped>
.main {
  height: 100vh;
}
.container {
  /* 总高度 - tab栏 - 结算栏 */
  height: calc(100vh - 73px - 65px);
  background: linear-gradient(180deg, #F5F5F5 0%, #FFEDED 178.57%);
}
</style>
