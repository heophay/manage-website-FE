<template>
  <div :class="$style.screen">
    <!-- Đơn hàng & Khách hàng-->
    <div :class="$style.screenMainInfo">
      <div :class="$style.screenMainInfoItem">
        <span :class="$style.labelItem">Đơn hàng</span>
        <div :class="$style.row">
          <span :class="$style.rowLabel">Đơn số</span>
          <el-input
            v-model="payment.id"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="$style.rowInput"
            :disabled="true"
          />
        </div>
        <div :class="$style.row">
          <span :class="$style.rowLabel">Nhân viên</span>
          <el-input
            v-model="payment.employee"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="$style.rowInput"
            :disabled="true"
          />
        </div>
      </div>
      <div :class="[$style.screenMainInfoItem, $style.screenCustomer]">
        <span :class="$style.labelItem">Khách hàng</span>
        <div :class="$style.row">
          <span :class="$style.rowLabel">Tên khách hàng</span>
          <el-input
            v-model="payment.customer.name"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="[$style.rowInput, $style.rowCustomer]"
          />
        </div>
        <div :class="$style.row">
          <span :class="$style.rowLabel">Số điện thoại</span>
          <el-input
            v-model="payment.customer.phone"
            placeholder="Pick a date"
            suffix-icon="el-icon-date"
            :class="[$style.rowInput, $style.rowCustomer]"
          />
        </div>
      </div>
    </div>

    <!-- Sản phẩm -->
    <div :class="$style.screenProd">
      <span :class="$style.labelPay">Sản phẩm</span>
      <div :class="$style.listProd">
        <el-row
          v-for="(prod, index) in listProdBuy"
          :key="index"
          type="flex"
          :class="$style.rowProd"
          justify="center"
        >
          <el-col :span="8">
            <div :class="$style.colProd">
              <span :class="$style.colLabel">Sản phẩm {{ index + 1 }}</span>
              <el-select
                v-model="value[index]"
                filterable
                remote
                reserve-keyword
                placeholder="Please enter a keyword"
                :remote-method="remoteMethod"
                :loading="loading"
                @change="onChangeValue"
              >
                <el-option
                  v-for="item in options"
                  :key="item.value"
                  :label="item.label"
                  :value="item.value"
                >
                </el-option>
              </el-select>
            </div>
          </el-col>
          <el-col :span="5" :offset="1">
            <div :class="$style.colProd">
              <span :class="$style.colLabel">Số lượng</span>
              <el-input
                v-model="prod.quantity"
                placeholder="Pick a date"
                suffix-icon="el-icon-date"
                :class="[$style.colInput, $style.colQuantity]"
              />
            </div>
          </el-col>
          <el-col :span="6" :offset="1">
            <div :class="$style.colProd">
              <span :class="$style.colLabel">Đơn giá</span>
              <el-input
                v-model="prod.price"
                placeholder="Pick a date"
                suffix-icon="el-icon-date"
                :class="$style.colInput"
              />
            </div>
          </el-col>
          <el-button
            type="danger"
            icon="el-icon-delete"
            size="medium"
            circle
            @click="onDelete(prod)"
          />
        </el-row>
      </div>

      <el-row type="flex" :class="$style.rowBtnAdd" justify="space-between">
        <el-col :span="10" :offset="2">
          <el-button type="primary" @click="onAddClick">Thêm</el-button>
        </el-col>
        <el-col :span="4" :offset="2">
          <span>Tổng</span>
          <span :class="$style.totalValue">{{ totalPrice }}</span>
        </el-col>
      </el-row>
    </div>

    <el-row type="flex" :class="$style.rowPayment" justify="center">
      <el-button type="primary" @click="onPay">Thanh toán</el-button>
    </el-row>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'PaymentComponent',
  data() {
    return {
      payment: {
        id: '10',
        employee: 'Nguyễn Bá Huy',
        customer: {
          name: 'Huy Bá Nguyễn',
          phone: '13464854'
        }
      },
      listProdBuy:[],
      allProducts: [],
      productOptions: [],
      value: [],
      loading: false,
      options: [],
      id_user: JSON.parse(localStorage.getItem('user'))._id,
    }
  },
  computed: {
    totalPrice() {
      return this.listProdBuy.reduce((accumulator, currentValue) => {
        return accumulator + currentValue.price * currentValue.quantity
      }, 0)
    },
  },
  mounted() {
    this.getProduct()
  },
  methods: {
    onAddClick() {
      this.listProdBuy.push({
        quantity: 0,
        price: 0,
      })
      this.value.push(null)
    },
    onDelete(prod) {
      const index = this.listProdBuy.indexOf(prod)
      if (index !== -1) {
        this.listProdBuy.splice(index, 1)
      }
    },
    getProduct() {
      axios.get('http://localhost:3001/api/product').then((res) => {
        if (res.data) {
          this.allProducts = res.data
          this.productOptions = this.getProdOptions()
        }
      })
    },
    getProdOptions() {
      return this.allProducts.map((e) => {
        return { value: e._id, label: e.name }
      })
    },
    remoteMethod(query) {
      if (query !== '') {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.options = this.productOptions.filter((item) => {
            return item.label.toLowerCase().includes(query.toLowerCase())
          })
        }, 200)
      } else {
        this.options =  this.productOptions
      }
    },
    onChangeValue(value) {
      const index = this.value.indexOf(value)
      const temp = this.allProducts.find((e) => {
        return e._id === value
      })
      this.listProdBuy[index]._id = temp._id
      this.listProdBuy[index].price = temp.price
    },
    async onPay() {
      const newCustomer = await axios.post('http://localhost:3001/api/customer/create',this.payment.customer)
      const bill = {
        total: this.totalPrice,
        id_user: this.id_user,
        id_customer: newCustomer.data.customer._id
      }
      const newBill = await axios.post('http://localhost:3001/api/bill/create', bill)
      await axios.post('http://localhost:3001/api/payitem/create', this.getPayitemDB(newBill.data.bill._id))
    },
    getPayitemDB(idBill) {
      return this.listProdBuy.map(function(e) {
        return {
          id_bill: idBill,
          id_product: e._id,
          quantity: e.quantity,
        }
      })
    }
  },
}
</script>

