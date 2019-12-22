<template>
  <div class="app-container">
    <el-collapse v-model="activeNames" style="padding-bottom:10px">
      <el-collapse-item name="1">
        <template slot="title">
          <i class="header-icon el-icon-info"></i>查询条件
        </template>
        <el-form ref="form" :inline="true" :model="form" label-width="120px">
          <el-form-item label="姓名">
            <el-input v-model="form.userName" />
          </el-form-item>
          <el-form-item label="登陆名">
            <el-input v-model="form.loginName" />
          </el-form-item>
          <el-form-item label="性别">
            <el-select v-model="form.region" placeholder="--不限--">
              <el-option label="男" value="1" />
              <el-option label="女" value="0" />
            </el-select>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSearch">查询</el-button>
            <el-button @click="onReset">重置</el-button>
          </el-form-item>
        </el-form>
      </el-collapse-item>
    </el-collapse>
    <div></div>
    <el-table
      v-loading="listLoading"
      :data="list"
      element-loading-text="Loading"
      border
      fit
      highlight-current-row
    >
      <el-table-column align="center" label="序号" width="95">
        <template slot-scope="scope">{{ scope.$index }}</template>
      </el-table-column>
      <el-table-column label="姓名">
        <template slot-scope="scope">{{ scope.row.loginName }}</template>
      </el-table-column>
      <el-table-column label="登陆名" align="center">
        <template slot-scope="scope">
          <span>{{ scope.row.userName }}</span>
        </template>
      </el-table-column>
      <el-table-column label="性别" align="center">
        <template slot-scope="scope">{{ scope.row.sex }}</template>
      </el-table-column>
      <el-table-column class-name="status-col" label="手机号" align="center">
        <template slot-scope="scope">{{ scope.row.mobile }}</template>
      </el-table-column>
      <el-table-column align="center" prop="created_at" label="Email">
        <template slot-scope="scope">
          <span>{{ scope.row.email }}</span>
        </template>
      </el-table-column>
    </el-table>

    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="currentPage"
      :page-sizes="[5, 10, 20,50, 100]"
      :page-size="10"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total"
    ></el-pagination>
  </div>
</template>



<script>
import { getUsers } from "@/api/user";

export default {
  filters: {
    statusFilter(status) {
      const statusMap = {
        published: "success",
        draft: "gray",
        deleted: "danger"
      };
      return statusMap[status];
    }
  },
  data() {
    return {
      list: null,
      total: 0,
      currentPage: 1,
      listLoading: true,
      form: {
        userName: "",
        loginName: "",
        sex: ""
      },
      activeNames: ["1"]
    };
  },
  created() {
    this.fetchData();
  },
  methods: {
    fetchData() {
      this.listLoading = true;
      getUsers().then(response => {
        this.list = response.data.items;
        this.total = response.data.total;
        this.listLoading = false;
      });
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    },
    onSearch() {
      this.$message('submit!')
    },
    onReset() {
      this.$message({
        message: 'cancel!',
        type: 'warning'
      })
    }
  }
};
</script>
