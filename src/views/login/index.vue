<template>
  <div class="login-container">
    <div class="login-form-wrap">
      <div class="login-head">
        <div class="logo"></div>
      </div>
      <el-form
        class="login-form"
        ref="login-form"
        :model="user"
        :rules="formRules"
      >
        <el-form-item prop="mobile">
          <el-input v-model="user.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-input v-model="user.code" placeholder="请输入验证码"></el-input>
        </el-form-item>
        <el-form-item prop="agree">
          <el-checkbox v-model="user.agree"
            >我已阅读并同意协议和隐私条款</el-checkbox
          >
        </el-form-item>
        <el-form-item>
          <el-button
            class="login-btn"
            type="primary"
            @click="onLogin"
            :loading="loginLoading"
            >登录</el-button
          >
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
import { login } from '@/api/user'
export default {
  name: 'LoginIndex',
  components: {},
  props: {},
  data() {
    return {
      user: {
        mobile: '13911111111',
        code: '246810',
        agree: false
      },
      loginLoading: false,
      formRules: {
        mobile: [
          // trigger 用来配置触发校验的时机，有两个选项，change 是当输入的内容发送变化的时候，blur 当失去焦点的时候
          { required: true, message: '请输入手机号', trigger: 'blur' },
          {
            pattern: /^1[3|5|7|8|9]\d{9}$/,
            message: '请输入正确的号码格式',
            trigger: 'blur'
          }
        ],
        code: [
          { required: true, message: '验证码不能为空', trigger: 'blur' },
          {
            pattern: /^\d{6}$/,
            message: '请输入正确的验证码格式',
            trigger: 'blur'
          }
        ],
        agree: [
          {
            validator: (rule, value, callback) => {
              if (value) {
                callback()
              } else {
                callback(new Error('请同意用户协议'))
              }
            },
            trigger: 'blur'
          }
        ]
      }
    }
  },
  computed: {},
  watch: {},
  created() {},
  mounted() {},
  methods: {
    onLogin() {
      this.$refs['login-form'].validate(valid => {
        if (!valid) {
          return
        }
        this.login()
      })
    },
    login() {
      this.loginLoading = true
      login(this.user).then(res => {
        console.log(res)
        this.$message({
          message: '登录成功',
          type: 'success'
        })
        this.loginLoading = false
        // 跳转到首页
        this.$router.push('/')
      }).catch(err => {
        console.log(err)
        this.$message.error('登录失败，手机号或者验证码错误')
        this.loginLoading = false
      })
    }
  }
}
</script>

<style lang="less" scoped>
.login-container {
  position: fixed;
  left: 0;
  top: 0;
  bottom: 0;
  right: 0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: url('./login_bg.jpg') no-repeat;
  background-size: cover;
  .login-form-wrap {
    min-width: 300px;
    padding: 30px 50px 10px;
    background: #fff;
    .login-head {
      display: flex;
      justify-content: center;
      .logo {
        width: 200px;
        height: 57px;
        background: url('./logo_index.png') no-repeat;
        background-size: contain;
      }
    }
  }
  .login-form {
    .login-btn {
      width: 100%;
    }
  }
}
</style>
