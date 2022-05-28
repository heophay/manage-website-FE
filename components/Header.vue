<template>
  <div :class="$style.header">
    <span :class="$style.icon">
      <i class="el-icon-user" />
    </span>
  </div>
</template>

<script>
export default {
  name: 'HeaderPage'
}
</script>

<style lang="scss" module>
@import "@/assets/scss/variables";
.header {
  color: #fff;
  height: 100%;

  .icon {
    i {
      font-size: 60px;
      margin-top: 15px;
      margin-left: 20px;
    }
  }
}
</style>
