<template>
  <div :class="$style.loginPage">
    <div :class="$style.content">
      <el-container>
        <el-main>
          <div :class="$style.contentSignIn">
            <span :class="$style.titleHeader">Welcome to My Website</span>
            <img
              src="https://media.istockphoto.com/photos/black-woman-hand-holding-modern-smart-phone-isolated-on-white-picture-id1333518127"
              alt="Phone Images"
              width="130px"
              height="130px"
            />
            <span :class="$style.textSignIn">Sign in</span>
            <el-input
              v-model="username"
              placeholder="Username"
              prefix-icon="el-icon-user-solid"
            >
            </el-input>
            <el-input
              v-model="password"
              placeholder="Please input password"
              show-password
              prefix-icon="el-icon-lock"
            >
            </el-input>
            <span v-if="!statusLogin" :class="$style.textStatusLogin">
              The username or password is incorrect. Please try again!
            </span>
            <el-row>
              <!-- <el-button :class="$style.btnSignUp" round>Sign up</el-button> -->
              <el-button :class="$style.btnSignIn" round @click="login">Sign in</el-button>
            </el-row>
          </div>
        </el-main>
        <el-aside width="35%">
          <div :class="$style.contentAside">
            <span :class="$style.textHello">Hello, Friends!</span>
            <span :class="$style.textDescription"
              >Enter your personal details and start jouney with us</span
            >
          </div>
        </el-aside>
      </el-container>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'LoginPage',
  data() {
    return {
      username: '',
      password: '',
      statusLogin: true,
    }
  },
  mounted() {
    if (this.checkLocalStorage()) this.$router.push('/home/' + this.checkLocalStorage())
  },
  methods: {
    login(){
      this.statusLogin = true
      const data = {
        username: this.username,
        password: this.password,
      }
      axios.post('http://localhost:3001/api/user/login', data)
        .then((res) => {
          const user = res.data.user
          if (user) {
            localStorage.setItem('user', JSON.stringify(user))
            this.$router.push('/home/' + user._id)
          } else {
            this.statusLogin = false
          }
        })
    },
    checkLocalStorage() {
      const user = localStorage.getItem('user')
      if (user) return user._id
      return null
    },
  },
}
</script>

<style lang="scss" module>
.loginPage {
  display: flex;
  height: 100vh;
  width: 100vw;
  .content {
    box-shadow: 1px -2px 20px #ccc;
    color: var(--color-primary);
    text-align: center;
    margin: auto;
    height: 65%;
    width: 60%;
    .contentSignIn {
      display: flex;
      flex-direction: column;
      align-items: center;
      height: 100%;
      .titleHeader {
        font-weight: 600;
        font-size: 25px;
        margin-top: 10px;
      }
      .textSignIn {
        font-weight: 600;
        font-size: 24px;
        margin-top: 32px;
        margin-bottom: 14px;
      }
      .btnSignUp {
        color: var(--color-primary);
        border-color: var(--color-primary);
      }
      .btnSignIn {
        background-color: var(--color-primary);
        color: #fff;
        margin-top: 20px;
      }
      .textStatusLogin {
        font-size: 14px;
        font-weight: 400;
        line-height: 20px;
        margin-top: 20px;
        color: red;
      }
    }
    .contentAside {
      display: flex;
      flex-direction: column;
      padding: 20px;
      .textHello {
        margin-top: 40%;
        font-size: 30px;
        font-weight: 700;
      }
      .textDescription {
        margin-top: 8px;
        font-size: 17px;
        line-height: 22px;
      }
    }
    :global {
      .el-input {
        margin-top: 10px;
        :focus {
          border-color: var(--color-primary);
          .el-input__icon::before {
            color: var(--color-primary);
          }
        }
      }
      .el-container {
        height: 100%;
        background-color: #fff;
      }
      .el-aside {
        background-color: var(--color-primary);
        color: #fff;
      }
    }
  }
}
</style>
