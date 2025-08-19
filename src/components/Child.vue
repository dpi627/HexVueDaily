<template>
  <h1>{{ data }}</h1>
  <h2>{{ value }}</h2>
  <div v-for="user in users" :key="user.id">
    {{ user.id }} - {{ user.name }}
  </div>
  <h3>{{ innerData }}</h3>
</template>

<script setup>
import { defineProps } from 'vue';

defineProps(['data', 'value', 'users', 'innerData'])
</script>