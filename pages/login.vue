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
            <span :class="$style.textForgotPw">Forgot your password?</span>
            <el-row>
              <!-- <el-button :class="$style.btnSignUp" round>Sign up</el-button> -->
              <el-button :class="$style.btnSignIn" round>Sign in</el-button>
            </el-row>
          </div>
        </el-main>
        <el-aside width="35%">
          <div :class="$style.contentAside">
            <span :class="$style.textHello">Hello, Tú Nhi!</span>
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
export default {
  name: 'LoginPage',
  data() {
    return {
      username: '',
      password: '',
    }
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
      }
      .textForgotPw {
        font-size: 14px;
        font-weight: 400;
        line-height: 20px;
        margin-top: 20px;
        margin-bottom: 20px;
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
