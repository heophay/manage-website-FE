<template>
  <div :class="$style.infoScreen">
    <!-- Đơn hàng -->
    <div>
      <span :class="$style.labelPay">Đơn hàng</span>
      <el-row type="flex" :class="$style.rowInfo">
        <el-col :span="10">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Đơn số</span>
            <el-input
              v-model="payment.id"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
              :disabled="true"
            />
          </div>
        </el-col>
        <el-col :span="10">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Nhân viên</span>
            <el-input
              v-model="payment.employee"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
      </el-row>
    </div>
    <!-- Khách hàng -->
    <div>
      <span :class="$style.labelPay">Khách hàng</span>
      <el-row type="flex" :class="$style.rowInfo">
        <el-col :span="10">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Tên khách hàng</span>
            <el-input
              v-model="payment.customer"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
              :disabled="true"
            />
          </div>
        </el-col>
        <el-col :span="10">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Số điện thoại</span>
            <el-input
              v-model="payment.phone_customer"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
      </el-row>
    </div>

    <!-- Sản phẩm -->
    <div>
      <span :class="$style.labelPay">Sản phẩm</span>
      <el-row type="flex" :class="$style.rowInfo">
        <el-col :span="10">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Sản phẩm 1</span>
            <el-input
              v-model="listProd[0].name"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
        <el-col :span="5">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Số lượng</span>
            <el-input
              v-model="listProd[0].num"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
        <el-col :span="7">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Đơn giá</span>
            <el-input
              v-model="listProd[0].price"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.rowInfo">
        <el-col :span="10">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Sản phẩm 2</span>
            <el-input
              v-model="listProd[1].name"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
        <el-col :span="5">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Số lượng</span>
            <el-input
              v-model="listProd[1].num"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
        <el-col :span="7">
          <div :class="$style.colInfo">
            <span :class="$style.colInfoLabel">Đơn giá</span>
            <el-input
              v-model="listProd[1].price"
              placeholder="Pick a date"
              suffix-icon="el-icon-date"
              :class="$style.colInforInput"
            />
          </div>
        </el-col>
      </el-row>
      <el-row>
        <el-col :span="10" :offset="2">
          <el-button type="primary">Thêm</el-button>
          <el-button>Xóa</el-button>
        </el-col>
      </el-row>
      <el-row type="flex" justify="end">
        <el-col :span="3" :offset="2">
          <span>Tổng</span>
          <span>{{ totalPrice }}</span>
        </el-col>
      </el-row>
    </div>


    <el-row type="flex" :class="$style.rowInfo" justify="center">
      <el-button type="primary">Thanh toán</el-button>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'PaymentComponent',
  data() {
    return {
      payment: {
        id: '10',
        employee: 'Nguyễn Bá Huy',
        customer: 'Huy Bá Nguyễn',
        phone_customer: '13464854',
      },
      listProd: [
        {
          name: 'sp1',
          num: 5,
          price: 500.000,
        },
        {
          name: 'sp2',
          num: 10,
          price: 1500.000,
        },
      ]
    }
  },
  computed: {
    totalPrice() {
      return this.listProd.reduce((accumulator, currentValue) => {
        return accumulator + currentValue.price * currentValue.num
      }, 0)
    }
  },
}
</script>

<style lang="scss" module>
.infoScreen {
  padding: 30px 60px;

  .labelPay {
    font-size: 20px;
    font-weight: 600;
    line-height: 22px;
    margin-left: 10px;
  }

  .rowInfo {
    margin-bottom: 20px;
    margin-top: 20px;

    .colInfo {
      margin-bottom: 10px;
      display: flex;
      padding: 0px 30px;
      margin-bottom: 10px;
      align-items: center;
      justify-content: flex-end;

      .colInfoLabel {
        margin-right: 24px;
      }
      .colInforInput {
        width: 56%;
      }
  }
  }


  :global {
    .el-row {
      border-bottom: 0px solid #fff;
    }
  }
}
</style>
