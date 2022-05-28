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
            <!-- <home-page /> -->
            <!-- <my-info /> -->
            <payment />
          </div>
        </el-main>
    </el-container>
</template>

<script>
// import HomePage from '~/components/HomePage.vue'
// import MyInfo from '~/components/MyInfo.vue'
import Payment from '~/components/Payment.vue'
export default {
  name: 'HomeUser',
  components: { Payment },
  layout: 'MainLayout',
  data() {
    return {
      indexComponent: 1,
      headerTitle: 'Thanh toán'
    }
  },
  methods: {
    onChangeIndex(index) {
      this.indexComponent = index
      console.log(this.indexComponent)
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
      border-bottom: 1px solid #ccc;
      height: 90px;
      border-bottom: 1px solid #ccc;
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
