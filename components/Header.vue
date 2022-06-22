<template>
  <div :class="$style.header">
    <div :class="$style.avatar">
      <span :class="$style.icon">
        <i class="el-icon-user" />
      </span>
      <span>{{ user.fullname }}</span>
    </div>
    <div>
      <el-button :class="$style.btnLogout" type="text" @click="logOut">Đăng xuất</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderPage',
  data() {
    return {
      user: {}
    }
  },
  mounted() {
    this.user = JSON.parse(localStorage.getItem('user'))
  },
  methods: {
    logOut() {
      console.log(this.$route)
      this.$router.back()
    },
    getFullname() {
      if (this.user) return  this.user.fullname
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
