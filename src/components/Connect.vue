<template>
  <div class="connect">
    <div class="location"></div>
    <div class="address">
      Dongcheng District Metro Cultural Building
    </div>
    <a class="phone" href="tel:8888888"></a>
  </div>
</template>

<script>
export default {
  name: 'Connect',
  components: {
  },
  data () {
    return {
    }
  }
}
</script>

<style scoped>
.connect {
  position: fixed;
  bottom: 65px;
  height: 62px;
  width: 100%;
  display: flex;
}
.location {
  margin-left: 22px;
  width: 25px;
  height: 33.8px;
  background-image: url('../assets/location.png');
  background-size: cover;
}
.address {
  margin: 0 18px 0 17px;
  flex: 1;
  font-weight: 600;
  font-size: 14px;
  line-height: 16px;
}
.phone {
  margin-right: 32px;
  width: 33px;
  height: 33px;
  background-image: url('../assets/phone.png');
  background-size: cover;
}
</style>
