<!-- 請不要修改下面內容 -->
<template>
  <div id="app" class="container">
    <div class="total-price mt-3">
      <h4>總價：{{ totalPrice }} 元</h4>
    </div>
    
    <div class="row">
      <template v-for="(fruit, index) in fruitData" :key="index">
        <div class="col-md-6 mb-3">
          <div class="card" style="width: 100%;">
            <img :src="fruit.imgUrl" class="card-img-top w-25" :alt="fruit.title">
            <div class="card-body">
              <h5 class="card-title">水果名稱：{{ fruit.title }}</h5>
              <p class="card-text my-2">價錢：{{ fruit.price }}</p>
              <p class="card-text mb-3">數量：{{ fruit.count }}</p>
            </div>
          </div>
        </div>
      </template>
    </div>
  </div>
</template>
<!-- -->

<script setup>
import { ref, computed } from "vue";

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
