<template>
  <common-layout>
    <div class="top">
      <div class="header">
        <img alt="logo" class="logo" src="@/assets/imgs/logo.png">
        <span class="title">{{ systemName }}</span>
      </div>
      <div class="desc">Mellion Admin 是一款后台管理系统通用脚手架</div>
    </div>
    <div class="login">
      <el-form ref="loginForm" :model="loginForm" :rules="loginRules" autocomplete="on" label-position="left">
        <el-tabs style="padding: 0 2px;" value="first" stretch>
          <transition name="el-fade-in-linear">
            <el-alert
              v-if="errorMsg"
              title="您输入的账号或者密码不正确，请重新输入"
              type="error"
              center
              show-icon
            />
          </transition>
          <el-tab-pane label="账号密码登录" name="first">
            <el-form-item prop="username">
              <el-input
                ref="username"
                v-model="loginForm.username"
                placeholder="请输入账号"
                name="username"
                type="text"
                tabindex="1"
                autocomplete="on"
                @focus="errorMsg = false"
              >
                <i slot="prefix" class="el-icon-user" style="margin-top: 13px; margin-left: 4px; color: black;" />
              </el-input>
            </el-form-item>
            <el-form-item prop="password">
              <el-input
                :key="passwordType"
                ref="password"
                v-model="loginForm.password"
                :type="passwordType"
                placeholder="请输入密码"
                name="password"
                tabindex="2"
                autocomplete="on"
                @keyup.enter.native="handleLogin"
                @focus="errorMsg = false"
              >
                <i slot="prefix" class="el-icon-lock" style="margin-top: 13px; margin-left: 4px; color: black;" />
              </el-input>
              <span class="show-pwd" @click="showPwd">
                <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
              </span>
            </el-form-item>
          </el-tab-pane>
          <el-tab-pane label="手机号码登录" name="second">
            <el-form-item>
              <el-input placeholder="请输入手机号码">
                <i slot="prefix" class="el-icon-user" style="margin-top: 13px; margin-left: 4px; color: black;" />
              </el-input>
            </el-form-item>
            <el-form-item>
              <el-row :gutter="8" style="margin: 0 -4px">
                <el-col :span="16">
                  <el-input placeholder="验证码">
                    <i slot="prefix" class="el-icon-message" style="margin-top: 13px; margin-left: 4px; color: black;" />
                  </el-input>
                </el-col>
                <el-col :span="8" style="padding-left: 4px">
                  <el-button style="height: 40px; width: 100%; margin-top: 8px;" class="captcha-button">获取验证码</el-button>
                </el-col>
              </el-row>
            </el-form-item>
          </el-tab-pane>
        </el-tabs>
        <el-form-item prop="autoLogin">
          <el-checkbox v-model="loginForm.autoLogin" label="自动登录" :checked="true" />
          <a style="float: right">忘记密码</a>
        </el-form-item>
        <el-form-item>
          <el-button
            :loading="loading"
            type="primary"
            style="font-size: 16px; letter-spacing: 8px; height: 40px; width: 100%;"
            @click.native.prevent="handleLogin"
          >登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </common-layout>
</template>

<script>
import CommonLayout from '@/layout/CommonLayout'
import { validUsername } from '@/utils/validate'

export default {
  name: 'Login',
  components: { CommonLayout },
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!validUsername(value)) {
        callback(new Error('请输入正确的账号！'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      var reg = new RegExp('^(?=.*[a-zA-Z0-9].*)(?=.*[a-zA-Z.!@#$%^&*].*)(?=.*[0-9.!@#$%^&*].*).{6,32}$')
      if (!reg.test(value)) {
        callback(new Error('请输入正确的密码！'))
      } else {
        callback()
      }
    }
    return {
      systemName: 'Mellion Admin',
      loginForm: {
        username: '',
        password: '',
        autoLogin: true
      },
      loginRules: {
        username: [{ required: true, trigger: 'blur', validator: validateUsername }],
        password: [{ required: true, trigger: 'blur', validator: validatePassword }]
      },
      passwordType: 'password',
      loading: false,
      errorMsg: false
    }
  },
  mounted() {
    // if (this.loginForm.username === '') {
    //   this.$refs.username.focus()
    // } else if (this.loginForm.password === '') {
    //   this.$refs.password.focus()
    // }
  },
  methods: {
    showPwd() {
      if (this.passwordType === 'password') {
        this.passwordType = ''
      } else {
        this.passwordType = 'password'
      }
      this.$nextTick(() => {
        this.$refs.password.focus()
      })
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('user/login', this.loginForm)
            .then(() => {
              this.$router.push({ path: this.redirect || '/' })
              this.loading = false
            })
            .catch(() => {
              this.loading = false
            })
        } else {
          console.log('error submit!!')
          this.errorMsg = true
          return false
        }
      })
    }
  }
}
</script>

<style scoped lang="scss">
.common-layout{
    .top {
      text-align: center;
      .header {
        height: 44px;
        line-height: 44px;
        a {
          text-decoration: none;
        }
        .logo {
          height: 44px;
          vertical-align: top;
          margin-right: 16px;
        }
        .title {
          font-size: 33px;
          // color: @title-color;
          font-family: 'Myriad Pro', 'Helvetica Neue', Arial, Helvetica, sans-serif;
          font-weight: 600;
          position: relative;
          top: 2px;
        }
      }
      .desc {
        font-size: 14px;
        color: rgba(0, 0, 0, 0.45);
        margin-top: 12px;
        margin-bottom: 40px;
      }
    }
    .login{
      width: 368px;
      margin: 0 auto;
      @media screen and (max-width: 576px) {
        width: 95%;
      }
      @media screen and (max-width: 320px) {
        .captcha-button{
          font-size: 14px;
        }
      }
      .icon {
        font-size: 24px;
        // color: @text-color-second;
        margin-left: 16px;
        vertical-align: middle;
        cursor: pointer;
        transition: color 0.3s;

        // &:hover {
        //   color: @primary-color;
        // }
      }
    }
  }
::v-deep .el-tabs__nav-wrap::after {
  height: 0;
}
::v-deep .el-tabs__nav-scroll{
	width:65%;
	margin:0 auto
}
::v-deep .el-tabs__item {
  font-size: 16px;
}
// ::v-deep .el-tabs__active-bar {
//   background-color: transparent !important;
//   background-image: linear-gradient(
//     90deg, transparent 0, transparent 27%,
//     #4d72f6 0, #4d72f6 73%,
//     transparent 0, transparent
//   );
// }
::v-deep .el-input {
  margin-top: 8px;
}
::v-deep .el-input__inner {
  height: 40px;
}
a,
a:focus,
a:hover {
  cursor: pointer;
  color: #1890ff;
  text-decoration: none;
}

.show-pwd {
    position: absolute;
    right: 10px;
    top: 10px;
    font-size: 16px;
    color: rgb(86, 86, 86);
    cursor: pointer;
    user-select: none;
  }
</style>
