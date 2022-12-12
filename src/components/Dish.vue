<template>
  <div class="dish">
    <div class="dish-container">
      <div :class="countType">
        <img
          class="item" v-for="(item, index) in list.slice(0, 3)"
          :key="index"
          :src="item.dishImg"
          :class="item.value"
          :style="`width: ${item.dishSize}px; height: ${item.dishSize}px`"
        />
      </div>
    </div>
  </div>
</template>

<script>
import Tab from './Tab';
import List from './List';


export default {
  name: 'Dish',
  components: {
    Tab,
    List
  },
  props: {
    list: [],
  },
  data () {
    return {
    }
  },
  computed: {
    countType() {
      if (this.list.length == 2) {
        return 'two-type';
      }
      return 'one-type';
    }
  }
}
</script>

<style scoped>
.dish {
  height: 50%;
  border-top: 3px solid #F2F1F1;
}
.dish-container {
  position: relative;
  margin: 72px auto 0;
  width: 256.5px;
  height: 97px;
  background-image: url('../assets/dish.png');
  background-size: cover;
}
.item {
  position: absolute;
  top: -20px;
  width: 94px;
  height: 94px;
}
.coffee {
  margin-top: -20px !important;
}
.burger {
  margin-top: 20px !important;
}
.one-type .item:nth-child(1) {
  top: -50px;
  left: 0;
  right: 0;
  margin: auto;
}
.one-type .item:nth-child(2) {
  left: 40px;
}
.one-type .item:nth-child(3) {
  right: 40px;
}
.two-type .item:nth-child(1) {
  top: -50px;
  left: 90px;
}
.two-type .item:nth-child(2) {
  left: 40px;
}
</style>
