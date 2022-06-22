<template>
  <div :class="$style.screen">
    <el-table
      :data="customers"
      height="560"
      stripe
      style="width: 100%">
      <el-table-column
        label="Tên nhân viên"
        prop="fullname">
      </el-table-column>
      <el-table-column
        label="Số điện thoại"
        prop="phone">
      </el-table-column>
      <el-table-column
        label="Email"
        prop="email">
      </el-table-column>
      <el-table-column
        label="Username"
        prop="username">
      </el-table-column>
      <el-table-column
        label="is Admin"
        prop="isAdmin">
      </el-table-column>
    </el-table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'ListCustomers',
  data() {
    return {
      customers: [],
    }
  },
  mounted() {
    this.getCustomers()
  },
  methods: {
    getCustomers() {
      axios.get('http://localhost:3001/api/user').then((res) => {
        this.customers = res.data
        this.customers.forEach(function(e) {
          e.isAdmin = e.isAdmin.toString()
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
