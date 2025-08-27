<template>
  <div id="app">
    <div class="container mt-2">
      <h2>登入</h2>
      <form>
        <div class="form-group">
          <label for="exampleInputEmail1">Email</label>
          <input v-model="emailSignIn" type="email" class="form-control" id="exampleInputEmail1"
            aria-describedby="emailHelp" placeholder="請輸入信箱">
        </div>
        <div class="form-group my-3">
          <label for="exampleInputPassword1">Password</label>
          <input v-model="passwordSignIn" type="password" class="form-control" id="exampleInputPassword1"
            placeholder="請輸入密碼">
        </div>
        <button @click="signIn" type="button" class="btn btn-primary">登入</button>
      </form>
      <template v-if="messageSignIn">
        <p :class="{ 'mt-2': true, 'text-danger': isErrorSignIn, 'text-success': !isErrorSignIn }"> {{ messageSignIn }}
        </p>
      </template>
      <hr />
      <form>
        <h2>新增資料</h2>
        <div class="form-group">
          <input v-model="newTodo" type="text" class="form-control" id="exampleFormControlInput2" placeholder="請輸入內容" />
        </div>
        <button @click="addTodo" type="button" class="btn btn-primary mt-3">送出</button>
      </form>
      <hr />
      <h2>取得資料</h2>
      <template v-if="todos">
        <ul>
          <li v-for="(item, index) in todos" :key="item.id">
            {{ item.content }}
          </li>
        </ul>
      </template>
    </div>
  </div>
</template>

<script setup>
import axios from "axios";
import { ref } from "vue";

const api = 'https://todolist-api.hexschool.io';

const emailSignIn = ref('');
const passwordSignIn = ref('');
const messageSignIn = ref('');
const isErrorSignIn = ref(false);
const token = ref('');

const todos = ref([]);
const newTodo = ref('');

const signIn = async () => {
  try {
    const response = await axios.post(`${api}/users/sign_in`, {
      email: emailSignIn.value,
      password: passwordSignIn.value,
    });
    messageSignIn.value = '登入成功';
    token.value = response.data.token;
    isErrorSignIn.value = false;
    getTodos();
  } catch (error) {
    messageSignIn.value = '登入失敗: ' + error.response.data.message;
    isErrorSignIn.value = true;
  }
};

const addTodo = async () => {
  await axios.post(`${api}/todos`, {
    content: newTodo.value + ` #${new Date().getTime()}`
  }, {
    headers: {
      Authorization: token.value,
    },
  })
    .then(res => {
      console.log(res.data)
      todos.value.push(res.data.newTodo)
      newTodo.value = ''
    })
    .catch(error => {
      console.log(error)
      messageSignIn.value = error.response.data.message;
      isErrorSignIn.value = !error.response.data.status;
    })
};

const getTodos = async () => {
  await axios.get(`${api}/todos`, {
    headers: {
      Authorization: token.value,
    },
  })
    .then(res => {
      // console.log(res.data)
      todos.value = res.data.data
    })
};
</script>