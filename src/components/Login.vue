<template>
  <div>
    <div class="middle" :style="backgroundDiv">
      <img>
      <div class="table">
  <el-form ref="AccountFrom" :model="account" :rules="rules" label-position="left" label-width="0px"
           class="demo-ruleForm login-container">
    <h3 class="title">管理员登录</h3>
    <el-form-item prop="username">
      <el-input type="text" v-model="account.username" auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item prop="pwd">
      <el-input type="password" v-model="account.pwd" auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <!--<el-checkbox v-model="checked" checked class="remember">记住密码</el-checkbox>-->
    <el-form-item>
      <el-radio v-model="radio" label="1">用户</el-radio>
      <el-radio v-model="radio" label="2">管理员</el-radio>
    </el-form-item>
    <el-form-item style="width:100%;">
      <el-button type="primary"  @click.native.prevent="handleLogin" :loading="loading">登录</el-button>
      <el-button type="primary"  @click.native.prevent="handleRegister" :loading="loading">注册</el-button>
    </el-form-item>
  </el-form>
      </div>
    </div>
  </div>
</template>

<script>
  import API from '../api/api_user';

  export default {
    data() {
      return {
        loading: false,
        account: {
          username: '',
          pwd: ''
        },
        rules: {
          username: [
            {required: true, message: '请输入账号', trigger: 'blur'},
            //{ validator: validaePass }
          ],
          pwd: [
            {required: true, message: '请输入密码', trigger: 'blur'},
            //{ validator: validaePass2 }
          ]
        },
        checked: true,
        backgroundDiv: {
          backgroundImage: 'url(' + require('../assets/images/img1.jpg') + ')'
        },
        radio: '1'
      };
    },
    methods: {
      handleLogin() {
        let that = this;
        this.$refs.AccountFrom.validate((valid) => {
          if (valid) {
            this.loading = true;
            let loginParams = {user_name: this.account.username, user_password: this.account.pwd};
            API.login(loginParams).then(function (result) {
              that.loading = false;
              console.log(result);
              if (result.status == '200') { //如果前台页面响应成功
                //将返回的用户数据保存在localStorage中
                localStorage.setItem('access-user', JSON.stringify(result.data));
                that.$router.push({path: '/'});  //路由到后台页面
              } else {
                that.$message.error({showClose: true, message: result.errmsg || '登录失败', duration: 2000});
              }
            }, function (err) {
              that.loading = false;
              that.$message.error({showClose: true, message: err.toString(), duration: 2000});
            }).catch(function (error) {
              that.loading = false;
              console.log(error);
              that.$message.error({showClose: true, message: '请求出现异常', duration: 2000});
            });
          }
        });
      }
    }
  }
</script>
<style>
  body {
    background: #DFE9FB;
  }
</style>
<style lang="scss" scoped>
  .login-container {
    /*box-shadow: 0 0px 8px 0 rgba(0, 0, 0, 0.06), 0 1px 0px 0 rgba(0, 0, 0, 0.02);*/
    -webkit-border-radius: 5px;
    border-radius: 5px;
    -moz-border-radius: 5px;
    background-clip: padding-box;
    margin: 160px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;

    background: -ms-linear-gradient(top, #ace, #00C1DE); /* IE 10 */
    background: -moz-linear-gradient(top, #ace, #00C1DE); /*火狐*/
    background: -webkit-gradient(linear, 0% 0%, 0% 100%, from(#ace), to(#00C1DE)); /*谷歌*/
    background: -webkit-linear-gradient(top, #ace, #00C1DE); /*Safari5.1 Chrome 10+*/
    background: -o-linear-gradient(top,#ace, #00C1DE);} /*Opera 11.10+*/

    .title {
      margin: 0px auto 40px auto;
      text-align: center;
      color: #505458;
    }
    .remember {
      margin: 0px 0px 35px 0px;
    }
  .middle {
    background: no-repeat ;
    height:200%;
    width:100%;
    overflow: hidden;
    background-size:cover
  }
</style>
