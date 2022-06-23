<template>
  <div :class="$style.aside">
    <el-menu
      class="el-menu-vertical-demo"
      :default-active="index"
      background-color="#137dc4"
      text-color="#aba8a8"
      active-text-color="#fff"
      @select="onChangeIndex">
      <el-menu-item index="1">
        <i class="el-icon-s-home"></i>
        <span>Trang chủ</span>
      </el-menu-item>
      <el-menu-item index="2">
        <i class="el-icon-document"></i>
        <span>Thông tin cá nhân</span>
      </el-menu-item>
      <el-menu-item index="3">
        <i class="el-icon-bank-card"></i>
        <span>Thanh Toán</span>
      </el-menu-item>
      <el-submenu index="4">
        <template slot="title">
          <i class="el-icon-s-goods"></i>
          <span>Sản phẩm</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="4-1">Danh sách</el-menu-item>
          <el-menu-item index="4-2">Thêm & sửa sản phẩm</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
      <el-submenu index="5">
        <template slot="title">
          <i class="el-icon-money"></i>
          <span>Doanh thu</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="5-1">Danh sách đơn hàng</el-menu-item>
          <el-menu-item index="5-2">Thống kê</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
      <el-submenu v-if="isAdmin" index="6">
        <template slot="title">
          <i class="el-icon-user-solid"></i>
          <span>Nhân viên</span>
        </template>
        <el-menu-item-group>
          <el-menu-item index="6-1">Danh sách</el-menu-item>
          <el-menu-item index="6-2">Thêm & sửa nhân viên</el-menu-item>
        </el-menu-item-group>
      </el-submenu>
    </el-menu>
  </div>
</template>

<script>
export default {
  name: 'AsidePage',
  props: {
    listAside: {
      type: Array,
      default() {
        return []
      }
    },
    index: {
      type: String,
      default: '1',
    }
  },
  data() {
    return {
      isAdmin: false,
    }
  },
  mounted() {
    this.isAdmin = JSON.parse(localStorage.getItem('user')).isAdmin
  },
  methods: {
    onChangeIndex(index) {
      this.$emit('index-component', index)
    }
  },
}
</script>

<style lang="scss" module>
.aside {
  background-color: var(--color-primary);
  color: #fff;
  height: 92vh;

  :global{
    .el-menu-item.is-active {
      background-color: #075081 !important;
    }
    .el-menu-item {
      &:hover{
        color: #fff !important;
      }
    }
  }

}
</style>
