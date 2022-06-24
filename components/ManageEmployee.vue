<template>
  <div :class="$style.manageEmployee">
    <div :class="$style.content">
      <div :class="$style.contentUser">
        <div :class="$style.infoSection">
          <div :class="$style.infoItem">
            <span :class="$style.infoText">Full Name</span>
            <el-input
              v-model="manageUser.fullname"
              placeholder="Type name"
              suffix-icon="el-icon-price-tag"
              :class="$style.infoInput"
            />
          </div>
          <div :class="$style.infoItem">
            <span :class="$style.infoText">Phone</span>
            <el-input
              v-model="manageUser.phone"
              placeholder="Type phone"
              suffix-icon="el-icon-price-tag"
              :class="$style.infoInput"
            />
          </div>
          <div :class="$style.infoItem">
            <span :class="$style.infoText">Email</span>
            <el-input
              v-model="manageUser.email"
              placeholder="Type email"
              suffix-icon="el-icon-price-tag"
              :class="$style.infoInput"
            />
          </div>
          <el-checkbox v-model="manageUser.isAdmin" :class="$style.infoCheck">
            is Admin
          </el-checkbox>
        </div>
        <div :class="$style.userSection">
          <div :class="$style.userItem">
            <span :class="$style.userText">Username</span>
            <el-input
              v-model="manageUser.username"
              placeholder="Type Username"
              suffix-icon="el-icon-price-tag"
              disabled
              :class="$style.userInput"
            />
          </div>
          <div :class="$style.userItem">
            <span :class="$style.userText">Password</span>
            <el-input
              v-model="manageUser.password"
              placeholder="Type Password"
              show-password
              :class="$style.userInput"
            />
          </div>
          <div :class="$style.userItem">
            <span :class="$style.userText">Confirm password</span>
            <el-input
              v-model="confirmPassword"
              placeholder="Type Password"
              show-password
              :class="$style.userInput"
            />
          </div>
        </div>
      </div>
      <el-button type="primary" :class="$style.btnAdd" @click="onClickAdd">
        {{ getTextButton }}
      </el-button>
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
  name: 'ManageEmployee',
  props: {
    user: {
      type: Object,
      default() {
        return {}
      },
    }
  },
  data() {
    return {
      manageUser: {},
      confirmPassword: '',
      isEditUser: false,
      toast: {}
    }
  },
  computed: {
    getTextButton() {
      return this.isEditUser ? 'Edit User' : 'Add user'
    }
  },
  mounted() {
    this.getEmployee()
  },
  destroyed() {
    this.$emit('reset-edit-user')
  },
  methods: {
    getEmployee() {
      if (this.user) {
        this.manageUser = Object.assign({}, this.user)
        this.manageUser.isAdmin = this.manageUser.isAdmin === 'true'
        this.isEditUser = true
      } else {
        this.resetUser()
      }
    },
    resetUser() {
      this.manageUser = {
        fullname: '',
        phone: '',
        email: '',
        user: '',
        password: '',
        isAdmin: false,
      }
    },
    onClickAdd() {
      if (!this.isEditUser) {
        axios.post('http://localhost:3001/api/user/create', this.manageUser)
          .then((res) => {
            if (res) {
              this.resetUser()
              this.showToast('success', 'Add user Success!!', true)
            } else {
              this.showToast('error', 'Error!!', true)
            }
        })
      } else {
        axios.put('http://localhost:3001/api/user/' + this.manageUser._id, this.manageUser)
          .then((res) => {
            if (res) {
              this.showToast('success', 'Edit user Success!!', true)
            } else {
              this.showToast('error', 'Error!!', true)
            }
        })
      }
    },
    showToast(type, text, isShow) {
      this.toast = {
        type,
        text,
        isShow
      }
    },
    closeToast() {
      this.toast.isShow = false
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
  position: relative;

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
