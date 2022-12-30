<template>
  <div class="animate-info">
      <div class="info">
        <div class="name">
          <transition name="up">
            <span v-if="trigger" class="animation-text">{{ name }}</span>
          </transition>
        </div>
        <div class="price">
          <transition name="up">
            <span v-if="trigger" class="animation-text">{{ price }}$</span>
          </transition>
        </div>
      </div>
      <transition name="fade">
        <div v-if="trigger" class="add" @click="add"></div>
      </transition>
  </div>
</template>

<script>

export default {
  name: 'AnimateInfo',
  props: {
    name: '',
    price: '',
    trigger: false
  },
  data () {
    return {
    }
  },
  methods: {
    add () {
      this.$emit('add')
    }
  }
}
</script>

<style scoped>
.animate-info {
  position: absolute;
}
.info {
  width: 150px;
  color: #EB5C77;
  text-align: right;
}
.name {
  position: relative;
  height: 34px;
  font-weight: 600;
  font-size: 32px;
  overflow: hidden;
}

.price {
  margin-top: 6px;
  position: relative;
  height: 26px;
  font-weight: 300;
  font-size: 24px;
  overflow: hidden;
}
.add {
  position: absolute;
  right: -20px;
  top: 110px;
  width: 109px;
  height: 109px;
  background-image: url('../assets/add.png');
  background-size: contain;
}
.animation-text {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
}
.up-enter-active {
  animation: bounce-in .5s;
}
.up-leave-active {
  animation: fade-in .3s reverse;
}
.fade-enter-active {
  animation: fade-in 1s;
}
.fade-leave-active {
  animation: fade-in .3s reverse;
}
@keyframes bounce-in {
  0% {
    opacity: 0;
    top: 50px;
  }
  50% {
    opacity: .5;
    top: 25px;
  }
  100% {
    opacity: 1;
    top: 0px;
  }
}
@keyframes fade-in {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
</style>
