<template>
  <div :class="$style.screen">
    <el-table
      :data="products.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
      height="526"
      stripe
      style="width: 100%">
      <el-table-column
        label="Tên sản phẩm"
        prop="name">
      </el-table-column>
      <el-table-column
        label="Số lượng"
        prop="quantity">
      </el-table-column>
      <el-table-column
        label="Đơn giá"
        prop="price">
      </el-table-column>
      <el-table-column
        label="Miêu tả"
        prop="description">
      </el-table-column>
      <el-table-column
      fixed="right"
      label="Operations">
        <template slot-scope="scope">
          <el-button
            size="mini"
            @click="onEdit(scope.$index, scope.row)">Edit</el-button>
          <el-button
            size="mini"
            type="danger"
            @click="onDeleteItem(scope.$index, scope.row)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ListProd',
  data() {
    return {
      products: [],
      search: '',
    }
  },
  mounted() {
    this.getProductList()
  },
  methods: {
    onEdit(index, row) {
      this.$emit('product', row)
      this.$emit('index-component', '4-2')
    },
    getProductList() {
      axios.get('http://localhost:3001/api/product')
      .then((res) => {
        console.log(res)
        if (res.data) {
          this.products = res.data
        }
      })
    },
    onDeleteItem(index, value) {
      this.products.splice(index, 1)
      axios.delete('http://localhost:3001/api/product/' + value._id)
    }
  },
}
</script>

<style lang="scss" module>
.screen {
  padding: 40px;
  display: flex;
  :global {
    .el-table {
      margin: auto;
    }
  }
}
</style>
