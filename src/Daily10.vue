<template>
  <div id="app" class="container">
    <h1>原始數量: {{ count }}</h1>
    <h1>三倍數量: {{ tripleCount }} (computed)</h1> 
    <h1>三倍數量: {{ calculateTripleCount() }} (method)</h1>
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const count = ref(2);

const tripleCount = computed(() => {
  return count.value * 3;
});

function calculateTripleCount() {
  return count.value * 3;
}
</script>