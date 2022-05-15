<template>
  <div class="login_container">
    <div class="login_box">
      <div class="avatar_box">
        <img src="../assets/logo.png">
      </div>
      <el-form class="login_form"
               :model="loginForm"
               :rules="loginRule"
               ref="loginFormRef"
      >
        <el-form-item prop="username">
          <el-input
            prefix-icon="el-icon-user"
            v-model="loginForm.username"
          />
        </el-form-item>
        <el-form-item prop="password" >
          <el-input
            prefix-icon="el-icon-lock"
            v-model="loginForm.password"
            show-password=""
          />
        </el-form-item>
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loginForm: {
        // eslint-disable-next-line no-unused-expressions
        username: 'admin',
        password: '123456'
      },
      loginRule: {
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 5, max: 15, message: '长度在 5 到 15 个字符', trigger: 'blur' }],
        password: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在 6 到 15 个字符', trigger: 'blur' }]
      }
    }
  },
  methods: {
    login() {
      // eslint-disable-next-line no-unused-expressions
      this.$refs.loginFormRef.validate(async (valid) => {
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        this.$message.success('登录成功')
        console.log(res)
        sessionStorage.setItem('token', res.data.token)
        this.$router.push('/home')
      //  result
      })
    },
    resetLoginForm() {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>

<style scoped>
  .login_container{
    background-color: #00a2d4;
    height: 100%;
  }
  .avatar_box{
    width: 130px;
    height: 130px;
    background-color: #FFFFFF;
    border-radius: 50%;
    border: 1px solid #eee;
    position: absolute;
    left: 50%;
    transform: translate(-50%,-50%);
    padding: 10px;
  }
  img{
    width: 100%;
    height: 100%;
    border-radius: 50%;
    background-color: #eeeeee;
  }
  .login_box{
    background-color: #FFFFFF;
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%,-50%);
    width: 450px;
    height: 300px;
    border-radius: 5px;
  }
  .login_form {
    position: absolute;
    bottom: 0;
    width: 100%;
    padding: 0 20px;
    box-sizing: border-box;
  }
  .btns{
    float: right;
  }
</style>

