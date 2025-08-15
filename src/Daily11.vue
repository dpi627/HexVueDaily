<template>
  <label for="email">Email：</label>
  <input id="email" v-model="email" />
  <span v-if="emailError">{{ emailError }}</span>
</template>

<script setup>
import { ref, watch } from 'vue';

const email = ref('');
const emailError = ref('');

watch(email, (newVal) => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(newVal)) {
    emailError.value = 'email 格式不正確';
  } else {
    emailError.value = '';
  }
});
</script>

<style>
span {
  color: red;
  font-size: 0.8em;
}
</style>