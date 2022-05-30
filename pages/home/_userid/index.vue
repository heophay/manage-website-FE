<template>
    <el-container :class="$style.home">
      <el-aside width="15vw">
        <Aside
          @index-component="onChangeIndex"
        />
      </el-aside>
        <el-main>
          <div :class="$style.mainScreen">
            <el-row :class="$style.rowTitle">
              <el-col :span="24">
                <div :class="$style.headerTitle">
                  <span>{{ headerTitle }}</span>
                </div>
              </el-col>
            </el-row>
            <home-page v-if="indexComponent === '1'" />
            <my-info v-if="indexComponent === '2'" />
            <payment v-if="indexComponent === '3'" />
            <list-prod v-if="indexComponent === '4-1'" />
            <add-prod v-if="indexComponent === '4-2'" />
            <list-bills v-if="indexComponent === '5-1'" />
            <total-revenue v-if="indexComponent === '5-2'" />
          </div>
        </el-main>
    </el-container>
</template>

<script>
export default {
  name: 'HomeUser',
  layout: 'MainLayout',
  data() {
    return {
      indexComponent: '1',
      headerTitle: 'Thống kê'
    }
  },
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
  background-color: $color-white;
  .mainScreen {
    height: 100%;

    .headerTitle{
      font-size: 25px;
      font-weight: 600;
      line-height: 22px;
    }

    .rowTitle {
      border-bottom: 1px solid black;
      height: 75px;
      padding: 32px 50px;
    }
  }
}
:global {
  .el-main {
    padding: 0;
  }
}
</style>
