<template>
  <div :class="$style.HomePage">
    <img src="" alt="">
  </div>
</template>

<script>
export default {
  name: 'HomeWebsite',
  data() {
    return {
      text: 'Đây là trang chủ!!'
    }
  },
}
</script>
<style lang="scss" module>
.HomePage{
  background-image: url("https://c8.alamy.com/zooms/9/d36fa0970d3d44069e7c9406fd822414/2hp9rgt.jpg");
  background-repeat: no-repeat;
  background-size: cover;
  height: 100%;
}
</style>
