<template>
  <div :class="$style.screen">
    <div :class="$style.screenAdd">
      <el-row type="flex" :class="$style.row" justify="center" :gutter="20">
        <el-col :span="13">
          <div :class="$style.column">
            <span :class="$style.colLabel">Tên sản phẩm</span>
            <el-input
              v-model="prod.name"
              placeholder="Type product name"
              suffix-icon="el-icon-s-goods"
              :class="$style.colInput"
            />
          </div>
        </el-col>
        <el-col :span="9">
          <div :class="$style.column">
            <span :class="$style.colLabel">Giá bán</span>
            <el-input
              v-model="prod.price"
              placeholder="0"
              suffix-icon="el-icon-price-tag"
              :class="$style.colInput"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.row" justify="center">
        <el-col :span="8" :offset="13">
          <div :class="$style.column">
            <span :class="$style.colLabel">Số lượng</span>
            <el-input
              v-model="prod.quantity"
              placeholder="0"
              suffix-icon="el-icon-shopping-cart-2"
              :class="$style.colInputNum"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.row" justify="center">
        <el-col :span="15">
          <div :class="$style.column">
            <span :class="$style.colLabel">Mô tả</span>
            <el-input
              v-model="prod.description"
              type="textarea"
              :autosize="{ minRows: 2, maxRows: 4 }"
              placeholder="Please input the description"
              :class="$style.colInput"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.row" justify="center">
        <el-button type="primary" @click="onClickButton">
          {{ getTextButton }}
        </el-button>
      </el-row>
    </div>
    <!-- List -->
    <div v-if="!isEditProd" :class="$style.screenList">
      <el-table
        :data="
          prodsAdded.filter(
            (data) =>
              !search || data.name.toLowerCase().includes(search.toLowerCase())
          )
        "
        height="250"
        stripe
        style="width: 100%"
      >
        <el-table-column label="Tên sản phẩm" prop="name"> </el-table-column>
        <el-table-column label="Số lượng" prop="quantity"> </el-table-column>
        <el-table-column label="Đơn giá" prop="price"> </el-table-column>
        <el-table-column label="Miêu tả" prop="description"> </el-table-column>
        <el-table-column fixed="right" label="Operations">
          <template slot-scope="scope">
            <el-button
              type="primary"
              icon="el-icon-edit"
              size="medium"
              circle
              @click="onEdit(scope.$index, scope.row)"
            />
            <el-button
              type="danger"
              icon="el-icon-delete"
              size="medium"
              circle
              @click="onDelete(scope.$index)"
            />
          </template>
        </el-table-column>
      </el-table>
      <el-row type="flex" :class="$style.btnAdd" justify="center">
        <el-button type="primary" @click="onAddProds">Thêm tất cả sản phẩm</el-button>
      </el-row>
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
  name: 'AddProdComponent',
  props: {
    product: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      prod: {},
      prodsAdded: [],
      isEditProd: false,
      toast: {}
    }
  },
  computed: {
    getTextButton() {
      return this.isEditProd ? 'Edit Product' : 'Add Product'
    }
  },
  mounted() {
    this.getProduct()
  },
  destroyed() {
    this.$emit('reset-edit-product')
  },
  methods: {
    getProduct() {
      if (this.product) {
        this.prod = Object.assign({}, this.product)
        this.isEditProd = true
      } else {
        this.resetProd()
      }
    },
    onClickButton() {
      if (this.isEditProd) {
        this.onUpdateProd()
      } else {
        this.prodsAdded.push(this.prod)
        this.resetProd()
      }

    },
    onDelete(index) {
      this.prodsAdded.splice(index, 1)
    },
    onEdit(index, row) {
      this.isEditProd = true
      this.prod = row
      this.indexEdit = index
    },
    resetProd() {
      this.prod = {
        name: '',
        price: null,
        quantity: null,
        description: '',
      }
    },
    onUpdateProd() {
      axios.put('http://localhost:3001/api/product/' + this.prod._id, this.prod)
        .then((res) => {
          if (res) this.showToast('success', 'Update Success!!', true)
        })
    },
    onAddProds() {
      axios.post('http://localhost:3001/api/product/create', this.prodsAdded)
      this.prodsAdded = []
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
.screen {
  padding: 20px 60px;
  display: flex;
  flex-direction: column;
  align-items: center;
  height: calc(100% - 75px);
  position: relative;

  .screenAdd {
    border: 2px solid var(--color-primary);
    width: 64%;
    padding: 20px 0;
    border-radius: 30px;
    margin: auto;

    .row {
      .column {
        margin-bottom: 10px;
        display: flex;
        margin-bottom: 10px;
        align-items: center;
        justify-content: flex-end;

        .colLabel {
          margin-right: 10px;
        }
        .colInput {
          width: 70%;
        }
        .colInputNum {
          width: 40%;
        }
      }
    }
  }
  .screenList {
    width: 100%;
    margin-top: 20px;
    border-top: 1px solid #ccc;

    .btnAdd {
      margin-top: 16px;
    }
  }
}
</style>
