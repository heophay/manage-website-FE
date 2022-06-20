<template>
  <div :class="$style.manageEmployee">
    <div :class="$style.content">
      <div :class="$style.contentUser">
        <div :class="$style.infoSection">
          <div :class="$style.infoItem">
            <span :class="$style.infoText">Full Name</span>
            <el-input
              v-model="user.fullname"
              placeholder="Type name"
              suffix-icon="el-icon-price-tag"
              :class="$style.infoInput"
            />
          </div>
          <div :class="$style.infoItem">
            <span :class="$style.infoText">Phone</span>
            <el-input
              v-model="user.phone"
              placeholder="Type phone"
              suffix-icon="el-icon-price-tag"
              :class="$style.infoInput"
            />
          </div>
          <div :class="$style.infoItem">
            <span :class="$style.infoText">Email</span>
            <el-input
              v-model="user.email"
              placeholder="Type email"
              suffix-icon="el-icon-price-tag"
              :class="$style.infoInput"
            />
          </div>
          <el-checkbox v-model="user.isAdmin" :class="$style.infoCheck"
            >is Admin</el-checkbox
          >
        </div>
        <div :class="$style.userSection">
          <div :class="$style.userItem">
            <span :class="$style.userText">Username</span>
            <el-input
              v-model="user.username"
              placeholder="Type Username"
              suffix-icon="el-icon-price-tag"
              :class="$style.userInput"
            />
          </div>
          <div :class="$style.userItem">
            <span :class="$style.userText">Password</span>
            <el-input
              v-model="user.password"
              placeholder="Type Username"
              suffix-icon="el-icon-price-tag"
              :class="$style.userInput"
            />
          </div>
          <div :class="$style.userItem">
            <span :class="$style.userText">Confirm password</span>
            <el-input
              v-model="confirmPassword"
              placeholder="Type Username"
              suffix-icon="el-icon-price-tag"
              :class="$style.userInput"
            />
          </div>
        </div>
      </div>
      <el-button type="primary" :class="$style.btnAdd" @click="onClickAdd">
        Adding Employee
      </el-button>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ManageEmployee',
  data() {
    return {
      user: {
        isAdmin: false
      },
      confirmPassword: '',
    }
  },
  // mounted() {
  //   this.getUser()
  // },
  methods: {
    getUser() {
      this.user = JSON.parse(localStorage.getItem('user'))
    },
    onClickAdd() {
      axios.post('http://localhost:3001/api/user/create', this.user)
      .then((res) => {
        console.log(JSON.stringify(this.user))
      })
      console.log(JSON.stringify(this.user))
    }
  },
}
</script>

<style lang="scss" module>
@import '@/assets/scss/variables';
.manageEmployee {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: calc(100% - 75px);

  .content {
    margin: auto;
    width: 40%;
    display: flex;
    flex-direction: column;
    align-items: center;
    .contentUser {
      border: 1px solid $color-primary;
      border-radius: 10px;
      padding: 20px 30px;
      width: 100%;
      .infoSection {
        display: flex;
        flex-direction: column;
        border-bottom: 1px solid $color-primary;
        .infoItem {
          width: 100%;
          display: flex;
          align-items: center;
          margin-bottom: 20px;
          .infoText {
            width: 25%;
          }
          .infoInput {
            width: auto;
            flex: 1;
          }
        }
        .infoCheck {
          display: flex;
          align-items: center;
          justify-content: flex-end;
          margin-bottom: 20px;
        }
      }
      .userSection {
        margin-top: 20px;
        display: flex;
        flex-direction: column;
        align-items: center;
        .userItem {
          display: flex;
          margin-bottom: 20px;
          align-items: center;
          width: 80%;
          .userText {
            width: 40%;
          }
          .userInput {
            width: auto;
            flex: 1;
          }
        }
      }
    }
    .btnAdd {
      margin-top: 20px;
      width: 200px;
    }
  }
}
</style>
