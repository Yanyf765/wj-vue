<template>
  <body id="paper">
    <el-form :rules="rules" class="login-container" label-position="left"
             label-width="0px" v-loading="loading">
      <h3 class="login_title">系统登录</h3>
      <el-form-item>
        <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="账号"></el-input>
      </el-form-item>
      <el-form-item>
        <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="密码"></el-input>
      </el-form-item>
      <el-checkbox class="login_remember" v-model="checked" label-position="left">记住密码</el-checkbox>
      <el-form-item>
        <el-button type="primary" @click.native.prevent="login" style="width: 100%">登录</el-button>
      </el-form-item>
    </el-form>
    <div class="el-login-footer">
      <span>版权所有 @ 卓牛科技有限公司</span>
    </div>
  </body>
</template>
<script>
import {postRequest} from '../utils/api'
export default{
  data () {
    return {
      rules: {
        account: [{required: true, message: '请输入正确的用户名', trigger: 'blur'}],
        checkPass: [{required: true, message: '请输入正确的密码', trigger: 'blur'}]
      },
      checked: true,
      loginForm: {
        username: 'admin',
        password: 'stHkzy2rog'
      },
      loading: false
    }
  },
  methods: {
    login () {
      var _this = this
      postRequest('/sys/user/admin/login', {
        userName: this.loginForm.username,
        password: this.loginForm.password
      }).then(resp => {
        _this.loading = false
        if (resp.status === 200) {
          // 成功
          var json = resp.data
          if (json.success === true) {
            _this.$router.replace({path: '/home'})
            this.$store.commit('handleUserName', this.loginForm.username)
          } else {
            _this.$alert('登录失败!', '失败!')
          }
        } else {
          // 失败
          _this.$alert('登录失败!', '失败!')
        }
      }, resp => {
        _this.loading = false
        _this.$alert('找不到服务器⊙﹏⊙∥!', '失败!')
      })
    }
  }
}
</script>
<style>
  body{
    margin: 0px;
  }
  #paper {
    background:url("../assets/img/bg/login_bg.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  .login-container{
    border-radius: 15px;
    background-clip: padding-box;
    margin: 180px auto;
    width: 400px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }
  .login_title{
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }
  .login_remember{
    margin: 0px 0px 35px 0px;
    text-align: left;
  }
  .el-login-footer{
    text-align: center;
  }
</style>
