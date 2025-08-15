<template>
  <div class="mb-2">
    <label for="username">姓名：</label>
    <input id="username" v-model="user.username" />
    <span v-if="errors.username">{{ errors.username }}</span>
  </div>
  <div class="mb-2">
    <label for="phone">電話：</label>
    <input id="phone" v-model="user.phone" />
    <span v-if="errors.phone">{{ errors.phone }}</span>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const user = ref({
  username: '',
  phone: ''
});

const errors = ref({
  username: '',
  phone: ''
});

watch(user, (newVal) => {
  if (newVal.username.length < 3) {
    errors.value.username = '需要正確的輸入名稱';
  } else {
    errors.value.username = '';
  }

  const phonePattern = /^[0-9]{10}$/;
  if (!phonePattern.test(newVal.phone)) {
    errors.value.phone = '需要正確的輸入電話號碼';
  } else {
    errors.value.phone = '';
  }
}, { deep: true });
</script>