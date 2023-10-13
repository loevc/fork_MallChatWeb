<script setup lang="ts">
import { computed, watchEffect, ref, reactive } from 'vue'
import { useWsLoginStore, LoginStatus } from '@/stores/ws'
// import QrCode from 'qrcode.vue'
import { ElForm, ElFormItem, ElButton } from 'element-plus'
const loginStore = useWsLoginStore()
const visible = computed({
  get() {
    return loginStore.showLogin
  },
  set(value) {
    loginStore.showLogin = value
  },
})

// const loginQrCode = computed(() => loginStore.loginQrCode)
const loginStatus = computed(() => loginStore.loginStatus)
const sendCodeForm = ref({
  email: '',
  verificationCode: '',
})
watchEffect(() => {
  // 打开窗口了 而且 二维码没获取，而且非登录就去获取二维码
  // if (visible.value && !loginQrCode.value) {
  //   // 获取登录二维码
  //   loginStore.getLoginQrCode()
  // }
})
const rules = reactive({
  email: [
    { required: true, message: '请输入电子邮件地址', trigger: 'blur' },
    { pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/, message: '请输入有效的电子邮件地址', trigger: 'blur' },
  ],
  verificationCode: [],
})
const sendVerificationCode = () => {}
const submitForm = () => {}
</script>

<template>
  <ElDialog class="login-box-modal" :width="376" v-model="visible" center>
    <div class="login-box">
      <img class="login-logo" src="@/assets/logo.jpeg" alt="MallChat" />
      <p class="login-slogan">边聊边买，岂不快哉~</p>
      <div>
        <ElForm
          ref="form"
          :model="sendCodeForm"
          :rules="rules"
          label-width="120px"
          label-position="left"
        >
          <ElFormItem class="formItem" label="邮箱" prop="email">
            <el-input v-model="sendCodeForm.email" />
          </ElFormItem>
          <ElFormItem class="formItem" label="验证码" prop="verificationCode">
            <div style="display: flex; align-items: center">
              <el-input v-model="sendCodeForm.verificationCode" />
              <el-button style="margin-left: 10px" @click="sendVerificationCode"
                >发送验证码</el-button
              >
            </div>
          </ElFormItem>
        </ElForm>
        <ElButton style="width: 100%" type="primary" @click="submitForm">登录</ElButton>
      </div>
      <!-- <div class="login-qrcode-wrapper" v-loading="!loginQrCode">
        <QrCode
          class="login-qrcode"
          v-if="loginQrCode"
          :value="loginQrCode"
          :size="328"
          :margin="5"
        />
      </div> -->

      <p class="login-desc" v-if="loginStatus === LoginStatus.Waiting">
        <ElIcon :size="32" class="login-desc-icon" color="var(--color-wechat)"
          ><IEpSuccessFilled
        /></ElIcon>
        扫码成功~，点击“登录”继续登录
      </p>
      <p class="login-desc" v-if="loginStatus === LoginStatus.Init">
        使用「<strong class="login-desc-bold">微信</strong>」扫描二维码登录~~
      </p>
    </div>
  </ElDialog>
</template>

<style lang="scss" src="./styles.scss" scoped />
