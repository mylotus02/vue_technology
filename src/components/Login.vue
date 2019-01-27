<template>
  <div class="login">
    <el-form ref="loginForm" :model="loginForm" :rules="rules" label-width="100px">
      <el-form-item label="用户名" prop="name">
        <el-input v-model="loginForm.name"  placeholder="请输入手机号码或用户名"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password"  v-model="loginForm.password" placeholder="请输入密码"></el-input>
      </el-form-item>

      <el-form-item>
        <el-button type="primary" class="width100" v-on:click="login('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>
  </div>
</template>

<script>
    export default {
        name: "Login",
      data() {
          return {
            loginForm: {
              name: '',
              password: ''
            },
            rules: {
              name: [
                {required: true, message: '请输入账号', trigger: 'blur'},
                {min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur'}
              ],
              password: [
                {required: true, message: '请输入密码', trigger: 'blur'},
                {min: 6, max: 12, message: '长度在 6 到 12 个字符', trigger: 'blur'}
              ]
            }
          }
      },
      methods: {
          login: function (formName) {
            this.$refs[formName].validate((valid) => {
              if (valid) {
                this.$http.get("/api/login?userName=" + this.loginForm.name + "&userPwd=" + this.loginForm.password).then(function (result) {
                  let resultInfo = JSON.parse(result.bodyText);
                  if (resultInfo.result == 0) {
                    alert("登录成功");
                  } else {
                    alert(resultInfo.info);
                  }
                }, function (err) {
                  alert(err);
                });
              } else {
                console.log('error submit!!');
                return false;
              }
            });
          }
      }
    }
</script>

<style scoped>
  .login{
    text-align: center;
    margin: 0 auto;
    margin-top: 200px;
    width: 500px;
  }
  .width100 {
    width: 100%;
  }
</style>
