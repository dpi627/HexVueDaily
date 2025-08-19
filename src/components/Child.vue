<template>
  <h1>{{ data }}</h1>
  <h2>{{ value }}</h2>
  <div v-for="user in users" :key="user.id">
    {{ user.id }} - {{ user.name }}
  </div>
</template>

<script setup>
import { defineProps } from 'vue';

defineProps(['data', 'value', 'users'])
</script>