<style lang="scss" module>
.screen {
  padding: 15px 60px;
  display: flex;
  flex-direction: column;
  align-items: center;

  .screenMainInfo {
    display: flex;
    align-items: center;
    justify-content: space-evenly;
    width: 100%;

    .screenMainInfoItem {
      border: 1px solid var(--color-primary);
      margin-bottom: 20px;
      padding: 20px 32px;
      border-radius: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
      width: 26vw;

      .labelItem {
        font-size: 22px;
        font-weight: 600;
        margin-bottom: 12px;
      }

      .row {
        display: flex;
        margin-bottom: 10px;
        align-items: center;
        justify-content: flex-end;
        width: 100%;

        .rowLabel {
          margin-right: 12px;
        }

        .rowInput {
          width: 74%;
        }
      }
    }

    .screenCustomer {
      width: 30vw;

      .rowCustomer {
        width: 67% !important;
      }
    }
  }

  .screenProd {
    border: 1px solid var(--color-primary);
    width: 100%;
    padding: 20px 30px;
    border-radius: 20px;
    height: 334px;

    .labelPay {
      font-size: 20px;
      font-weight: 600;
      line-height: 22px;
    }
    .listProd {
      max-height: 200px;
      margin-top: 10px;
      overflow-y: auto;
      .rowProd {
        display: flex;
        align-items: center;
        margin: 10px 0;

        .colProd {
          .colInput {
            width: 70%;
            &.colQuantity {
              width: 50%;
            }
          }
        }
      }
    }
    .rowBtnAdd {
      margin-top: 24px;
      display: flex;
      align-items: center;

      .totalValue {
        font-size: 24px;
        font-weight: 600;
        border: 1px solid #ccc;
        padding: 12px;
        border-radius: 8px;
      }
    }
  }

  .rowPayment {
    margin-top: 16px;
  }

  :global {
    .el-row {
      border-bottom: 0px solid #fff;
    }
  }
}
</style>
