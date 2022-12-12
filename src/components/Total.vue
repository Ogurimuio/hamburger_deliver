<template>
  <div class="total">
    <div class="price">{{ price }}<span class="price-unit">$</span></div>
    <div class="pay" @click="pay">Pay</div>
  </div>
</template>

<script>
export default {
  name: 'Total',
  components: {
  },
  props: {
    price: 0,
  },
  data () {
    return {
    }
  },
  methods: {
    pay() {
      this.$emit('pay');
    }
  }
}
</script>

<style scoped>
.total {
  position: fixed;
  bottom: 0;
  height: 65px;
  width: 100%;
  display: flex;
}
.price {
  padding-left: 20px;
  flex: 1;
  background: #FFF;
  font-style: normal;
  font-weight: 600;
  font-size: 43.8px;
  line-height: 65px;
  color: #4F4F4F;
}
.price-unit {
  font-size: 28px;
}
.pay {
  width: 143px;
  background: linear-gradient(324.97deg, #FD003C -11%, #FF5D79 105.7%);
  text-align: center;
  font-weight: 600;
  font-size: 32px;
  line-height: 65px;
  color: #FFF;
}
</style>
