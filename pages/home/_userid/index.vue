<template>
    <el-container :class="$style.home">
      <el-aside width="15vw">
        <Aside
          @index-component="onChangeIndex"
        />
      </el-aside>
        <el-main>
          <div :class="$style.mainScreen">
            <home-page v-if="indexComponent === 1"/>
          </div>
        </el-main>
    </el-container>
</template>

<script>
import HomePage from '~/components/HomePage.vue'
export default {
  name: 'HomeUser',
  data() {
    return {
      indexComponent: 1,
    }
  },
  components: { HomePage },
  layout: 'MainLayout',
  methods: {
    onChangeIndex(index) {
      this.indexComponent = index
    }
  },
}
</script>

<style lang="scss" module>
@import '@/assets/scss/variables';
.home {
  padding: 5px;
  background-color: $color-white;
  .mainScreen {
    // background-color: #ccc;
    height: 100%;
  }
}
:global {
  .el-main {
    padding: 0;
  }
}
</style>
