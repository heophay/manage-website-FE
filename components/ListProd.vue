<template>
  <div :class="$style.screen">
    <el-table
      :data="prods.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
      height="526"
      stripe
      style="width: 100%">
      <el-table-column
        label="Mã sp"
        prop="id">
      </el-table-column>
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
        prop="prices">
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
            @click="handleDelete(scope.$index, scope.row)">Delete</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'ListProd',
  data() {
    return {
      prods: [
        {
          id: 1,
          name: 'sp1',
          quantity: '20',
          prices: 500000,
          description: 'Mô tả sản phẩm 1',
        },
        {
          id: 2,
          name: 'sp2',
          quantity: '25',
          prices: 1500000,
          description: 'Mô tả sản phẩm 2',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
        {
          id: 3,
          name: 'sp3',
          quantity: '35',
          prices: 1500000,
          description: 'Mô tả sản phẩm 3',
        },
      ],
      search: '',
    }
  },
  methods: {
    onEdit(index, row) {
      this.$emit('product', row)
      this.$emit('index-component', '4-2')
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
