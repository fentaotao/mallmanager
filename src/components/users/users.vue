<template>
  <el-card class="box-card">
    <el-breadcrumb separator="/">
      <el-breadcrumb-item>首页</el-breadcrumb-item>
      <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      <el-breadcrumb-item>用户列表</el-breadcrumb-item>
    </el-breadcrumb>

    <el-row class="searchRow">
      <el-col>
        <el-input placeholder="请输入内容" v-model="query" class="inputSearch">
          <el-button slot="append" icon="el-icon-search"></el-button>
        </el-input>
        <el-button type="success">添加用户</el-button>
      </el-col>
    </el-row>

    <el-table :data="userlist" stripe style="width: 100%">
      <el-table-column type="index" label="编号" width="60"></el-table-column>
      <el-table-column prop="username" label="姓名" width="80"></el-table-column>
      <el-table-column prop="email" label="邮箱"></el-table-column>
      <el-table-column prop="mobile" label="电话"></el-table-column>
      <el-table-column prop="create_time" label="创建日期"></el-table-column>
      <el-table-column prop="mg_state" label="用户状态"></el-table-column>
      <el-table-column prop="address" label="操作"></el-table-column>
    </el-table>
  </el-card>
</template>

<script>
export default {
  name: '',

  data() {
    return {
      query: '',
      pagenum: 1,
      pagesize: 2,
      //分页相关
      total: -1,
      userlist: [],
    }
  },
  components: {},
  created() {
    this.getUserList()
  },
  methods: {
    //获取用户列表请求
    async getUserList() {
      //需要授权的 API ，必须在请求头中使用 `Authorization` 字段提供 `token` 令牌
      const AUTH_TOKEN = localStorage.getItem('token')
      this.$http.defaults.headers.common['Authorization'] = AUTH_TOKEN
      const res = await this.$http.get(
        `users?query=${this.query}&pagenum=${this.pagenum}&pagesize=${this.pagesize}`
      )

      console.log(res.data)
      const {
        meta: { status, msg },
        data: { users, total },
      } = res.data
      if (status === 200) {
        //给表格数据赋值
        this.userlist = users
        //给total赋值
        this.total = total
        this.$message.success(msg)
      } else {
        this.$message.warning(msg)
      }
    },
  },
}
</script>

<style scoped >
.box-card {
  height: 100%;
}
.inputSearch {
  width: 300px;
}
.searchRow {
  margin-top: 20px;
}
</style>
