<template>
  <div class="page-user-chat">
    <van-nav-bar left-arrow @click-left="$router.back()" title="登 录"></van-nav-bar>
    <van-cell-group>
      <van-field @blur="validMobile" :error-message="errMsg.mobile" v-model.trim="formData.mobile" label="手机号" placeholder="请输入手机号" />
      <van-field @blur="validCode" :error-message="errMsg.code" v-model.trim="formData.code" label="验证码" placeholder="请输入验证码">
      <van-button slot="button" size="small" type="primary">发送验证码</van-button>
      </van-field>
    </van-cell-group>
    <div class="btn_box">
        <van-button type="info" @click="login" block round>登 录</van-button>
    </div>
  </div>
</template>

<script>
import { login } from '@/api/user'
import { mapMutations } from 'vuex'
export default {
  name: 'user-chat',
  data () {
    return {
      // 表单数据
      formData: {
        mobile: '',
        code: ''
      },
      // 错误提示
      errMsg: {
        mobile: '',
        code: ''
      }
    }
  },
  methods: {
    validMobile () {
      // 1. 非空
      if (this.formData.mobile === '') {
        this.errMsg.mobile = '请输入手机号'
        return false
      }
      // 2. 格式
      if (!/^1[3-9]\d{9}$/.test(this.formData.mobile)) {
        this.errMsg.mobile = '请输入正确手机号'
        return false
      }
      // 3. 正确
      this.errMsg.mobile = ''
    },
    validCode () {
      // 1. 非空
      if (this.formData.code === '') {
        this.errMsg.code = '请输入验证码'
        return false
      }
      // 2. 格式
      if (!/^\d{6}$/.test(this.formData.code)) {
        this.errMsg.code = '请输入正确验证码'
        return false
      }
      // 3. 正确
      this.errMsg.code = ''
    },
    async login () {
      // 对整体表单进行校验
      this.validMobile()
      this.validCode()
      // 判断errMsg对象中是否有错误信息   校验失败
      // 判断errMsg对象中是否有错误信息   校验失败
      if (this.errMsg.mobile || this.errMsg.code) {
        return false
      }
      // 如果校验成功进行登录
      try {
        // 基于request.js在api目录下封装一个调用接口的函数，当组件导入该函数使用即可。
        const { data } = await login(this.formData)
        // 登录成功
        // 1. 更新 vuex 和 本地存储 用户信息
        this.setUser(data)
        // 2. 跳转（如果地址栏有回跳地址哪就回跳，如果没有跳转到个人中心）
        const { redirectUrl } = this.$route.query
        // || && 短路或 短路与
        this.$router.push(redirectUrl || '/user')
      } catch (e) {
        // 登录失败
        this.$toast.fail('手机号或验证码错误')
      }
    },
    ...mapMutations(['setUser'])
  }
}
</script>

<style scoped lang='less'>
.p5{
  padding: 0 5px;
}
.btn_box{
  padding: 10px;
  .van-button{
    height: 32px;
    line-height: 30px;
  }
}
</style>
