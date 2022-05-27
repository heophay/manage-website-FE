<template>
    <el-container :class="$style.home">
      <el-aside width="15vw">
        <Aside />
      </el-aside>
        <el-main>
          <div :class="$style.mainScreen">
            Đây là trang chủ!
          </div>
        </el-main>
    </el-container>
</template>

<script>
export default {
  name: 'HomeUser',
  layout: 'MainLayout',
}
</script>

<style lang="scss" module>
@import '@/assets/scss/variables';
.home {
  padding: 5px;
  background-color: $color-white;
  .mainScreen {
    // background-color: #ccc;
    height: 100%;
  }
}
:global {
  .el-main {
    padding: 0;
  }
}
</style>
