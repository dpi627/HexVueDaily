<template>
  <div>
    <h2>Posts</h2>
    <div v-if="loading">載入中...</div>
    <div v-else>
      <div v-for="post in posts" :key="post.id" class="post-item">
        <h3>{{ post.title }}</h3>
        <p>{{ post.body }}</p>
        <small>Post ID: {{ post.id }} | User ID: {{ post.userId }}</small>
      </div>
    </div>
  </div>
</template>

<script setup>
import axios from 'axios'
import { ref, onMounted } from 'vue'

const posts = ref([])
const loading = ref(true)
const api = 'https://jsonplaceholder.typicode.com/posts';

onMounted(async () => {
  try {
    const res = await axios.get(api)
    
    console.log(res);
    posts.value = res.data // 將 API 資料存入響應式變數
    
    console.log(res.data) // 伺服器的回應內容
    console.log(res.status) // 伺服器的回應狀態碼
    console.log(res.statusText) // 伺服器回應的狀態訊息
    console.log(res.headers) // 伺服器的 response headers
    console.log(res.config) // 我們設定的請求配置
    console.log(res.request) // 原生請求物件
  } catch (error) {
    console.error('Error:', error);
  } finally {
    loading.value = false
  }
})
</script>

<style scoped>
.post-item {
  border: 1px solid #ccc;
  padding: 15px;
  margin: 10px 0;
  border-radius: 5px;
}

.post-item h3 {
  margin-top: 0;
  color: #333;
}

.post-item p {
  color: #666;
  line-height: 1.5;
}

.post-item small {
  color: #999;
}
</style>