<template>
  <div v-if="injectMsg" class="alert alert-success">
    Hi, {{ injectMsg }}
    <button type="button" @click="injectMethod">Clear</button>
  </div>
  
  <hr />

  <button @click="handleAddUser">
    新增使用者
  </button>
  <div v-for="user in injectUsers" :key="user.id" class="alert">
    {{ user.id }} - {{ user.name }}
    <button @click="handleRemoveUser(user.id)" class="btn btn-sm btn-danger ms-2">
      刪除
    </button>
  </div>

</template>

<script setup>
import { inject } from 'vue';

// 定義注入的變數或方法，使用 key 取出 (提供預設值，避免注入失敗)
const injectMsg = inject('msg', '')
const injectMethod = inject('clearMsg', () => { })
const injectUsers = inject('obj', [])
const addUser = inject('addUser', () => { })
const removeUser = inject('removeUser', () => { })

// 子組件也可以呼叫注入的方法
const handleAddUser = () => {
  addUser()
}

const handleRemoveUser = (id) => {
  removeUser(id)
}
</script>