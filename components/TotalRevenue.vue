<template>
  <div :class="$style.screen">
    <!-- Input -->
    <div :class="$style.screenInput">
      <span>Thống kê theo</span>
      <!-- Select item -->
      <div>
        <el-radio-group v-model="radioLabel">
          <el-radio-button label="Ngày"></el-radio-button>
          <el-radio-button label="Tháng"></el-radio-button>
          <el-radio-button label="Năm"></el-radio-button>
        </el-radio-group>
        <div>
          <el-select v-model="radioResult.day" placeholder="Ngày" :disabled="radioLabel === 'Tháng' || radioLabel === 'Năm'">
            <el-option
              v-for="item in getDays()"
              :key="item.value"
              size="mini"
              :value="item.value"
              :disabled="item.disabled">
            </el-option>
          </el-select>
          <el-select v-model="radioResult.month" placeholder="Tháng" :disabled="radioLabel === 'Năm'">
            <el-option
              v-for="item in getMonths()"
              :key="item.value"
              :value="item.value"
              :disabled="item.disabled">
            </el-option>
          </el-select>
          <el-select v-model="radioResult.year" placeholder="Năm">
            <el-option
              v-for="item in getYears()"
              :key="item.value"
              :value="item.value"
              :disabled="item.disabled">
            </el-option>
          </el-select>
        </div>
      </div>
      <!-- Button -->
      <el-button type="primary">Xác nhận</el-button>
    </div>
    <!-- Table -->
    <div :class="$style.listBills">
      <el-table
      :data="bills"
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
        label="Khách hàng"
        prop="customer">
      </el-table-column>
      <el-table-column
        label="Tổng sản phẩm"
        prop="total_product">
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
    <!-- Total -->
    <div :class="$style.screenTotal">
      <span :class="$style.totalLabel">Tổng</span>
      <div :class="$style.totalList">
        <div :class="$style.totalItem">
          <span :class="$style.totalItemLabel">Hóa đơn:</span>
          <span :class="$style.totalItemResult">10</span>
        </div>
        <div :class="$style.totalItem">
          <span :class="$style.totalItemLabel">Sản phẩm:</span>
          <span :class="$style.totalItemResult">150000000</span>
        </div>
        <div :class="$style.totalItem">
          <span :class="$style.totalItemLabel">Số tiền:</span>
          <span :class="$style.totalItemResult">150000000</span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      radioLabel: 'Tháng',
      radioResult: {
        day: null,
        month: null,
        year: null,
      },
      bills: [],
      payitems: [],
      customers: [],
    }
  },
  mounted() {
    this.getBills()
    this.getPayitem()
    this.getCustomer()
  },
  methods: {
    getDays() {
      const days = []
      for(let i = 1; i <= 31; i++) {
        days.push({
          value: i,
          label: i,
        })
      }
      return days
    },
    getMonths() {
      const months = []
      for(let i = 1; i <= 12; i++) {
        months.push({
          value: i,
          label: i,
        })
      }
      return months
    },
    getYears() {
      const years = []
      for(let i = 2021; i <= 2023; i++) {
        years.push({
          value: i,
          label: i,
        })
      }
      return years
    },
    getBills() {
      axios.get('http://localhost:3001/api/bill').then((res) => {
        this.bills = res.data.bills
        this.bills.forEach(function(e) {
          e.employee = JSON.parse(localStorage.getItem('user')).fullname
        })
      })
    },
    getPayitem() {
      const self = this
      axios.get('http://localhost:3001/api/payitem').then((res) => {
        self.payitems = res.data.payitems
        self.bills.forEach(function(bill) {
          const total = self.payitems.reduce(function(num, payitem) {
            return payitem.id_bill === bill._id ? num + 1 : num
          }, 0)
          bill.total_product = total
        })
      })
    },
    getCustomer() {
      const self = this
      axios.get('http://localhost:3001/api/customer').then((res) => {
        self.customers = res.data.customers
        self.bills.forEach(function(bill) {
          const tempCus = self.customers.find(function(customer) {
            return customer._id === bill.id_customer
          })
          bill.customer = tempCus.name
        })
        console.log(JSON.stringify(self.bills))
      })
    },
  },
}
</script>

<style lang="scss" module>
.screen {
  padding: 20px;
  flex-direction: column;
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;

  .screenInput {
    display: flex;
    align-items: center;
    border: 1px solid #ccc;
    width: 40%;
    justify-content: space-between;
    padding: 11px;
    border-radius: 8px;

    :global {
      .el-select {
        width: 80px;
      }
    }
  }
  .listBills {
    margin: 30px 0;
    border: 1px solid #9d9a9a;
    width: 85%;
    border-radius: 8px;
    overflow: hidden;
  }
  .screenTotal {
    display: flex;
    align-items: center;
    width: 50%;
    margin: 10px 0 0 0;

    .totalLabel {
      margin-right: 15px;
      font-size: 24px;
    }

    .totalList {
      display: flex;
      justify-content: space-between;
      width: 100%;
      border: 1px solid #ccc;
      padding: 20px;
      border-radius: 8px;
      background-color: #409eff;
      color: #fff;
      .totalItem {
        display: flex;
        align-items: center;
        .totalItemLabel {
          margin-right: 10px;
          font-size: 18px;
          font-weight: 400;
        }
        .totalItemResult {
          font-size: 18px;
        }
      }
    }
  }
}
</style>
