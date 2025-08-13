<template>
  <div>
    <p>Count: {{ count }}</p>
    <Counter :initialCount="count" @update-count="handleUpdate" />
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Counter from './Child.vue';

const count = ref(0);

const handleUpdate = (newValue) => {
  count.value = newValue;
};
</script>