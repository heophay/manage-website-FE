<template>
  <div v-if="showToast" class="toastNotification" :class="type">
    <div>
      <i :class="iconClass"></i>
      <span class="textNotification">{{ text }}</span>
    </div>
    <i class="el-icon-close" @click="closeToast"></i>
  </div>
</template>

<script>
export default {
  name: 'HeaderPage',
  props: {
    type: {
      type: String,
      default: '',
    },
    text: {
      type: String,
      default: '',
    },
    showToast: {
      type: Boolean,
      default: false,
    },

  },
  data() {
    return {
      icon: {
        success: 'el-icon-success',
        error: 'el-icon-error',
      },
    }
  },
  computed: {
    iconClass() {
      return this.icon[this.type]
    },
    typeClass() {
      return `${this.type}`
    }
  },
  watch: {
    showToast() {
      const self = this
      const timeOut = setTimeout(function() {
        self.$emit('close')
        clearTimeout(timeOut);
      }, 3000)
    }
  },
  methods: {
    closeToast() {
      this.$emit('close')
    }
  },
}
</script>

<style lang="scss">
@import "@/assets/scss/variables";
.toastNotification {
  width: 230px;
  display: flex;
  justify-content: space-between;
  padding: 10px;
  border-radius: 6px;
  position: absolute;
  bottom: 40px;
  left: calc(50% - 115px);
  > i {
    cursor: pointer;
  }
  &.success {
    background-color: $color-primary;
    color: #fff;
  }
  &.error {
    background-color: red;
    color: #fff;
  }
}
</style>
