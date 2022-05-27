<template>
  <div :class="$style.aside">
    <el-menu
      class="el-menu-vertical-demo"
      background-color="#137dc4"
      text-color="#aba8a8"
      active-text-color="#fff">
      <el-menu-item index="1">
        <i class="el-icon-menu"></i>
        <span>Trang chủ</span>
      </el-menu-item>
      <el-menu-item index="2">
        <i class="el-icon-document"></i>
        <span>Thông tin cá nhân</span>
      </el-menu-item>
      <el-menu-item index="3">
        <i class="el-icon-setting"></i>
        <span>Thanh Toán</span>
      </el-menu-item>


      <el-submenu index="4">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>Sản phẩm</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="4-1">Danh sách</el-menu-item>
          <el-menu-item index="4-2">Thêm sản phẩm</el-menu-item>
          <el-menu-item index="4-3">Thông tin sản phẩm</el-menu-item>
        </el-menu-item-group>
      </el-submenu>

      <el-submenu index="5">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>Doanh thu</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="5-1">Danh sách đơn hàng</el-menu-item>
          <el-menu-item index="5-2">Thống kê</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>
  </div>
</template>

<script>
export default {
  name: 'AsidePage'
}
</script>

<style lang="scss" module>
.aside {
  background-color: var(--color-primary);
  color: #fff;
  height: 100vh;
  margin-right: 15px;

}
</style>
