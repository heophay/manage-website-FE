<template>
    <el-container :class="$style.home">
      <el-aside width="15vw">
        <Aside
          :list-aside="listAside"
          :index="indexComponent"
          @index-component="onChangeIndex"
        />
      </el-aside>
        <el-main>
          <div :class="$style.mainScreen">
            <el-row v-if="indexComponent !== '1'" :class="$style.rowTitle">
              <el-col :span="24">
                <div :class="$style.headerTitle">
                  <span>{{ headerTitle }}</span>
                </div>
              </el-col>
            </el-row>
            <home-page v-if="indexComponent === '1'" />
            <my-info v-if="indexComponent === '2'" />
            <payment v-if="indexComponent === '3'" />
            <list-prod v-if="indexComponent === '4-1'" @index-component="onChangeIndex" @product="getProd" />
            <add-prod v-if="indexComponent === '4-2'" :product="editProduct" @reset-edit-product="onResetProduct" />
            <!-- <list-bills v-if="indexComponent === '5-1'" /> -->
            <total-revenue v-if="indexComponent === '5-2'" />
            <list-employee v-if="indexComponent === '6-1'" @index-component="onChangeIndex" @edit="getEmployy" />
            <manage-employee v-if="indexComponent === '6-2'" :user="editUser" @reset-edit-user="onResetUser" />
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
      headerTitle: 'Trang chủ',
      listAside: [],
      editProduct: null,
      editUser: null,
      user: null,
    }
  },
  watch: {
    indexComponent(newVal) {
      const component = this.listAside.filter((e) => e.index === newVal)
      this.headerTitle = component[0].title
    }
  },
  mounted() {
    this.getInitData()
    this.user = JSON.parse(localStorage.getItem('user'))
  },
  methods: {
    onChangeIndex(index) {
      this.indexComponent = index
    },
    getProd(val) {
      this.editProduct = val
    },
    getEmployy(val) {
      this.editUser = val
    },
    getInitData() {
      this.listAside = [
        {
          index: '1',
          title: 'Trang chủ',
        },
        {
          index: '2',
          title: 'Thông tin cá nhân',
        },
        {
          index: '3',
          title: 'Thanh toán',
        },
        {
          index: '4-1',
          title: 'Danh sách sản phẩm',
        },
        {
          index: '4-2',
          title: 'Thêm & sửa sản phẩm',
        },
        {
          index: '5-1',
          title: 'Danh sách đơn hàng',
        },
        {
          index: '5-2',
          title: 'Thống kê',
        },
        {
          index: '6-1',
          title: 'Danh sách nhân viên',
        },
        {
          index: '6-2',
          title: 'Thêm sửa nhân viên',
        },
      ]
    },
    onResetProduct() {
      this.editProduct = null
    },
    onResetUser() {
      this.editUser = null
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
