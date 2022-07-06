<template>
  <div :class="$style.screen">
    <div :class="$style.infoScreen">
      <!-- Information Main -->
      <div :class="$style.infoMain">
        <el-row type="flex" :class="$style.rowInfo" justify="end">
          <el-col :span="12">
            <div :class="$style.colInfo">
              <span :class="$style.colInfoLabel">Username</span>
              <el-input
                v-model="user.username"
                suffix-icon="el-icon-user-solid"
                :disabled="true"
              />
            </div>
          </el-col>
          <el-col :span="12">
            <div :class="$style.colInfo">
              <span :class="$style.colInfoLabel">Fullname</span>
              <el-input
                v-model="user.fullname"
                placeholder="Type your Fullname"
                suffix-icon="el-icon-s-custom"
                @input="checkValidChangeInfo()"
              />
            </div>
          </el-col>
        </el-row>
        <el-row type="flex" :class="$style.rowInfo" justify="end">
          <el-col :span="12">
            <div :class="$style.colInfo">
              <span :class="$style.colInfoLabel">Email</span>
              <el-input
                v-model="user.email"
                placeholder="Type your email"
                suffix-icon="el-icon-office-building"
                @input="checkValidChangeInfo()"
              />
            </div>
          </el-col>
          <el-col :span="12">
            <div :class="$style.colInfo">
              <span :class="$style.colInfoLabel">Phone</span>
              <el-input
                v-model="user.phone"
                placeholder="Type your phone number"
                suffix-icon="el-icon-phone"
                @input="checkValidChangeInfo()"
              />
            </div>
          </el-col>
        </el-row>
        <span>
          <el-button type="primary" :disabled="!checkValidButton" @click="onChangeInfo">Thay đổi thông tin</el-button>
        </span>
      </div>
      <!-- Password -->
      <div :class="$style.infoPassword">
        <el-row type="flex" :class="$style.rowPass" justify="center">
          <el-col :span="20">
            <div :class="$style.colPass">
              <span :class="$style.colPassLabel">Password</span>
              <el-input
                v-model="user.password"
                placeholder="Pick a date"
                show-password
              />
            </div>
          </el-col>
        </el-row>
        <el-row type="flex" :class="$style.rowPass" justify="center">
          <el-col :span="20">
            <div :class="$style.colPass">
              <span :class="$style.colPassLabel">Confirm Password</span>
              <div :class="$style.inputConfirm">
                <el-input
                  v-model="passwordConfirm"
                  placeholder="Pick a date"
                  show-password
                />
                <span v-if="!isValid" :class="$style.statusConfirmPW">Password confirm is incorrect!!</span>
              </div>
            </div>
          </el-col>
        </el-row>
        <el-row type="flex" :class="$style.rowInfo" justify="center">
          <el-button type="primary" :disabled="!isValid" @click="onChangeInfo">
            Thay đổi password
          </el-button>
        </el-row>
      </div>
    </div>
    <toast-notification
      :type="toast.type"
      :text="toast.text"
      :show-toast="toast.isShow"
      @close="closeToast"
    >
    </toast-notification>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'HomeWebsite',
  data() {
    return {
      user: {},
      toast: {
        isShow: false,
        text: '',
        type: '',
      },
      passwordConfirm: '',
      checkValidButton: false,
    }
  },
  computed: {
    isValid() {
      return this.passwordConfirm === this.user.password && !!this.user.password
    }
  },
  mounted() {
    this.user = JSON.parse(localStorage.getItem('user'))
    this.checkValidChangeInfo()
  },
  methods: {
    onChangeInfo() {
      const info = {
        fullname: this.user.fullname,
        phone: this.user.phone,
        email: this.user.email,
      }
      axios.put('http://localhost:3001/api/user/' + this.user._id, info)
        .then((res) => {
          this.changeToast('success','Change success!!', true)
        })
      this.getUser()
    },
    changeToast(type, text, isShow) {
      this.toast.type = type
      this.toast.text = text
      this.toast.isShow = isShow
    },
    closeToast() {
      this.toast.isShow = false
    },
    getUser(){
      axios.get('http://localhost:3001/api/user/' + this.user._id)
        .then((res) => {
          const user = res.data.user
          if (user) {
            localStorage.setItem('user', JSON.stringify(user))
          } else {
            console.log('faild')
          }
        })
    },
    checkValidChangeInfo() {
      this.checkValidButton = !!this.user.phone && !!this.user.fullname && !!this.user.email
    }
  },
}
</script>

<style lang="scss" module>
.screen {
  display: flex;
  align-items: center;
  height: calc(100% - 75px);
  width: 100%;
  position: relative;
  .infoScreen {
    padding: 30px 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    margin: auto;
    width: 100%;

    .infoMain {
      border: 2px solid var(--color-primary);
      width: 60%;
      border-radius: 24px;
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 24px 0;

      .rowInfo {
        margin-bottom: 15px;

        .colInfo {
          margin-bottom: 10px;
          display: flex;
          padding: 0px 30px;
          margin-bottom: 10px;
          align-items: center;
          justify-content: flex-end;

          .colInfoLabel {
            margin-right: 10px;
            width: 100px;
          }
        }
      }
    }
    .infoPassword {
      border: 2px solid var(--color-primary);
      margin-top: 20px;
      padding: 20px 0;
      width: 40%;
      border-radius: 20px;
      .rowPass {
        width: 100%;
        .colPass {
          display: flex;
          align-items: center;
          margin-bottom: 16px;
          .colPassLabel {
            width: 300px;
          }
          .inputConfirm {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
            .statusConfirmPW {
              color: red;
              margin-top: 8px;
            }
          }
        }
      }
    }
  }
}
</style>
