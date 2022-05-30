<template>
  <div :class="$style.header">
    <div :class="$style.avatar">
      <span :class="$style.icon">
        <i class="el-icon-user" />
      </span>
      <span> {{ user.name }}</span>
    </div>
    <div>
      <el-button :class="$style.btnLogout" type="text">Đăng xuất</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderPage',
  data() {
    return {
      user: {
        name: 'Nguyễn Bá Rôn',
      }
    }
  },
}
</script>

<style lang="scss" module>
@import "@/assets/scss/variables";
.header {
  color: #fff;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px;

  .btnLogout {
    color: #fff;
    font-size: 16px;
  }

  .avatar {
    display: flex;
    align-items: center;
  }

  .icon {
    i {
      font-size: 34px;
      margin-right: 15px;
    }
  }
}
</style>
