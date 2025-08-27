<template>
  <div id="app">
    <div class="container mt-2">
      <h2>註冊</h2>
      <form>
        <div class="form-group">
          <label for="exampleInputEmail1">Email</label>
          <input v-model="emailSignUp" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="請輸入信箱">
        </div>
        <div class="form-group my-3">
          <label for="exampleInputPassword1">Password</label>
          <input v-model="passwordSignUp" type="password" class="form-control" id="exampleInputPassword1" placeholder="請輸入密碼">
        </div>
        <div class="form-group my-3">
          <label for="exampleInputNickName">Nick Name</label>
          <input v-model="nickname" type="text" class="form-control" id="exampleInputNickName" placeholder="請輸入暱稱">
        </div>
        <button @click="signUp" type="button" class="btn btn-primary">註冊</button>
      </form>
      <template v-if="messageSignUp">
        <p :class="{'mt-2': true, 'text-danger': isErrorSignUp, 'text-success': !isErrorSignUp}"> {{ messageSignUp }}</p>       
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from 'axios'


const api = 'https://todolist-api.hexschool.io';

const emailSignUp = ref('');
const passwordSignUp = ref('');
const nickname = ref('');
const messageSignUp = ref('');
const isErrorSignUp = ref(false);

const signUp = async () => {
  try {
    const response = await axios.post(`${api}/users/sign_up`, {
      email: emailSignUp.value,
      password: passwordSignUp.value,
      nickname: nickname.value,
    });
    messageSignUp.value = '註冊成功. UID: ' + response.data.uid;
    isErrorSignUp.value = false;
  } catch (error) {
    messageSignUp.value = '註冊失敗: ' + error.response.data.message;
    isErrorSignUp.value = true;
  }
};
</script>