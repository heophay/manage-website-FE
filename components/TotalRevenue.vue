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
          <el-select v-model="radioResult.day" placeholder="Ngày">
            <el-option
              v-for="item in getDays()"
              :key="item.value"
              size="mini"
              :value="item.value"
              :disabled="item.disabled">
            </el-option>
          </el-select>
          <el-select v-model="radioResult.month" placeholder="Tháng">
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
      :data="bills.filter(data => !search || data.name.toLowerCase().includes(search.toLowerCase()))"
      height="314"
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
        label="Khách hàng"
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
export default {
  data() {
    return {
      radioLabel: 'Tháng',
      radioResult: {
        day: null,
        month: null,
        year: null,
      },
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
      ],
    }
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
    }
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
    margin: 40px 0;
    border: 1px solid #ccc;
    width: 80%;
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
