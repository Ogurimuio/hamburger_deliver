<template>
  <div class="list">
    <div class="photo-view" ref="photoView">
      <div class="star star_large" :style="getStarPostion('large')"></div>
      <div class="star star_medium" :style="getStarPostion('medium')"></div>
      <div class="star star_small" :style="getStarPostion('small')"></div>

      <div class="photo-list" ref="photoList" @scroll="scrollHandler">
        <div class="chip" :class="{ hide: curIndex !== 0 }">
          <img class="chip-sub_1" src="../assets/chip_1.png" />
          <img class="chip-sub_2" src="../assets/chip_2.png" />
          <img class="chip-main" src="../assets/chips.png" />
        </div>

        <div class="coffee">
          <img v-if="curIndex === 1" class="coffee-sub" src="../assets/coffee_sub.png" />
          <img class="coffee-main" src="../assets/coffee.png" />
        </div>

        <div class="burger" :class="{ active: curIndex === 2 }">
          <img class="burger-sub" src="../assets/burger_top.png" />
          <img class="burger-main" src="../assets/burger_bottom.png" />
        </div>
      </div>
    </div>
  
    <div class="info">
      <div class="info-name" :class="{'active': infoActive}">{{ listInfo[curIndex].name }}</div>
      <div class="info-price">{{ listInfo[curIndex].price }}$</div>
      <div class="info-add" @click="add"></div>
    </div>
  </div>
</template>

<script>
const STAR_LARGE_POSITION = {
  0: 'top: 0px; left: 160px',
  1: 'top: 170px; left: 140px',
  2: 'top: 130px; left: 20px',
};
const STAR_MEDIUM_POSITION = {
  0: 'top: 80px; left: 200px',
  1: 'top: 180px; left: 40px',
  2: 'top: 20px; left: 40px',
};
const STAR_SMALL_POSITION = {
  0: 'top: 110px; left: 20px',
  1: 'top: 50px; left: 160px',
  2: 'top: 200px; left: 160px',
};
const LIST_INFO = [{
  value: 'chips',
  name: 'FRIES',
  price: 4,
  dishImg: require('../assets/dish_chips.png'),
  dishSize: 94,
}, {
  value: 'coffee',
  name: 'LATTE',
  price: 3,
  dishImg: require('../assets/dish_coffee.png'),
  dishSize: 128,
}, {
  value: 'burger',
  name: 'BURGER',
  price: 6,
  dishImg: require('../assets/dish_burger.png'),
  dishSize: 101,
}];

export default {
  name: 'List',
  data () {
    return {
      curIndex: 0,
      curScrollLeft: 0,
      listInfo: LIST_INFO,
      width: 0,
      infoActive: false,
    }
  },
  mounted() {
    this.width = this.$refs.photoView.clientWidth;
    console.log('width', this.$refs.photoView);
  },
  methods: {
    getStarPostion(type) {
      const { curIndex } = this;
      if (type == 'large') {
        return STAR_LARGE_POSITION[curIndex];
      }
      if (type == 'medium') {
        return STAR_MEDIUM_POSITION[curIndex];
      }
      if (type == 'small') {
        return STAR_SMALL_POSITION[curIndex];
      }
    },
    activeInfo() {
      this.infoActive = true;
      setTimeout(() => {
        this.infoActive = false;
      }, 10);
    },
    scrollNext() {
      this.curIndex = this.curIndex + 1;
      const newScrollLeft = this.width * this.curIndex;
      this.$refs.photoList.scrollTo({ left: newScrollLeft });
      this.curScrollLeft = newScrollLeft;
      this.activeInfo();
    },
    scrollPre() {
      this.curIndex = this.curIndex - 1;
      const newScrollLeft = this.width * this.curIndex;
      this.$refs.photoList.scrollTo({ left: newScrollLeft });
      this.curScrollLeft = newScrollLeft;
      this.activeInfo();
    },
    scrollHandler() {
      const { scrollLeft } = this.$refs.photoList;
      const { width } = this;
      if (Math.abs(scrollLeft - this.curScrollLeft) == this.width) {
        return;
      }
      if (scrollLeft - this.curScrollLeft > 150) {
        this.scrollNext();
      }
      else if (this.curScrollLeft - scrollLeft > 150) {
        this.scrollPre();
      }
    },
    add() {
      this.$emit('add', LIST_INFO[this.curIndex]);
    },
  }
}
</script>

<style scoped>
.list {
  height: 50%;
  display: flex;
  align-items: center;
  justify-content: space-around;
}
.photo-view {
  position: relative;
  width: 100%;
  height: 200px;
}
.photo-view ::-webkit-scrollbar {
  display: none;
}
.photo-list {
  padding-top: 10px;
  display: flex;
  overflow: scroll;
}
.star {
  z-index: 99;
  position: absolute;
  background-image: url('../assets/star.png');
  background-size: contain;
  transition: all .5s ease-in;
}
.star_large {
  width: 25px;
  height: 27px;
}
.star_medium {
  width: 15px;
  height: 17px;
}
.star_small {
  width: 11px;
  height: 13px;
}

.chip,
.coffee,
.burger {
  position: relative;
  width: 100%;
  height: 200px;
  flex-shrink: 0;
}
.chip-sub_1,
.chip-sub_2,
.chip-main {
  position: absolute;
  width: 200px;
  height: 200px;
  transition: all 1s linear;
}
.chip-sub_1 {
  top: -75.5px;
  left: 32.3px;
}
.chip-sub_2 {
  top: -33.8px;
  left: 0;
}
.active .chip-sub_1 {
  top: -75.5px;
  left: 32.3px;
}
.active .chip-sub_2 {
  top: -33.8px;
  left: 0;
}
.chip-main {
  left: 20px;
}

.coffee-main {
  position: absolute;
  left: 20px;
  width: 230px;
  height: 240px;
}
.coffee-sub {
  position: absolute;
  width: 230px;
  height: 230px;
  left: 9px;
  top: -57px;
  z-index: 99;
}

.burger {
  top: 20px;
}
.burger-main {
  position: absolute;
  left: 30px;
  width: 170px;
  height: 171px;
}
.burger-sub {
  position: absolute;
  top: -30px;
  left: 50px;
  width: 130px;
  height: 129px;
  z-index: 99;
  transition: all .5s ease-in;
}
.active .burger-sub {
  top: -40px;
}

.info {
  position: absolute;
  right: 0;
  margin: 75px 35px 0 0;
  width: 150px;
  display: flex;
  flex-flow: column;
  align-items: end;
}
.info .active {
  top: 30px;
}
.info-name {
  margin-right: 16px;
  font-weight: 600;
  font-size: 32px;
  color: #EB5C77;
  text-align: right;
}
.info-price {
  margin: 6px 16px 0 0;
  font-weight: 300;
  font-size: 24px;
  line-height: 16px;
  color: #EB5C77;
  text-align: right;
}
.info-add {
  margin-top: 49px;
  width: 109px;
  height: 109px;
  background-image: url('../assets/add.png');
  background-size: contain;
}
</style>
