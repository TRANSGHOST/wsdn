<template>
  <body id="poster">
  <el-form class="logup-container" label-position="left"
           label-width="0px">
    <h3 class="logup_title">系统注册</h3>
    <el-form-item>
      <el-input type="text" v-model="logupForm.username"
                auto-complete="off" placeholder="账号"></el-input>
    </el-form-item>
    <el-form-item>
      <el-input type="password" v-model="logupForm.password"
                auto-complete="off" placeholder="密码"></el-input>
    </el-form-item>
    <el-form-item style="width: 100%">
      <el-button type="primary" style="width: 100%;background: #505458;border: none" v-on:click="login">注册</el-button>
    </el-form-item>
  </el-form>
  </body>
</template>

<script>

  export default {
    name: 'Logup',
    data () {
      return {
        logupForm: {
          username: 'admin',
          password: '123'
        },
        responseResult: []
      }
    },
    methods: {
      login () {
        this.$axios
          .post('/logup', {
            username: this.logupForm.username,
            password: this.logupForm.password
          })
          .then(successResponse => {
            if (successResponse.data.code === 200) {
              this.$router.replace({path: '/index'})
            }
          })
          .catch(failResponse => {
          })
      }
    }
  }
</script>

<style>
  #poster {
    background:url("../assets/logo.jpg") no-repeat;
    background-position: center;
    height: 100%;
    width: 100%;
    background-size: cover;
    position: fixed;
  }
  body{
    margin: 0px;
  }
  .logup-container {
    border-radius: 15px;
    background-clip: padding-box;
    margin: 90px auto;
    width: 350px;
    padding: 35px 35px 15px 35px;
    background: #fff;
    border: 1px solid #eaeaea;
    box-shadow: 0 0 25px #cac6c6;
  }
  .logup_title {
    margin: 0px auto 40px auto;
    text-align: center;
    color: #505458;
  }

</style>
