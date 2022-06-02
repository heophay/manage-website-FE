<template>
  <div :class="$style.screen">
    <div :class="$style.screenAdd">
      <el-row type="flex" :class="$style.row" justify="center" :gutter="20">
        <el-col :span="13">
          <div :class="$style.column">
            <span :class="$style.colLabel">Tên sản phẩm</span>
            <el-input
              v-model="prod.name"
              placeholder="Type product name"
              suffix-icon="el-icon-s-goods"
              :class="$style.colInput"
            />
          </div>
        </el-col>
        <el-col :span="9">
          <div :class="$style.column">
            <span :class="$style.colLabel">Giá bán</span>
            <el-input
              v-model="prod.price"
              placeholder="0"
              suffix-icon="el-icon-price-tag"
              :class="$style.colInput"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.row" justify="center">
        <el-col :span="8" :offset="13">
          <div :class="$style.column">
            <span :class="$style.colLabel">Số lượng</span>
            <el-input
              v-model="prod.quantity"
              placeholder="0"
              suffix-icon="el-icon-shopping-cart-2"
              :class="$style.colInputNum"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.row" justify="center">
        <el-col :span="15">
          <div :class="$style.column">
            <span :class="$style.colLabel">Mô tả</span>
            <el-input
              v-model="prod.description"
              type="textarea"
              :autosize="{ minRows: 2, maxRows: 4 }"
              placeholder="Please input the description"
              :class="$style.colInput"
            />
          </div>
        </el-col>
      </el-row>
      <el-row type="flex" :class="$style.row" justify="center">
        <el-button type="primary" @click="onClickAdd"
          >Thêm vào danh sách</el-button
        >
      </el-row>
    </div>
    <!-- List -->
    <div :class="$style.screenList">
      <el-table
        :data="
          prodsAdded.filter(
            (data) =>
              !search || data.name.toLowerCase().includes(search.toLowerCase())
          )
        "
        height="250"
        stripe
        style="width: 100%"
      >
        <el-table-column label="Tên sản phẩm" prop="name"> </el-table-column>
        <el-table-column label="Số lượng" prop="quantity"> </el-table-column>
        <el-table-column label="Đơn giá" prop="price"> </el-table-column>
        <el-table-column label="Miêu tả" prop="description"> </el-table-column>
        <el-table-column fixed="right" label="Operations">
          <template slot-scope="scope">
            <el-button
              type="primary"
              icon="el-icon-edit"
              size="medium"
              circle
              @click="onEdit(scope.$index, scope.row)"
            />
            <el-button
              type="danger"
              icon="el-icon-delete"
              size="medium"
              circle
              @click="onDelete(scope.$index)"
            />
          </template>
        </el-table-column>
      </el-table>
      <el-row type="flex" :class="$style.btnAdd" justify="center">
        <el-button type="primary">Thêm tất cả sản phẩm</el-button>
      </el-row>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AddProdComponent',
  props: {
    product: {
      type: Object,
      default: null,
    },
  },
  data() {
    return {
      prod: {
        name: 'San pham 1',
        price: 200000,
        quantity: 50,
        description: 'Đây là mô tả của sản phẩm 1',
      },
      prodsAdded: [
        {
          name: 'sp4',
          price: 500000,
          quantity: '20',
          description: 'Mô tả sản phẩm 4',
        },
      ],
      isEditProd: false,
      indexEdit: -1,
    }
  },
  watch: {
    product(newVal) {
      console.log(newVal)
      this.prod = newVal
    }
  },
  methods: {
    onClickAdd() {
      // if (this.isEditProd) {
      //   this.prodsAdded[this.indexEdit] = this.prod
      // } else {
      //   this.prodsAdded.push(this.prod)
      // }
      this.prodsAdded.push(this.prod)
      this.prod =
      {
          name: '',
          price: null,
          quantity: null,
          description: '',
      }
    },
    onDelete(index) {
      this.prodsAdded.splice(index, 1)
    },
    onEdit(index, row) {
      this.isEditProd = true
      this.prod = row
      this.indexEdit = index
    },
  },
}
</script>

<style lang="scss" module>
.screen {
  padding: 20px 60px;
  display: flex;
  flex-direction: column;
  align-items: center;

  .screenAdd {
    border: 2px solid var(--color-primary);
    width: 64%;
    padding: 20px 0;
    border-radius: 30px;

    .row {
      .column {
        margin-bottom: 10px;
        display: flex;
        margin-bottom: 10px;
        align-items: center;
        justify-content: flex-end;

        .colLabel {
          margin-right: 10px;
        }
        .colInput {
          width: 70%;
        }
        .colInputNum {
          width: 40%;
        }
      }
    }
  }
  .screenList {
    width: 100%;
    margin-top: 20px;
    border-top: 1px solid #ccc;

    .btnAdd {
      margin-top: 16px;
    }
  }
}
</style>
