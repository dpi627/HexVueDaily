<!-- 請不要修改下面內容 -->
<template>
  <div id="app" class="container">
    <div class="row">
      <template v-for="(fruit, index) in fruitData" :key="index">
        <div class="col-md-6 mb-3">
          <div class="card" style="width: 100%;">
            <img :src="fruit.imgUrl" class="card-img-top w-25" :alt="fruit.title">
            <div class="card-body">
              <h5 class="card-title">水果名稱：{{ fruit.title }}</h5>
              <p class="card-text my-2">價錢：{{ fruit.price }}</p>
              <p class="card-text mb-3">數量：{{ fruit.count }}</p>
              <a href="#" class="btn btn-primary" @click="addProduct(fruit)">加入購物車</a>
            </div>
          </div>
        </div>
      </template>
    </div>

    <div class="toast-container position-fixed top-0 end-0 p-3">
      <div v-for="(toast, index) in toasts" :key="index" class="toast show" role="alert" aria-live="assertive" aria-atomic="true">
        <div class="toast-body">
          {{ toast.message }}
        </div>
      </div>
    </div>
  </div>
</template>
<!-- -->

<script setup>
import { ref } from "vue";

const toasts = ref([]);

const addProduct = (fruit) => {
  if (fruit.count > 0) {
    fruit.count--;
    toasts.value.push({ message: `您已將 ${fruit.title} 加入購物車` });
    setTimeout(() => {
      toasts.value.shift();
    }, 3000);
  } else {
    toasts.value.push({ message: `${fruit.title} 已完售` });
    setTimeout(() => {
      toasts.value.shift();
    }, 3000);
  }
};
  
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
    imgUrl: 'https://i.imgur.com/FIMmh6h.png',
  },
  {
    title: 'kiwi',
    price: 55,
    count: 20,
    imgUrl: 'https://i.imgur.com/TIA6v4m.jpeg'
  }
]);
</script>
