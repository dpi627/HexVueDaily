<template>
  <h1>{{ data }}</h1>
  <h2>{{ value }}</h2>
  <div v-for="user in users" :key="user.id">
    {{ user.id }} - {{ user.name }}
    <button @click.prevent="greeting(user)" type="button">Say Hello</button>
  </div>
  <h3>{{ innerData }}</h3>
</template>

<script setup>
import { defineProps } from 'vue';
import { defineEmits } from 'vue';

defineProps(['data', 'value', 'users', 'innerData'])
const emits = defineEmits(['say-hello-to'])

const greeting = (user) => {
  emits('say-hello-to', user)
}
</script>