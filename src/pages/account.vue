<template>
  <el-card id="account">

    <div id="updatePwd">
      <div>
        <h3>更改密码</h3>
        <el-form ref="form" label-width="80px">
          <el-form-item label="旧密码">
            <el-input v-model="oldPassword" placeholder="旧密码" show-password></el-input>
          </el-form-item>
          <el-form-item label="新密码">
            <el-input v-model="newPassword" placeholder="新密码" show-password></el-input>
          </el-form-item>
          <el-form-item label="确认密码">
            <el-input v-model="confirmPassword" placeholder="再次输入密码" show-password></el-input>
          </el-form-item>
        </el-form>
      </div>
      <div style="margin-left: 45%">
        <el-button icon="el-icon-potato-strips" @click="updatePwd()">更改密码</el-button>
      </div>
    </div>

    <el-divider/>

    <div id="updateMail">
      <div>
        <h3>改绑邮箱</h3>
        <el-form ref="form" label-width="100px">
          <el-form-item label="密码">
            <el-input v-model="password" placeholder="请输入用户密码" show-password></el-input>
          </el-form-item>
          <el-form-item label="新邮箱">
            <el-input v-model="newMail" placeholder="新邮箱"></el-input>
            <el-button type="text" style="width: 85%;" @click="updateMailSendMailToNew()">
              发送验证码
              <i class="el-icon-loading" v-if="updateMailToNewSendFlag"/>
            </el-button>
          </el-form-item>
          <el-form-item label="新邮箱验证码">
            <el-input v-model="newMailCode" placeholder="新邮箱验证码"></el-input>

          </el-form-item>
        </el-form>
      </div>
      <div style="margin-left: 45%">
        <el-button icon="el-icon-cherry" @click="updateMail()">改绑邮箱</el-button>
      </div>
    </div>

    <el-divider/>



  </el-card>
</template>

<script>

    export default {
        name: 'account',
        data() {
            return {
                oldPassword: '',
                newPassword: '',
                confirmPassword: '',
                updatePwdMailCode: '', // 修改密码验证码
                mail: '', //用户原邮箱
                oldMailCode: '', //原邮箱验证码
                newMail: '',    //新邮箱
                newMailCode: '',  //新邮箱验证码
                updatePwdSendFlag: false,
                updateMailToNewSendFlag: false,
                password: ''
            }
        },
        created() {
            this.load();
        },
        methods: {
            load() {
            },
            updateMailSendMailToNew() {  //更改密码发送验证码到新邮箱
                var reg = new RegExp(/^([a-zA-Z0-9._-])+@([a-zA-Z0-9_-])+(\.[a-zA-Z0-9_-])+/);
                if (!reg.test(this.newMail)) {//检测字符串是否符合正则表达式
                    this.$message({
                        message: '邮箱格式不正确',
                        type: 'error'
                    });
                    return;
                }
                this.updateMailToNewSendFlag = true;
                this.$axios
                    .post('/checkmail',{
                        mailAddress: this.newMail
                    })
                    .then(res=>{
                        this.$message({
                            message:'发送成功!',
                            type:'success'
                        });
                        this.updateMailToNewSendFlag=false;
                    }).catch(()=>{
                    this.updateMailToNewSendFlag=false;
                })
            },
            updatePwd() {
                if (this.oldPassword.length <= 0) {
                    this.$message({
                        message: '原密码不能为空',
                        type: 'error'
                    });
                    return;
                }

                if (this.newPassword.length < 6) {
                    this.$message({
                        message: '密码不能小于6位',
                        type: 'error'
                    });
                    return;
                }
                if (this.newPassword != this.confirmPassword) {
                    this.$message({
                        message: '两次输入的密码不一致',
                        type: 'error'
                    });
                    return;
                }
                this.$axios
                    .post('/changePwd',{
                        username:this.$store.state.name,
                        oldPassword: this.oldPassword,
                        newPassword: this.newPassword
                    })
                    .then(res=>{
                        this.$message({
                            message:'密码更改成功！',
                            type:'success'
                        })
                    })
            },
            updateMail() {
                var reg = new RegExp(/^([a-zA-Z0-9._-])+@([a-zA-Z0-9_-])+(\.[a-zA-Z0-9_-])+/);
                if (!reg.test(this.newMail)) {//检测字符串是否符合正则表达式
                    this.$message({
                        message: '邮箱格式不正确',
                        type: 'error'
                    });
                    return;
                }

                if (this.newMailCode.length <= 0) {//
                    this.$message({
                        message: '请填写验证码',
                        type: 'error'
                    });
                    return;
                }
                this.$axios
                    .post('/changemail',{
                        username: this.$store.state.name,
                        mailAddress:this.newMail,
                        password: this.password,
                        checkCode: this.newMailCode
                    })
                    .then(res=>{
                        this.$message({
                            message:'邮箱改绑成功！',
                            type:'success'
                        })
                    })
            }
        },
    }
</script>
<style scoped>
  #account {
    margin: 1% 5%;
    text-align: left;
  }

  #updatePwd {
    padding-left: 30%;
    width: 30%;
  }

  #updateMail {
    padding-left: 30%;
    width: 30%;
  }

  #updateReward {
    padding-left: 30%;
    width: 30%;
  }

  .avatar-uploader .el-upload {
    border: 1px dashed #000000;
    border-radius: 6px;
    cursor: pointer;
    position: relative;
    overflow: hidden;
  }

  .avatar-uploader .el-upload:hover {
    border-color: #000000;
  }

  .avatar-uploader-icon {
    font-size: 28px;
    color: #8c939d;
    width: 176px;
    height: 176px;
    text-align: center;
  }

  .avatar {
    width: 176px;
    height: 176px;
    display: block;
  }

</style>

