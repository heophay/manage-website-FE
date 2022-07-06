<template>
  <div :class="$style.totalRevenue">
    <!-- Input -->
    <div :class="$style.screenInput">
      <span>Thống kê theo</span>
      <!-- Select item -->
      <div>
        <el-radio-group v-model="radioLabel">
          <el-radio-button label="day">Ngày</el-radio-button>
          <el-radio-button label="month">Tháng</el-radio-button>
          <el-radio-button label="year">Năm</el-radio-button>
        </el-radio-group>
        <div>
          <el-select v-model="radioResult.day" placeholder="Ngày" :disabled="radioLabel === 'month' || radioLabel === 'year'">
            <el-option
              v-for="item in getDays()"
              :key="item.value"
              size="mini"
              :value="item.value"
              :disabled="item.disabled">
            </el-option>
          </el-select>
          <el-select v-model="radioResult.month" placeholder="Tháng" :disabled="radioLabel === 'year'">
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
      <el-button type="primary" @click="filterBill">Xác nhận</el-button>
    </div>
    <!-- Table -->
    <div :class="$style.listBills">
      <el-table
      :data="showBills"
      stripe
      style="width: 100%"
      height="330px">
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
      label="Operations">
        <template slot-scope="scope">
          <el-popover
            placement="right"
            width="280"
            trigger="click">
            <el-table :data="getTransactionFromBill(scope.$index, scope.row)">
              <el-table-column width="150" property="name" label="name"></el-table-column>
              <el-table-column width="100" property="quantity" label="quantity"></el-table-column>
            </el-table>
            <el-button slot="reference" type="primary">Detail</el-button>
          </el-popover>
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
          <span :class="$style.totalItemResult">{{ total.totalBill }}</span>
        </div>
        <div :class="$style.totalItem">
          <span :class="$style.totalItemLabel">Sản phẩm:</span>
          <span :class="$style.totalItemResult">{{ total.totalProduct }}</span>
        </div>
        <div :class="$style.totalItem">
          <span :class="$style.totalItemLabel">Số tiền:</span>
          <span :class="$style.totalItemResult">{{ total.totalMoney }}</span>
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
      radioLabel: 'month',
      radioResult: {
        day: null,
        month: null,
        year: null,
      },
      bills: [],
      showBills: [],
      payitems: [],
      customers: [],
      products: [],
      user: {},
      total:{},
    }
  },
  watch: {
    radioLabel(newVal) {
      const self = this
      switch (newVal) {
        case 'month':
          self.radioResult.day = null
          break
        case 'year':
          self.radioResult.day = null
          self.radioResult.month = null
          break
      }
    },
    bills(newVal) {
      this.showBills = newVal
    }
  },
  mounted() {
    this.getUser()
    this.getProduct()
    this.getBills()
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
    getUser() {
      this.user = JSON.parse(localStorage.getItem('user'))
    },
    getBills() {
      const url = this.user.isAdmin
        ? 'http://localhost:3001/api/bill/'
        : 'http://localhost:3001/api/bill/' + this.user._id
      axios.get(url).then((res) => {
        this.bills = res.data.bills
        this.getCustomer()
      })
    },
    getPayitem() {
      const self = this
      axios.get('http://localhost:8080/api/queryallTransaction').then((res) => {
        self.payitems = JSON.parse(res.data.transactions.replace(/\//g,''))
        self.showBills.forEach(function(bill) {
          const total = self.payitems.reduce(function(num, payitem) {
            return payitem.Record.id_bill === bill._id ? num + 1 : num
          }, 0)
          bill.total_product = total
        })
        this.getTotal()
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
        this.getPayitem()
      })
    },
    getProduct() {
      const self = this
      axios.get('http://localhost:3001/api/product').then((res) => {
        self.products = res.data
      })
    },
    getTotal() {
      this.total = this.showBills.reduce(function(total, e) {
        total.totalBill++
        total.totalMoney = total.totalMoney + e.total
        total.totalProduct = total.totalProduct + e.total_product
        return total
      }, {
        totalBill: 0,
        totalMoney: 0,
        totalProduct: 0,
      })
    },
    filterBill() {
      const self = this
       self.showBills = self.bills.filter(function(e) {
        return self.getDateFromString(e.datetime).getFullYear() === self.radioResult.year
            && (self.getDateFromString(e.datetime).getMonth() + 1 === self.radioResult.month || !self.radioResult.month)
            && (self.getDateFromString(e.datetime).getDate() === self.radioResult.day || !self.radioResult.day)
      })
      this.getTotal()
    },
    getDateFromString(str) {
      return new Date(str)
    },
    getTransactionFromBill(index, value) {
      const self = this
      const transactionsTemp = this.payitems.filter(function(e) {
        return e.Record.id_bill === value._id
      })
      return transactionsTemp.map(function(e) {
        return {
          _id: e.Record._id,
          name: self.getNameProductFromTransaction(e.Record.id_product),
          quantity: e.Record.quantity
        }
      })
    },
    getNameProductFromTransaction(idProduct) {
      const product = this.products.find(function(e) {
        return e._id === idProduct
      })
      return product.name
    }
  },
}
</script>

<style lang="scss" module>
.totalRevenue {
  position: relative;
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
        margin-right: -10px;
      }
    }
  }
  .listBills {
    margin: 20px 0;
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
