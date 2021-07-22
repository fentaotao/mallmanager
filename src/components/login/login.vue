<template>
  <div class="login-wrap">
    <el-form class="login-form" label-position="top" label-width="80px" :model="formdata">
      <h2>用户登录</h2>
      <el-form-item label="用户名">
        <el-input v-model="formdata.username"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formdata.password"></el-input>
      </el-form-item>
      <el-button @click.prevent="loginAysnc()" class="login-btn" type="primary">登录</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
  name: 'Login',

  data() {
    return {
      formdata: {
        username: '',
        password: '',
      },
    }
  },
  components: {},
  methods: {
    login() {
      this.$http.post('login', this.formdata).then((res) => {
        const {
          data,
          meta: { msg, status },
        } = res.data
        if (status === 200) {
          //登录成功
          //1.跳转到home
          this.$router.push({ name: 'home' })
          //2提示成功
          this.$message.success(msg)
        } else {
          //2登录不成功
          this.$message.warning(msg)
          //不成功 提示消息
        }
      })
    },
    //简化代码,使用aysnc和await
    async loginAysnc() {
      const res = await this.$http.post('login', this.formdata)
      const {
        data,
        meta: { msg, status },
      } = res.data
      if (status === 200) {
        //登录成功
        //保存token  目的：如果用户没有登录，通过复制url直接来到home组件也可以进入，现在不想这样，
        //在登录成功的时候保存正确用户的token
        localStorage.setItem('token', data.token)
        //1.跳转到home
        this.$router.push({ name: 'home' })
        //2提示成功
        this.$message.success(msg)
      } else {
        //2登录不成功
        this.$message.warning(msg)
        //不成功 提示消息
      }
    },
  },
}
</script>

<style scoped >
.login-wrap {
  height: 100%;
  background: #324152;
  display: flex;
  justify-content: center;
  align-items: center;
}
.login-wrap .login-form {
  width: 400px;
  background-color: #fff;
  border-radius: 5px;
  padding: 30px;
}
.login-wrap .login-btn {
  width: 100%;
}
</style>
