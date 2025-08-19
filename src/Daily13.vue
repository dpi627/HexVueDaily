<template>
  <Child :data="data" :value="2" :users="list" />
</template>

<script setup>
import { ref } from 'vue'
import Child from './components/Child.vue';

const data = ref('Hello')

const list = ref([
  { "id": 1, "name": "Andy" },
  { "id": 2, "name": "Bill" },
])
</script>