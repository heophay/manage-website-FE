<template>
  <div :class="$style.screen">
    <el-table
      :data="bills"
      height="400"
      stripe
      style="width: 100%">
      <el-table-column
        label="Mã hóa đơn"
        prop="_id">
      </el-table-column>
      <el-table-column
        label="Tổng số tiền"
        prop="total">
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
import axios from 'axios'
export default {
  name: 'ListBills',
  data() {
    return {
      bills: [],
    }
  },
  mounted() {
    this.getBills()
  },
  methods: {
    getBills() {
      axios.get('http://localhost:3001/api/bill').then((res) => {
        this.bills = res.data.bills
        this.bills.forEach(function(e) {
          e.employee = JSON.parse(localStorage.getItem('user')).fullname
        })
      })
    },
  },
}
</script>

<style lang="scss" module>
.screen {
  padding: 20px 40px;
}
</style>
