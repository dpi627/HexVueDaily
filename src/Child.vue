<template>
  <div>
    <button @click="increment">+1</button>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const props = defineProps({
  initialCount: {
    type: Number,
    default: 0
  }
});

const emit = defineEmits(['update-count']);

const localCount = ref(props.initialCount);

const increment = () => {
  localCount.value++;
  emit('update-count', localCount.value);
};
</script>