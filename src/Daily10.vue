<template>
  <div id="app" class="container">
    <h1>原始數量: {{ count }}</h1>
    <h1>三倍數量: {{ tripleCount }} (computed)</h1>
    修改三倍數量: <input v-model="tripleCount" /> (反向計算)
  </div>
</template>

<script setup>
import { ref, computed } from "vue";

const count = ref(2);

const tripleCount = computed({
  get() {
    return count.value * 3;
  },
  set(newValue) {
    count.value = newValue / 3; // 反向計算
  }
});
</script>