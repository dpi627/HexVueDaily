<template>
  <div id="app">
    <div class="container mt-2">
      <h2>登入</h2>
      <form>
        <div class="form-group">
          <label for="exampleInputEmail2">Email</label>
          <input v-model="emailSignIn" type="email" class="form-control" id="exampleInputEmail2" aria-describedby="emailHelp" placeholder="請輸入信箱">
        </div>
        <div class="form-group my-3">
          <label for="exampleInputPassword2">Password</label>
          <input v-model="passwordSignIn" type="password" class="form-control" id="exampleInputPassword2" placeholder="請輸入密碼">
        </div>
        <button @click="signIn" type="button" class="btn btn-success">登入</button>
      </form>
      <template v-if="responseMessage">
        <p class="h3" :class="{'mt-3': true, 'text-danger': isErrorMessage, 'text-success': !isErrorMessage}"> {{ responseMessage }}</p>        
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const api = 'https://todolist-api.hexschool.io';

const emailSignIn = ref('');
const passwordSignIn = ref('');
  
const responseMessage = ref('');
const isErrorMessage = ref(false);
const token = ref('');

const signIn = async () => {

};
</script>
