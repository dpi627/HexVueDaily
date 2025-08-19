<template>
  <h1>{{ data }}</h1>
  <h2>{{ value }}</h2>
</template>

<script setup>
// import { defineProps } from 'vue';

defineProps(['data', 'value'])
</script>