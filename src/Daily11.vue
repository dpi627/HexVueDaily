<!-- 請不要修改下面內容 -->
<template>
  <div>
    <form>
      <div class="m-5">
        <label for="username">購買人姓名：</label>
        <input id="username" v-model="user.username" />
        <span v-if="errors.username">{{ errors.username }}</span>
      </div>
    </form>
  </div>
</template>
<!-- -->

<script setup>
import { ref, watch } from 'vue';

const user = ref({
  username: '',
});

const errors = ref({
  username: '',
});

// 監聽 user.username 的變化
watch(() => user, (newVal) => {
  if (newVal.value.username.length != 3) {
    errors.value.username = "需要輸入正確名稱"
  }
  else {
    errors.value.username = ""
  }
}, { deep: true, immediate: true });

</script>

<style>
span {
  color: red;
  font-size: 0.8em;
}
</style>
