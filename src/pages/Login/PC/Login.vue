<template>
  <div class="login-container">
    <el-input 
      class="login-input"
      :placeholder=placeholderText
      v-model="userID"
      @input="handleInputUserID"
      @keyup.enter="handleLogin"
    />
    <p class="login-tip"> {{ t('userID Limit') }}</p>

    <el-input
      class="login-input"
      v-model="SDKAppID"
      :placeholder="placeholderSDKAppIdText"
      @input="handleInputSDKAppId"
      @keyup.enter="handleLogin"
    />
    <el-input
      class="login-input"
      v-model="SDKSecretKey"
      :placeholder="placeholderSDKSecretKeyText"
      @input="handleInputSDKSecretKey"
      @keyup.enter="handleLogin"
    />
    <el-alert class="tips" type="warning">
      The SDKAppID and SDKSecretKey can be found in the<a href="https://console.trtc.io/overview" target="_blank">TRTC console↗</a>. Please refer to the <a href="https://trtc.io/document/35166?platform=web&amp;product=conference#.E5.AE.A2.E6.88.B7.E7.AB.AF.E7.A4.BA.E4.BE.8B.E4.BB.A3.E7.A0.81.E8.AE.A1.E7.AE.97-UserSig">document</a> for how to obtain it.
    </el-alert>

    <Button 
      color="#1C66E5" 
      :width="200"
      :height="40"
      class="login-btn"
      @click="handleLogin"
    > 
      {{ t('Create / Log in userID') }}
    </Button>
  </div>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'
import { useLanguage } from '../../../hooks/index';
import useLogin from '../useLogin';
import { trim } from '../../../utils';
import Button from '../../../components/common/Button/Button.vue';

const userID = ref('');
const SDKAppID = ref('');
const SDKSecretKey = ref('');
const { t } = useLanguage();
const { login } = useLogin();
const placeholderText = computed(() => {
  return t('Please enter the userID to create or log in');
})

const placeholderSDKAppIdText = computed(() => {
  return t('Please enter the SDKAppId from the console');
})

const placeholderSDKSecretKeyText = computed(() => {
  return t('Please enter the SDKSecretKey from the console');
})

const handleLogin = async () => {
  sessionStorage.setItem('SDKAppID', SDKAppID.value);
  sessionStorage.setItem('SDKSecretKey', SDKSecretKey.value);
  await login(userID);
}

const handleInputUserID = () => {
  userID.value = trim(userID.value);
}

const handleInputSDKAppId = () => {
  SDKAppID.value = trim(SDKAppID.value);
}

const handleInputSDKSecretKey = () => {
  SDKSecretKey.value = trim(SDKSecretKey.value);
}
</script>

<style lang="scss" scoped>
.login-container {
  margin: 40px 140px;
  display: flex;
  flex-direction: column;
  
  .tips {
    margin-top: 10px;
    line-height: 1.2rem;
  }

  .login-input {
    width: 400px;
    height: 48px;
    padding: 12px 20px;
    margin-top: 18px;

    display: flex;
    flex-direction: row;
    align-items: center;

    background: #F9FAFC;
    border: 1px solid #E7ECF6;
    border-radius: 8px;
  }
  .login-tip {
    height: 24px;
    margin-top: 5px;
    margin-left: 12px;
    font-weight: 400;
    font-size: 12px;
    line-height: 24px;
    color: #99A2B2;
  }
  .login-btn {
    margin: 0 auto;
    margin-top: 24px;
  }
}

</style>