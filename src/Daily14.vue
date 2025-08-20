<template>
  <Child 
  :users="list" @say-hello-to="greetingUser" />
</template>

<script setup>
import { ref } from 'vue'
import Child from './components/Child.vue';

const greetingUser = (user) => {
  alert(`Hi, ${user.name}`)
}

const list = ref([
  { "id": 1, "name": "Andy" },
  { "id": 2, "name": "Bill" },
])
</script>