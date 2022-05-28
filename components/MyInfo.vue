<template>
  <div :class="$style.infoScreen">
    <el-row type="flex" :class="$style.rowInfo" justify="center">
      <el-col :span="10">
        <div :class="$style.colInfo">
          <span :class="$style.colInfoLabel">Username</span>
          <el-input
            v-model="infoUser.username"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="$style.colInforInput"
            :disabled="true"
          />
        </div>
      </el-col>
      <el-col :span="10">
        <div :class="$style.colInfo">
          <span :class="$style.colInfoLabel">Fullname</span>
          <el-input
            v-model="infoUser.fullName"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="$style.colInforInput"
          />
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" :class="$style.rowInfo" justify="center">
      <el-col :span="10">
        <div :class="$style.colInfo">
          <span :class="$style.colInfoLabel">Email</span>
          <el-input
            v-model="infoUser.email"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="$style.colInforInput"
          />
        </div>
      </el-col>
      <el-col :span="10">
        <div :class="$style.colInfo">
          <span :class="$style.colInfoLabel">Phone</span>
          <el-input
            v-model="infoUser.phone"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="$style.colInforInput"
          />
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" :class="$style.rowInfo" justify="center">
      <el-col :span="10">
        <div :class="$style.colInfo">
          <span :class="$style.colInfoLabel">Password</span>
          <el-input
            v-model="infoUser.password"
            placeholder="Pick a date"
            :class="$style.colInforInput"
            show-password
          />
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" :class="$style.rowInfo" justify="center">
      <el-col :span="10">
        <div :class="$style.colInfo">
          <span :class="$style.colInfoLabel">Confirm Password</span>
          <el-input
            v-model="infoUser.password"
            placeholder="Pick a date"
            :class="$style.colInforInput"
            show-password
          />
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" :class="$style.rowInfo" justify="center">
      <el-button type="primary">Lưu thay đổi</el-button>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'HomeWebsite',
  data() {
    return {
      text: 'Đây là trang chủ!!',
      infoUser: {
        username: 'heophay',
        fullName: 'Nguyễn Bá Huy',
        phone: '123123132',
        email: 'heophay@pd.ai',
        password: '123456',
      },
    }
  },
}
</script>

<style lang="scss" module>
.infoScreen {
  padding: 80px 20px;

  .rowInfo {
    margin-bottom: 40px;

    .colInfo {
      margin-bottom: 10px;
      display: flex;
      padding: 0px 30px;
      margin-bottom: 10px;
      align-items: center;
      justify-content: flex-end;

      .colInfoLabel {
        margin-right: 24px;
      }
      .colInforInput {
        width: 56%;
      }
  }
  }


  :global {
    .el-row {
      border-bottom: 0px solid #fff;
    }
  }
}
</style>
