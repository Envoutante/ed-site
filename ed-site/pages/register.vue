<template>
  <!--注册-->
  <div class="wrap">
    <div v-if="step === 1" class="tdbModule register">
      <div class="registerTitle">注册账户</div>
      <div class="registerLc1">
        <p class="p1">填写账户信息</p>
        <p class="p2">注册成功</p>
      </div>

      <div class="registerCont">
        <ul>
          <li>
            <span class="dis"></span>
            <el-radio v-model="userInfo.userType" label="1">我要投资</el-radio>
            <el-radio v-model="userInfo.userType" label="2">我要借钱</el-radio>
          </li>
          <li class="telNumber">
            <span class="dis">手机号码</span>
            <input
              class="input"
              v-model="userInfo.mobile"
              placeholder="请输入手机号码"
            />
            <button v-if="!sending" class="button" @click="send()">
              获取验证码
            </button>
            <button v-else disabled class="button disabled">
              {{ leftSecond }}秒后重发
            </button>
          </li>

          <li>
            <span class="dis">短信验证码</span>
            <input
              class="input"
              v-model="userInfo.code"
              placeholder="请输入验证码"
            />
          </li>

          <li>
            <span class="dis">密码</span>
            <input
              type="password"
              v-model="userInfo.password"
              class="input"
              placeholder="请输入密码"
              @blur="check()"
            />
            <span class="info">
              6-16位数字、字母或常用符号，字母区分大小写
            </span>
          </li>

          <li>
            <span class="dis">确认密码</span>
            <input
              type="password"
              v-model="userInfo.passwordAssure"
              class="input"
              placeholder="请确认密码"
              @blur="assure()"
            />
          </li>

          <li class="agree">
            <input type="checkbox" checked />
            我同意《<NuxtLink to="#" target="_black">易贷注册协议</NuxtLink>》
            <span>请查看协议</span>
          </li>
          <li class="btn">
            <button @click="register()">
              下一步
            </button>
          </li>
        </ul>
      </div>
    </div>

    <div v-if="step === 2" class="tdbModule register">
      <div class="registerTitle">注册账户</div>
      <div class="registerLc2">
        <p class="p1">填写账户信息</p>
        <p class="p2">注册成功</p>
      </div>
      <div class="registerCont">
        <ul>
          <li class="scses">
            {{ this.userInfo.mobile }} 恭喜您注册成功！
            <NuxtLink class="blue" to="/login">
              请登录
            </NuxtLink>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import '~/assets/css/register.css'
export default {
  data() {
    return {
      step: 1, // 注册步骤
      userInfo: {
        userType: 1,
        mobile: null,
      },
      sending: false, // 是否发送验证码
      second: 10, // 倒计时间
      leftSecond: 0, // 剩余时间
    }
  },

  methods: {
    // 发短信
    send() {
      // 1. 判读手机号是否为空
      if (!this.userInfo.mobile) {
        this.$message.error('请输入手机号码')
        return
      }

      // 2. 若已经提交了，就禁止再次提交
      if (this.sending) return
      // 2.1 未提交，就发请求
      this.sending = true
      // 2.2 开启倒计时
      this.timeDown()

      // 2.3 获取验证码
      this.$axios.$get('/api/sms/send/' + this.userInfo.mobile).then((res) => {
        this.$message.success(res.message)
      })
    },

    // 倒计时
    timeDown() {
      console.log('进入倒计时')

      this.leftSecond = this.second
      const timer = setInterval(() => {
        this.leftSecond--
        if (this.leftSecond <= 0) {
          // 停止计时
          clearInterval(timer)
          this.sending = false
        }
      }, 1000)
    },

    // 注册
    register() {
      this.$axios
        .$post('/api/core/userInfo/register', this.userInfo)
        .then((res) => {
          this.step = 2
          this.$message.success(res.message)
        })
    },

    // 密码正则校验
    check() {
      if (this.userInfo.password != null) {
        var length = /^.{6,16}$/ // 判断是否长度为6-16位
        var test = length.test(this.userInfo.password)
        if (test == false) {
          this.$message.error('密码格式错误')
        }
      }
    },

    // 确认密码
    assure() {
      if (this.userInfo.password != this.userInfo.passwordAssure) {
        this.$message.error('两次输入的密码不一致')
      }
    },
  },
}
</script>
