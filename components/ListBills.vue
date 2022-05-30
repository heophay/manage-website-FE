<template>
  <div :class="$style.screen">
    <el-table
      :data="bills.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
      height="500"
      stripe
      style="width: 100%">
      <el-table-column
        label="Mã hóa đơn"
        prop="id">
      </el-table-column>
      <el-table-column
        label="Tổng số tiền"
        prop="total_prices">
      </el-table-column>
      <el-table-column
        label="Ngày thanh toán"
        prop="datetime">
      </el-table-column>
      <el-table-column
        label="Nhân viên"
        prop="employee">
      </el-table-column>
      <el-table-column
      fixed="right"
      label="Operations">
        <template slot-scope="scope">
          <el-button
            size="mini"
            type="primary"
            @click="handleEdit(scope.$index, scope.row)">Xem chi tiết</el-button>
        </template>
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
export default {
  name: 'ListBills',
  data() {
    return {
      bills: [
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
        {
          id: 1,
          total_prices: 150000000,
          datetime: '26/5/2022',
          employee: 'NGuyễn Bá Rôn',
        },
      ],
      search:'',

    }
  },
}
</script>

<style lang="scss" module>
.screen {
  padding: 20px 40px;
}
</style>
