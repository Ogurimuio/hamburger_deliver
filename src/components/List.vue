<template>
  <div class="list">
    <div class="photo-view" ref="photoView">
      <div class="star star_large" :style="getStarPostion('large')"></div>
      <div class="star star_medium" :style="getStarPostion('medium')"></div>
      <div class="star star_small" :style="getStarPostion('small')"></div>

      <div class="photo-list" ref="photoList" @scroll="scrollHandler">
        <div class="chip" :class="{ active: curIndex == 0 }">
          <img class="chip-sub_1" src="../assets/chip_1.png" />
          <img class="chip-sub_2" src="../assets/chip_2.png" />
          <img class="chip-main" src="../assets/chips.png" />
          <div class="info">
            <AnimateInfo
              :trigger="curIndex == 0 && !scrolling"
              :name="listInfo[curIndex].name"
              :price="listInfo[curIndex].price"
              @add="add"
            />
            <div class="info-add" @click="add"></div>
          </div>
        </div>

        <div class="coffee" :class="{ active: curIndex === 1 }">
          <img class="coffee-sub" src="../assets/coffee_sub.png" />
          <img class="coffee-main" src="../assets/coffee.png" />

          <div class="info">
            <AnimateInfo
              :trigger="curIndex == 1 && !scrolling"
              :name="listInfo[curIndex].name"
              :price="listInfo[curIndex].price"
              @add="add"
            />
            <div class="info-add" @click="add"></div>
          </div>
        </div>

        <div class="burger" :class="{ active: curIndex === 2 }">
          <img class="burger-sub" src="../assets/burger_top.png" />
          <img class="burger-main" src="../assets/burger_bottom.png" />

          <div class="info">
            <AnimateInfo
              :trigger="curIndex == 2 && !scrolling"
              :name="listInfo[curIndex].name"
              :price="listInfo[curIndex].price"
              @add="add"
            />
            <div class="info-add" @click="add"></div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import AnimateInfo from './AnimateInfo'

const STAR_LARGE_POSITION = {
  0: 'top: 0px; left: 160px',
  1: 'top: 160px; left: 170px',
  2: 'top: 130px; left: 20px'
}
const STAR_MEDIUM_POSITION = {
  0: 'top: 80px; left: 200px',
  1: 'top: 180px; left: 40px',
  2: 'top: 10px; left: 40px'
}
const STAR_SMALL_POSITION = {
  0: 'top: 110px; left: 20px',
  1: 'top: 50px; left: 210px',
  2: 'top: 180px; left: 180px'
}
const LIST_INFO = [{
  value: 'chips',
  name: 'FRIES',
  price: 4,
  dishImg: require('../assets/dish_chips.png'),
  dishSize: 94
}, {
  value: 'coffee',
  name: 'LATTE',
  price: 3,
  dishImg: require('../assets/dish_coffee.png'),
  dishSize: 128
}, {
  value: 'burger',
  name: 'BURGER',
  price: 6,
  dishImg: require('../assets/dish_burger.png'),
  dishSize: 101
}]
let timer = null

export default {
  name: 'List',
  components: {
    AnimateInfo
  },
  data () {
    return {
      curIndex: 0,
      starIndex: 0,
      curScrollLeft: 0,
      scrollLimit: false,
      scrolling: false,
      listInfo: LIST_INFO,
      width: 0,
      infoActive: false
    }
  },
  mounted () {
    // 更新客户端宽度
    this.width = this.$refs.photoView.clientWidth
  },
  methods: {
    getStarPostion (type) {
      const { starIndex } = this
      if (type === 'large') {
        return STAR_LARGE_POSITION[starIndex % 3]
      }
      if (type === 'medium') {
        return STAR_MEDIUM_POSITION[starIndex % 3]
      }
      if (type === 'small') {
        return STAR_SMALL_POSITION[starIndex % 3]
      }
    },
    activeInfo () {
      this.infoActive = true
      setTimeout(() => {
        this.infoActive = false
      }, 10)
    },
    limitScroll () {
      // 自动滚动到下一个产品后，防止用户滚动导致偏移
      this.scrollLimit = true
      setTimeout(() => {
        this.scrollLimit = false
      }, 1000)
    },
    scrollNext () {
      this.curIndex = this.curIndex + 1
      const newScrollLeft = this.width * this.curIndex
      this.$refs.photoList.scrollTo({ left: newScrollLeft })
      this.curScrollLeft = newScrollLeft
      this.scrolling = false
      this.activeInfo()
      this.limitScroll()
    },
    scrollPre () {
      this.curIndex = this.curIndex - 1
      const newScrollLeft = this.width * this.curIndex
      this.$refs.photoList.scrollTo({ left: newScrollLeft })
      this.curScrollLeft = newScrollLeft
      this.scrolling = false
      this.activeInfo()
      this.limitScroll()
    },
    // 滚动时启动动画
    scrollActive () {
      if (!timer) {
        const { scrollLeft } = this.$refs.photoList
        this.starIndex = scrollLeft > this.curScrollLeft ? this.starIndex + 1 : this.starIndex - 1
        this.scrolling = true
        timer = setTimeout(() => {
          timer = null
        }, 1500)
        this.preIndex = this.curIndex
      }
    },
    scrollHandler () {
      if (this.scrollLimit) {
        this.$refs.photoList.scrollTo({ left: this.curScrollLeft })
      }
      const { scrollLeft } = this.$refs.photoList
      const { width } = this
      const widthLimit = 150
      this.scrollActive()
      if (Math.abs(scrollLeft - this.curScrollLeft) === width) {
        return
      }
      if (scrollLeft - this.curScrollLeft > widthLimit) {
        this.scrollNext()
      } else if (this.curScrollLeft - scrollLeft > widthLimit) {
        this.scrollPre()
      }
    },
    add () {
      this.$emit('add', LIST_INFO[this.curIndex])
    }
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
  transition: all 1s ease-in;
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
  height: 250px;
  flex-shrink: 0;
}
.chip-sub_1,
.chip-sub_2,
.chip-main {
  position: absolute;
  width: 200px;
  height: 200px;
  left: 10px;
  transition: all .5s linear;
}
.chip-sub_1 {
  top: 0;
  left: 33px;
}
.chip-sub_2 {
  top: 0px;
  left: 30px;
}
.active .chip-sub_1 {
  top: -77px;
}
.active .chip-sub_2 {
  top: -50px;
  left: 0;
}

.coffee-main {
  position: absolute;
  top: -10px;
  width: 260px;
  height: 280px;
  transform: rotate(15deg);
  transition: all .5s ease;
}
.coffee-sub {
  position: absolute;
  width: 250px;
  height: 250px;
  left: -10px;
  top: -65px;
  z-index: 99;
  opacity: 0;
  transform: scale(0);
  transition: all .5s ease;
}
.active .coffee-main {
  transform: rotate(0);
}
.active .coffee-sub {
  opacity: 1;
  transform: scale(1);
}

.burger-main {
  position: absolute;
  top: 20px;
  left: 30px;
  width: 170px;
  height: 171px;
}
.burger-sub {
  position: absolute;
  top: 0;
  left: 50px;
  width: 130px;
  height: 129px;
  z-index: 99;
  transition: all .3s ease-in;
}
.active .burger-sub {
  top: -20px;
}

.info {
  position: absolute;
  right: 0;
  margin: 10px 35px 0 0;
  display: flex;
  flex-flow: column;
  align-items: end;
}
</style>
