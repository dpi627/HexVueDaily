<template>
  <div id="app" class="container py-4">
    <div class="row">
      <div class="col-md-6 mb-3" v-for="(fruit, index) in fruitData" :key="index">
        <div class="card h-100">
          <img :src="fruit.imgUrl" class="card-img-top" :alt="fruit.title" style="width: 25%; margin: 16px auto" />
          <div class="card-body">
            <h5 class="card-title">水果名稱：{{ fruit.title }}</h5>
            <p class="card-text">價錢：{{ fruit.price }}</p>
            <p class="card-text">數量：{{ fruit.count }}</p>
            <button class="btn btn-primary" @click="addToCart(fruit.title)">
              加入購物車
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="position-fixed top-0 end-0 p-3" style="z-index: 1050">
      <div v-if="state.message" class="toast show align-items-center text-white bg-success border-0">
        <div class="d-flex">
          <div class="toast-body">{{ state.message }}</div>
          <button type="button" class="btn-close btn-close-white me-2 m-auto" @click="state.message = ''"></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref } from 'vue'

const state = reactive({
  message: ''
})

const fruitData = ref([
  {
    title: 'apple',
    price: 25,
    count: 50,
    imgUrl: 'https://i.imgur.com/w4sYWlS.jpeg'
  },
  {
    title: 'orange',
    price: 15,
    count: 20,
    imgUrl: 'https://i.imgur.com/PSmzmXi.jpeg'
  },
  {
    title: 'strawberry',
    price: 45,
    count: 10,
    imgUrl: 'https://i.imgur.com/FIMmh6h.png'
  },
  {
    title: 'kiwi',
    price: 55,
    count: 20,
    imgUrl: 'https://i.imgur.com/TIA6v4m.jpeg'
  }
])

// 請完成 addToCart 的內容
const addToCart = (name) => {
  state.message = `${name}`
  setTimeout(() => {
    state.message = ''
  }, 3000)

}

</script>

<style scoped>
.container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 16px;
}
</style>