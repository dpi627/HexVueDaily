<template>
  <input type="text" v-model="msg" placeholder="enter you name"></input>
  <Child16 />
</template>

<script setup>
import { reactive, provide, ref } from 'vue'
import Child16 from './components/Child16.vue';

const msg = ref('')
const obj = reactive([])

const resetTextbox = () => { 
  msg.value = ''
}

const addUser = () => {
  obj.push({ id: new Date().getTime(), name: msg.value || 'User' })
}

const removeUser = (id) => {
  const index = obj.findIndex(user => user.id === id)
  if (index > -1) {
    obj.splice(index, 1)
  }
}

// 宣告要提供的變數或方法，設定一個名稱當作 key
provide('msg', msg)
provide('clearMsg', resetTextbox)
provide('obj', obj)
provide('addUser', addUser)
provide('removeUser', removeUser)
</script>