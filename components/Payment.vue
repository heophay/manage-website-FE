<template>
  <div :class="$style.screen">
    <!-- Đơn hàng & Khách hàng-->
    <div :class="$style.screenMainInfo">
      <div :class="$style.screenMainInfoItem">
        <span :class="$style.labelItem">Nhân viên</span>
        <div :class="$style.row">
          <span :class="$style.rowLabel">Tên Nhân viên</span>
          <el-input
            v-model="user.fullname"
            suffix-icon="el-icon-user-solid"
            :class="$style.rowInput"
            :disabled="true"
          />
        </div>
        <div :class="$style.row">
          <span :class="$style.rowLabel">Phone</span>
          <el-input
            v-model="user.phone"
            suffix-icon="el-icon-phone"
            :class="$style.rowInput"
            :disabled="true"
          />
        </div>
      </div>
      <div :class="[$style.screenMainInfoItem, $style.screenCustomer]">
        <span :class="$style.labelItem">Khách hàng</span>
        <div :class="[$style.row]">
          <span :class="[$style.rowLabel, $style.rowLabelCustomer]">Tên khách hàng</span>
          <el-input
            v-model="customer.name"
            placeholder="Name"
            suffix-icon="el-icon-user-solid"
            :class="[$style.rowInput, $style.rowCustomer]"
            @input="checkValidPayment"
          />
        </div>
        <div :class="$style.row">
          <span :class="[$style.rowLabel, $style.rowLabelCustomer]">Số điện thoại</span>
          <el-input
            v-model="customer.phone"
            placeholder="Phone number"
            suffix-icon="el-icon-phone"
            :class="[$style.rowInput, $style.rowCustomer]"
            @input="checkValidPayment"
          />
          <el-button type="primary" :disabled="!customer.phone" @click="onSearchCustomer">Tìm</el-button>
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
                placeholder="Type product's name"
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
                suffix-icon="el-icon-shopping-bag-2"
                :class="[$style.colInput, $style.colQuantity]"
                @input="checkValidPayment"
              />
            </div>
          </el-col>
          <el-col :span="6" :offset="1">
            <div :class="$style.colProd">
              <span :class="$style.colLabel">Đơn giá</span>
              <el-input
                v-model="prod.price"
                suffix-icon="el-icon-price-tag"
                :class="$style.colInput"
                disabled
              />
            </div>
          </el-col>
          <el-button
            :disabled="listProdBuy.length === 1"
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
      <el-button type="primary" :disabled="!checkValidButton" @click="onPay">Thanh toán</el-button>
    </el-row>
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
  name: 'PaymentComponent',
  data() {
    return {
      customer: {
        name: '',
        phone: '',
        isExist: false,
      },
      listProdBuy:[],
      allProducts: [],
      productOptions: [],
      value: [],
      loading: false,
      options: [],
      user: {},
      checkValidButton: false,
      toast: {}
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
    this.getUser()
    this.onAddClick()
  },
  methods: {
    getUser() {
      this.user = JSON.parse(localStorage.getItem('user'))
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
    onAddClick() {
      this.listProdBuy.push({
        quantity: 0,
        price: 0,
      })
      this.value.push(null)
      this.checkValidPayment()
    },
    onDelete(prod) {
      const index = this.listProdBuy.indexOf(prod)
      if (index !== -1) {
        this.listProdBuy.splice(index, 1)
      }
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
      this.checkValidPayment()
    },
    async onPay() {
      let payCustomer = {}
      if (this.customer.isExist) {
        payCustomer = this.customer
      } else {
        const cus = await this.putCustomer()
        payCustomer = cus.data.customer
      }
      console.log(payCustomer)
      const newBill = await this.putBill(payCustomer._id)
      await this.putTransaction(newBill.data.bill._id)
    },
    putCustomer() {
      return axios.post('http://localhost:3001/api/customer/create',this.customer)
    },
    putBill(idCustomer) {
      const bill = {
        id_customer: idCustomer,
        id_user: this.user._id,
        total: this.totalPrice
      }
      return axios.post('http://localhost:3001/api/bill/create', bill)
    },
    putTransaction(idBill) {
      return axios.post('http://localhost:8080/api/addTransaction', this.getPayitemDB(idBill))
        .then((res) => {
          if (res) {
            axios.post('http://localhost:3001/api/payitem/create', this.getPayitemDB(idBill))
            this.showToast('success', 'Payment success!!', true)
            this.resetData()
          } else {
            this.showToast('error', 'Faild!!', true)
          }
        })
    },
    getPayitemDB(idBill) {
      return this.listProdBuy.map(function(e) {
        return {
          "id_bill": idBill,
          "id_product": e._id,
          "quantity": e.quantity,
        }
      })
    },
    checkValidPayment() {
      this.checkValidButton = this.checkValidCustomer() && this.checkValidProducts()
    },
    checkValidCustomer() {
      return !!this.customer.name && !!this.customer.phone
    },
    checkValidProducts() {
      const self = this
      return self.listProdBuy.reduce(function(check, e, index) {
        return check && self.value[index] !== null && !!Number(e.quantity)
      }, true)
    },
    resetData() {
      this.customer = {
        name: '',
        phone: '',
      }
      this.listProdBuy = []
      this.value = []
      this.onAddClick()
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
    },
    onSearchCustomer() {
      axios.get('http://localhost:3001/api/customer/' + this.customer.phone).then((res) => {
        if (res.data.customers) {
          this.customer = res.data.customers
          this.customer.isExist = true
        } else {
          this.customer.name = ''
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
      width: 28vw;

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
          width: 67%;
        }
      }
    }

    .screenCustomer {
      width: 30vw;
      .rowLabelCustomer {
        width: 120px;
      }
      .rowCustomer {
        flex: 1;
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
