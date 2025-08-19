<template>
  <Child :data="data" />
</template>

<script setup>
import { ref } from 'vue'
import Child from './components/Child.vue';

const data = ref('Hello')
</script>