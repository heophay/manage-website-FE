<template>
  <div :class="$style.header">
    <el-row>
      <el-col :span="24">
        <span>Here is Header</span>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'HeaderPage'
}
</script>

<style lang="scss" module>
@import "@/assets/scss/variables";
.header {
  color: #fff;
}
</style>
