<template>
  <div class="tab">
    <div class="tab-list">
      <div
        class="tab-item"
        v-for="item in tabList"
        :key="item.value"
      >
        <img class="tab-item__img" :class="item.className" :src="item.url" />
      </div>
    </div>
    <div class="flur"></div>
  </div>
</template>

<script>
export default {
  name: 'Tab',
  data () {
    return {
      tabList: [{
        value: 1,
        className: 'tab-item__img-home',
        url: require('../assets/tab_home.png'),
      }, {
        value: 2,
        className: 'tab-item__img-burger',
        url: require('../assets/tab_burger.png'),
      }, {
        value: 3,
        className: 'tab-item__img-drink',
        url: require('../assets/tab_drink.png'),
      }, {
        value: 4,
        className: 'tab-item__img-snack',
        url: require('../assets/tab_snack.png'),
      }]
    }
  }
}
</script>

<style scoped>
.tab-list {
  z-index: 10;
  position: relative;
  width: 100%;
  height: 73px;
  display: flex;
  align-items: center;
  justify-content: space-around;
  background: linear-gradient(324.97deg, #FD003C -11%, #FF5D79 105.7%);
}
.tab-item__img {
  width: 34px;
  height: 34px;
}
.tab-item__img-home {
  width: 21px;
  height: 31px;
}
.tab-item__img-burger {
  width: 31px;
  height: 32px;
}
.tab-item__img-drink {
  width: 25.5px;
  height: 35.5px;
}
.tab-item__img-snack {
  width: 34px;
  height: 34px;
}
.flur {
  z-index: 9;
  position: absolute;
  top: 25px;
  margin: auto;
  width: 86%;
  height: 61px;
  background: linear-gradient(180deg, #FF0C46 -8.71%, #FFD7CC 126.67%);
  mix-blend-mode: normal;
  opacity: 0.78;
  filter: blur(27.8996px);
}
</style>
