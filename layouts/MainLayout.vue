<template>
  <el-container class="app">
    <el-header height="12vh"><Header /></el-header>
    <Nuxt />
  </el-container>
</template>

<script>
export default {}
</script>

<style lang="scss" module>
@import "@/assets/scss/variables";
:global {
  .el-header {
    background-color: #137dc4;
    padding: 0;
    border-bottom: 1px solid #ccc;
  }

}
</style>

