![](https://img.shields.io/badge/Vite-ddd?logo=vite)
![](https://img.shields.io/badge/Vue-3-4FC08D?logo=vuedotjs)

# Day 1

https://hackmd.io/4_zPrubBTBO3wMGRuIZ2Tw

## v-text

```html
<div v-text="message"></div>
<div>{{ message }}</div>
```

## v-html

```html
<div v-html="htmlContent"></div>
```

## v-once

```html
<div v-once>{{ message }}</div>
```

## Summary

| command | description           |
| :-----: | --------------------- |
| v-text  | 純文字綁定，等同 `{{}}`       |
| v-html  | 渲染 HTML 內容，須注意 XSS 攻擊 |
| v-once  | 單向綁定，用於靜態內容           |

> [!WARNING]
> - XSS（Cross-Site Scripting）是一種攻擊方式，例如在網頁中注入 JavaScript
> - `v-html` 可渲染原始 HTML 標籤，也包含了 `<script>`，所以要小心

---

# Day 2

https://hackmd.io/wtgmXvXCSyaKOuMlst-l6A

## v-model

- 用於將表單元件與資料雙向綁定

```html
<template>
  <div id="app">
    <input v-model="message" />
    {{ message }}
  </div>
</template>

<script setup>
import { ref } from "vue";

const message = ref('這是一段話')
</script>
```

### .lazy

- 避免持續觸發，僅離開 onblur 才觸發

```html
<input v-model.lazy="message" />
```

### .number

- input 一律預設為文字，加上 `.number` 可設定為數字

```html
<input v-model.number="number" />
```

### .trim

```
<input type="text" v-model.trim.lazy="message" />
```

- 可移除輸入前後空白，可與其他修飾符混用

---

# Day 3

https://hackmd.io/p8Oebgk8Rb6gPBcKrTfhMw

- `v-model` 除了 `input`，也可用於 `select` `textarea` `checkbox`
- 不同元素的使用方式略有不同

## select

- `v-model` 綁定 `<option>` 中的 `value`

### static data

```html
<template>
  <select name="" v-model="selected">
    <option disabled value="">請選擇</option>
    <option value="小美">小美</option>
    <option value="可愛小妞">可愛小妞</option>
    <option value="漂亮阿姨">漂亮阿姨</option>
  </select>
  <p>小明喜歡的女生是 {{ selected }}。</p>
</template>
```

### dynamic data

```html
<template>
  <select name="" id="" class="form-control" v-model="selected">
    <option disabled value="">請選擇</option>
    <option :value="item" v-for="item in selectData">{{ item }}</option>
  </select>
  <p>小明喜歡的女生是 {{ selected }}。</p>
</template>

<script setup>
import { ref } from "vue";

const selectData = ref(['小美', '可愛小妞', '漂亮阿姨']);
const selected = ref('')
</script>
```

## checkbox

- `checkbox` 沒有設定 `value` 時綁定 `ture` `false`
- 多選項目透過 `v-model` 綁定到陣列

### single item

```html
<template>
  <input type="checkbox" id="chk1" v-model="checkbox1" />
  <label for="chk1">{{ checkbox1 }}</label>
</template>

<script setup>
import { ref } from "vue";

const checkbox1 = ref(false);
</script>
```

### multiple items

```html
<template>
  <input type="checkbox" id="check2" v-model="checkboxArray" value="雞">
  <label for="check2">雞</label>
  <input type="checkbox" id="check3" v-model="checkboxArray" value="豬">
  <label for="check3">豬</label>
  <input type="checkbox" id="check4" v-model="checkboxArray" value="牛">
  <label for="check4">牛</label>

  {{ checkboxArray }}
</template>

<script setup>
import { ref } from "vue";

const checkboxArray = ref([]);
</script>
```

## radio

```html
<template>
  <input type="radio" id="radio2" v-model="selectItem" value="雞">
  <label for="radio2">雞</label>
  <input type="radio" id="radio3" v-model="selectItem" value="豬">
  <label for="radio3">豬</label>
  <input type="radio" id="radio4" v-model="selectItem" value="牛">
  <label for="radio4">牛</label>

  您選擇了：{{ selectItem }}
</template>

<script setup>
import { ref } from "vue";

const selectItem = ref('');
</script>
```

---

# Day 4

https://hackmd.io/Z_FWfuh_R2Ss32bdlF0kYw

## v-for

- 針對陣列 `array` 或物件 `object` 進行渲染
- 使用 `(item, key) in array` 的語法：
	- `item` 代表單一元素的別名
	- `key` 如果是陣列為其索引，物件則為屬性
	- `array` 表示來源陣列或資料
- 陣列的索引為 `0, 1, 2...`，物件索引為屬性名稱

```html
<template>
  <div id="app" class="container">
    <ul>
      <li v-for="(item, key) in arrayData">
        {{ key }} - {{ item.name }} {{ item.age }} 歲
      </li>
      <br />
      <li v-for="(item, key) in objectData">
        {{ key }} - {{ item.name }} {{ item.age }} 歲
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from "vue";

const arrayData = ref(
  [
    {
      name: '卡斯伯',
      age: 35
    },
    {
      name: 'Ray',
      age: 28
    }
  ]
)

const objectData = ref(
  {
    casper: {
      name: '卡斯伯',
      age: 35
    },
    ray: {
      name: 'Ray',
      age: 32
    }
  }
)
</script>
```

### 套版範例

```html
<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-6 mb-3" v-for="(item, key) in fruitData">
        <div class="card" style="width: 100%;">
          <div class="card-body">
            <h5 class="card-title">水果名稱：{{ item.title }}</h5>
            <p class="card-text my-2">價錢：{{ item.price }}</p>
            <p class="card-text mb-3">數量：{{ item.count }}</p>
            <a href="#" class="btn btn-primary">加入購物車</a>
          </div>
        </div>
      </div>
    </div>
    <!--     <div>放入購物車的水果：</div> -->
  </div>
</template>

<script setup>
import { ref } from "vue";

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
  }
]);
</script>
```

### :key

```html
<!-- without :key -->
<li v-for="(item, key) in arrayData" >
  {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
</li>

<!-- with :key -->
<li v-for="(item, key) in arrayData" :key="item.age">
  {{ key }} - {{ item.name }} {{ item.age }} 歲 <input type="text">
</li>
```

- 有 `:key` 時，Vue 會在 Virtual DOM 追蹤每個 `<li>`，確保資料和 DOM 節點對應。 
- 但在生成的 HTML 上，**並不會多出什麼特殊屬性**
- `key` 只在 Vue 的內部運作，不會反映在最終的 HTML 標籤上
- `:key` 讓 Vue 能夠辨識每個 `<li>` 對應哪一筆資料
- 當資料順序改變（如反轉陣列），Vue 會根據 key 來「搬移」整個 DOM 節點
- 這樣 input 的內容（如你輸入的文字）就會跟著資料一起移動，不會錯位

> [!IMPORTANT]
> 新版的 Vue 相關開發工具中，都會強烈建議加上 `key`

## `<template v-for>`

```html
<template v-for="(item, key) in arrayData">
  <tr>
	<td>{{ key }}</td>
	<td>{{ item.name }}</td>
  </tr>
</template>
```

- 想產生多個元素又不想多包一層的時候可用
- 上述範例其實也相當於直接寫 `<tr v-for>`，因為其中只有一個 `<tr>`

```html
<tr v-for="(item, key) in arrayData">
  <td>{{ key }}</td>
  <td>{{ item.name }}</td>>
</tr>
```

- 如果是以下範例就可以看出 `<template v-for>` 效果

```html
<template v-for="(item, key) in arrayData">
  <tr>
    <td colspan="2">姓名</td>
    <td>{{ item.name }}</td>
  </tr>
  <tr>
    <td colspan="2">年齡</td>
    <td>{{ item.age }} 歲</td>
  </tr>
</template>
```

- `<tr v-for>` 每次只能產生一個 `<tr>`
- `<template v-for>` 每次可以產生多個元素（如多個 `<tr>`、`<td>`），更有彈性

## 避免與 v-if 混用

- [https://cn.vuejs.org/guide/essentials/list.html#v-for-with-v-if](https://cn.vuejs.org/guide/essentials/list.html#v-for-with-v-if)
- Vue.js 的規範中建議不要將 v-for 與 v-if 混合使用
- 搭配進階工具如 Vue Cli 及 ESLint 時，混用會錯誤，盡可能用不同標籤呈現

```html
<table class="table">
  <tbody>
	<template v-for="(item, key) in arrayData">
	  <tr v-if="item.age > 30">
		<td>{{ key }}</td>
		<td>{{ item.name }}</td>
	  </tr>
	</template>
  </tbody>
</table>
```

---

# Day 5

https://hackmd.io/N780g4MhSfuydposvY1X8g

## v-if, v-else

- `v-if` `v-else` 用於條件區塊渲染，當指令內容為 `true` 時產生 `DOM`
- `v-if` 會完整移除 DOM 元素，使其從 HTML 結構上消失
- 當使用此方法切換 Vue 元件時，元件的生命週期會重新計算

```html
<template>
  <div id="app">
    <div class="alert alert-success" v-if="isSuccess">啟用!</div>
    <div class="alert alert-danger" v-else>停用!</div>
    <div class="form-check">
      <input type="checkbox" class="form-check-input" id="isSuccess" v-model="isSuccess">
      <label class="form-check-label" for="isSuccess">啟用元素狀態</label>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const isSuccess = ref(true);
</script>
```

- `v-else` `v-else-if` 為 `v-if` 的延伸運用

```html
<template>
  <div id="app">
    <ul class="nav nav-tabs">
      <li class="nav-item">
        <a class="nav-link" href="#" :class="{'active': link === 'a'}" @click="link = 'a'">標題一</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#" :class="{'active': link === 'b'}" @click="link = 'b'">標題二</a>
      </li>
    </ul>
    <div class="content">
      <div v-if="link === 'a'">A</div>
      <div v-else-if="link === 'b'">B</div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const link = ref('a');
</script>
```

## v-show

- `v-show` 將物件加上 `display: none`，讓物件從視覺上不可見
- 以下為前述範例改用 `v-show`，運行結果相同，但元素會套用 `display: none` 而非移除

```html
<div class="alert alert-success" v-show="isSuccess">成功!</div>
<div class="alert alert-danger" v-show="!isSuccess">失敗!</div>
```

## 如何選擇

- 當元件生命週期需要重新計算，可以用 `v-if`，反之可用 `v-show`
- 當隱藏元件時需要完整移除 DOM 結構，可使用 `v-if`

| 指令       | 顯示時               | 隱藏時               | 說明            |
| -------- | ----------------- | ----------------- | ------------- |
| `v-if`   | 生成 `DOM`          | 移除 `DOM`          | 顯示會重新計算生命週期   |
| `v-show` | 移除 `display:none` | 套用 `display:none` | `DOM` 生成後持續存在 |

## 避免與 v-for 混用

- `v-if` 與 `v-for` 混用可能有衝突問題，前述 `v-for` 章節已提過
- Vue.js 的規範中建議不要將 `v-for` 與 `v-if` 混用，盡可能用不同標籤呈現

---

# Day 6

https://hackmd.io/KQem0FToQy2n4aLakQu2Xg

## v-on

https://zh-hk.vuejs.org/guide/essentials/event-handling.html

- `v-on` 是觸發器，`click` 是觸發的方法，`v-on` 可縮寫為 `:`
- HTML 中的 `v-on:click` 可以直接操作 data 內的資料或傳入方法
- 監聽網頁上的事件（如同純 JS 的 DOM 事件）

```html
<template>
    <button v-on:click="counter += 1">Count +</button>
    <p>Count： {{ counter }} </p>
    <button @click="greet('World')">Greet</button>
</template>

<script setup>
import { ref } from "vue";

const counter = ref(1);
const greet = (msg) => {
  alert(`Hello ${msg}!`)
}
</script>
```

## $event

- 使用特殊變數 `$event` 便可帶入原生 DOM 事件
- 方法內可使用原生事件 `event`

```html
<template>
  <button @click="greet('World', $event)">Greet</button>
</template>

<script setup>
const greet = (msg, event) => {
  alert(`Hello ${msg}! Tag: ${event.target.tagName}`)
}
</script>
```

## 修飾符

- 在處理事件時很常調用 `event.preventDefault()` 或 `event.stopPropagation()` 
- `v-on` 提供了 **事件修飾符**，可以不用在方法內調用

```html
<!-- 單擊事件將停止傳遞 -->
<a @click.stop="doThis"></a>

<!-- 提交事件將不再重新加載頁面 -->
<form @submit.prevent="onSubmit"></form>

<!-- 修飾語可以使用鏈式書寫 -->
<a @click.stop.prevent="doThat"></a>

<!-- 也可以只有修飾符 -->
<form @submit.prevent></form>

<!-- 僅當 event.target 是元素本身時才會觸發事件處理器 -->
<!-- 例如：事件處理器不來自子元素 -->
<div @click.self="doThat">...</div>
```

---

# Day 7

https://hackmd.io/ZKR644K6S8-Lpxuuzgftew

## v-bind

https://zh-hk.vuejs.org/api/built-in-directives.html#v-bind

- 動態的將資料綁定到 HTML Attribute，通常也會影響對應的 DOM Property
- 瀏覽器會將 HTML 解析成一個結構化模型 **DOM（Document Object Model）**
- 每個標籤都會被轉換成 **DOM** 的一個 **node**，或說是 **元素節點（Element Node）**

## 補充 Attribute vs Property
### Attribute 

- 靜態的，存在於 HTML 標籤的結構中，反映初始狀態。
- 可通過 `DOM` 方法如 `getAttribute()` 和 `setAttribute()` 存取或修改。

### Property

- `DOM` 的 JavaScript 屬性，存在於 `DOM` 的記憶體結構，反映元素執行時的動態狀態。
- 例如：`inputElement.value` 會隨著使用者輸入或 JavaScript 修改而改變。

> [!NOTE]
> `Property` 是動態的，與 JavaScript 物件的屬性類似，可以直接通過點 `.` 或 `[]` 存取

## 動態綁定

```html
  <div id="app">
    <input type="text" :value="text">
    <input type="text" :value="1 + 1">
    <br>
    <img :src="imgUrl" alt="">
  </div>
```
```js
import { ref } from "vue";

const text = ref('這是一段話')
const imgUrl = ref('https://images-url')
```

- 透過例如 `:value` `:src` 可綁定資料
- 也可以在其中運行 JavaScript 程式

## Class

```html
  <div id="app">
    <div class="box" :class="{rotate: isTransform}"></div>
    <button 
	    class="btn btn-outline-primary" 
	    v-on:click="isTransform = !isTransform">選轉物件</button>
  </div>
```
```js
import { ref } from "vue";

const isTransform = ref(false);
```
```html
<style>
.box {
  width: 100px;
  height: 100px;
  background-color: blue;
  transition: all 1s;
}

.box.rotate {
  transform: rotate(45deg);
  background-color: red;
}
</style>
```

- `:class` 可以動態切換 className
- `"{ rotate: isTransform }"` 物件中前者 `rotate` 為 className，後者為判斷式
- 透過動態設定 `isTransform` = `true` or `false` 來決定是否套用 `rotate`

```html
  <div id="app">
    <div class="box" :class="objectClass"></div>
    <hr>
    <button 
      type="button"
      class="btn btn-outline-primary" 
      @click="objectClass.rotate = !objectClass.rotate"
    >
      選轉物件
    </button>
    <div class="form-check">
      <input 
        type="checkbox" 
        id="classToggle2" 
        class="form-check-input" 
        v-model="objectClass['bg-danger']"
      >
      <label class="form-check-label" for="classToggle2">
        切換色彩
      </label>
    </div>
  </div>
```
```js
import { ref } from "vue";

const objectClass = ref({
  'rotate': false,
  'bg-danger': false,
},);
```

- 綁定的邏輯寫在資料結構內，例如 `objectClass` 定義了多個樣式(屬性)
- `button` 透過 `@clicke` 綁定物件屬性

```html
    <button class="btn" :class="[...arrayClass]">請操作本元件</button>
    <div class="form-check">
      <input type="checkbox" class="form-check-input" id="classToggle3" v-model="arrayClass" value="btn-outline-primary">
      <label class="form-check-label" for="classToggle3">切換樣式</label>
    </div>
    <div class="form-check">
      <input type="checkbox" class="form-check-input" id="classToggle4" v-model="arrayClass" value="active">
      <label class="form-check-label" for="classToggle4">啟用元素狀態</label>
    </div>
```
```js
import { ref } from "vue";

const arrayClass = ref([]);
```

- 綁定到一個陣列，所有針對這個陣列的操作都會疊加到物件上
- 透過勾選將樣式 `btn-outline-primary` 與 `active` 加入到陣列(或移除)

## Style

```html
<div class="box" 
	:style="{backgroundColor: styleObject.backgroundColor}"></div>
<div class="box" 
	:style="styleObject"></div>
<div class="box" 
	:style="[styleObject, styleObject2]"></div>
```
```js
import { ref } from "vue";

const styleObject = ref({
  backgroundColor: 'yellow',
  borderWidth: '3px',
  borderStyle: 'solid',
  borderColor: 'red',
});
  
const styleObject2 = ref({
  borderRadius: '20%',
  boxShadow: '3px 3px 5px rgba(0, 0, 0, 1)'
});
```
```html
<style>
.box {
  width: 100px;
  height: 100px;
  margin-bottom: 10px;
}
</style>
```

- `style` 可綁定物件特定屬性 (樣式名稱必須使用小駝峰)
- 也可綁定整個物件 (同樣注意屬性命名)，或整個屬性物件陣列

## 動態屬性 vs 靜態屬性

|             |   `:src`   |     `src`     |
| ----------: | :--------: | :-----------: |
| 傳入 Vue data |     ✔️     |       ❌       |
|        執行運算 | 可寫入 JS 表達式 |       ❌       |
|        具備型別 | `true` `1` | `"tru"` `"1"` |

---

# Day 8

https://hackmd.io/jOYApsKmSTSWqnPB7Gfogw

- 綜合練習 `v-for` `v-on` `v-bind` `v-model` `v-if`
- `<select>` 透過 `v-model` 綁定 `selected`
- `selected = ref('')`，第一個 `<option>` 設定 `value=''` 對應 `ref('')`
- 其他 `<option>` 綁定單一個水果物件 `:value="fruit"`

```html
<select v-model="selected">
  <option value="">請選擇水果...</option>
  <option v-for="fruit in fruitData" :key="fruit.title" :value="fruit">
	{{ fruit.title }}
  </option>
</select>

<div>
  透過 select 選擇的水果：
  <span v-if="selected">
	{{ selected.title }}
  </span>
  <span v-else>未選擇</span>
</div>
```
```js
import { ref } from "vue";

const selected = ref('');
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
  }
]);
```

- 如果後續只需顯示名稱，`<option>` 也可改為綁定 `:value="fruit.title"`
- 顯示 `selected` 部分則修改為

```html
  <span v-if="selected !== ''">
	{{ selected }}
  </span>
```

---

# Day 9

https://hackmd.io/895Xn5fSRYumx2tzA6X5Sw

# method

```html
<template>
	<a href="#" @click="addProduct(fruit)">加入購物車</a>
</template>

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
</script>
```

- `@click` 綁定方法 `method` 將水果加入陣列 (例如購物車)
- 方法檢查庫存、計算數量、透過 `push` 推送物件

---

# Day 10

https://hackmd.io/WdCj0K7LQUSc00iSp1QIwA

## computed

- 會根據其依賴的資料進行自動更新，只在資料發生變化時重新計算
- 可避免在樣板區塊放入太多邏輯，例如 `{{ count * 3 }}`
- `method` 每次渲染都會執行，`computed` 只在依賴的數據變化時更新

```html
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
```

## getter & setter

```html
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
```

- 多數情況下 `computed` 只需要 getter，用來基於響應式數據計算值
- 如果需要，`computed` 也可以定義 setter，使其成為一個可寫的屬性
- 行為類似於 JavaScript 中的 getter 和 setter，但與 Vue 結合得更緊密

```js
const count = ref(2);

const tripleCount = computed(() => {
  return count.value * 3;
});

// 嘗試修改會報錯，因為 tripleCount 唯讀
tripleCount.value = 10;
```

- 預設情況下，`computed` 用來計算依賴數據的衍生值，而不是直接修改數據
- 如需修改數據，應該直接操作原始的響應式數據（如 `ref` 或 `reactive`）為主

## demo

```js
const totalPrice = computed(() => {
  let price = 0;
  fruitData.value.forEach(fruit => {
    price += fruit.count * fruit.price
  });
  return price;
})
```

- 計算購買所有水果的總金額，以下為更簡潔的寫法

```js
const totalPrice = computed(() =>
  fruitData.value.reduce((price, fruit) =>
    price + fruit.count * fruit.price, 0
  ));
```

- `reduce` 直接將陣列中的每個元素累加到一個初始值上
- 核心概念是「累加器」，可以用來執行各種累積操作，例如加總、乘積、串接字串
- `reduce` 名稱來自於它將陣列「縮減」為單一值的功能

---

# Day 11

https://hackmd.io/qte1WAMuSQSNBW7FkAWelg

## watch

- 用來監控 Vue 3 中的響應式物件（例如 `ref` 或 `reactive`）的變化
- 當被監控的值發生變化時，`watch` 會執行指定的 callback

```html
<template>
  <label for="email">Email：</label>
  <input id="email" v-model="email" />
  <span v-if="emailError">{{ emailError }}</span>
</template>

<script setup>
import { ref, watch } from 'vue';

const email = ref('');
const emailError = ref('');

watch(email, (newVal) => {
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!emailPattern.test(newVal)) {
    emailError.value = 'email 格式不正確';
  } else {
    emailError.value = '';
  }
});
</script>

<style>
span {
  color: red;
  font-size: 0.8em;
}
</style>
```

- 可以監控單一的 `ref` 或 `reactive` 屬性，也可以監控多個值（使用陣列）
- 當監控的值改變時，會接收兩個參數：新值（`newVal`）與舊值（`oldVal`）

```js
import { ref, watch } from 'vue';

const count = ref(0);

watch(count, (newVal, oldVal) => {
  console.log(`count 從 ${oldVal} 變為 ${newVal}`);
});

count.value++; 
```

- 監聽多個項目

```js
// 監測多個 ref
watch([count1, count2], ([newCount1, newCount2], [oldCount1, oldCount2]) => {
  console.log(`count1 從 ${oldCount1} 變為 ${newCount1}`);
  console.log(`count2 從 ${oldCount2} 變為 ${newCount2}`);
});
```

- 監聽多個項目，用於資料驗證

```html
<template>
  <div class="mb-2">
    <label for="username">姓名：</label>
    <input id="username" v-model="username" />
    <span v-if="usernameError">{{ usernameError }}</span>
  </div>
  <div class="mb-2">
    <label for="phone">電話：</label>
    <input id="phone" v-model="phone" />
    <span v-if="phoneError">{{ phoneError }}</span>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const username = ref('');
const phone = ref('');
const usernameError = ref('');
const phoneError = ref('');

watch([username, phone], ([newUsername, newPhone]) => {
  if (newUsername.length < 3) {
    usernameError.value = '需要正確的輸入名稱';
  } else {
    usernameError.value = '';
  }

  const phonePattern = /^[0-9]{10}$/;
  if (!phonePattern.test(newPhone)) {
    phoneError.value = '需要正確的輸入電話號碼';
  } else {
    phoneError.value = '';
  }
});
</script>

<style>
span {
  color: red;
  font-size: 0.8em;
}
</style>
```

### deep

- 當監控的目標是物件或陣列時，`deep: true` 允許監控目標內部的所有屬性或元素
- 理論上無層數限制，可遞歸監控所有層級，但過多的嵌套可能影響效能

```html
<template>
  <div class="mb-2">
    <label for="username">姓名：</label>
    <input id="username" v-model="user.username" />
    <span v-if="errors.username">{{ errors.username }}</span>
  </div>
  <div class="mb-2">
    <label for="phone">電話：</label>
    <input id="phone" v-model="user.phone" />
    <span v-if="errors.phone">{{ errors.phone }}</span>
  </div>
</template>

<script setup>
import { ref, watch } from 'vue';

const user = ref({
  username: '',
  phone: ''
});

const errors = ref({
  username: '',
  phone: ''
});

watch(user, (newVal) => {
  if (newVal.username.length < 3) {
    errors.value.username = '需要正確的輸入名稱';
  } else {
    errors.value.username = '';
  }

  const phonePattern = /^[0-9]{10}$/;
  if (!phonePattern.test(newVal.phone)) {
    errors.value.phone = '需要正確的輸入電話號碼';
  } else {
    errors.value.phone = '';
  }
}, { deep: true });
</script>
```

- 如果只需要監控特定屬性，建議直接監控該屬性，而不是使用深層監控

```js
watch(() => user.value.username, (newVal) => {
  if (newVal.length < 3) {
    errors.value.username = '需要正確的輸入名稱';
  } else {
    errors.value.username = '';
  }
});
```

### immediate

- 初始化就直接執行，例如顯示畫面馬上驗證資料

```js
watch(user, (newVal) => {
  //callback()
}, { deep: true, immediate: true });
```

---

# Day 12

https://hackmd.io/L8An80BGQiePO-2g6hlVUQ

## component

- 每個 Vue 元件都是一個獨立的、可重複使用的實體
- 包含了自己的模板、邏輯和樣式 (也就是 TSS)
- 以下是一個範例，將 Header 與 Footer 拆為獨立元件

```html
<!-- Header.vue -->
<template>
  <header>
    我是 header
  </header>
</template>

<style>
  header {
    background-color: lightskyblue;
  }
</style>
```
```html
<!-- Footer.vue -->
<template>
  <footer>
    我是 footer
  </footer>
</template>

<style>
  footer {
    background-color: lightgray;
  }
</style>
```

- 然後在需要的頁面引用

```html
<!-- App.vue -->
<script setup>
import Header from './Header.vue'
import Footer from './Footer.vue'
</script>

<template>
  <Header />
   <div class="content">
     <h2>我是內容</h2>
   </div>
  <Footer />
</template>

<style>
  .content {
    margin: 50px 0;
  }
</style>
```

> [!note] 
> View 也可視為一個 Component

---

# Day 13

https://hackmd.io/vBgGQagdTYKn5_REkPjDTA

## props

- 向元件單向傳遞資料
- 或者說，由外層向內，由父元件向子元件傳遞資料
- 子元件透過 `defineProps` 定義屬性，一個子元件只能有一個 `definProps`
- 父元件 `import` 子元件後設定屬性，以下是最簡單寫法，只宣告名稱，變數可為任意型別

```html
<!-- 子元件 -->
<template>
  <h1>{{ data }}</h1>
</template>

<script setup>
// import { defineProps } from 'vue'; // 可省略但不建議

defineProps(['data'])
</script>
```
```html
<!-- 父元件 -->
<template>
  <Child :data="data" />
</template>

<script setup>
import { ref } from 'vue'
import Child from './components/Child.vue';

const data = ref('Hello')
</script>
```

- 可定義多個傳入變數，傳入陣列也沒問題

```js
defineProps(['data', 'value'])
// 上述等同 defineProps({ content: null, value: null });
```
```html
<Child :data="data" :value="2" />
```

- 可詳細定義屬性的類型，甚至包含驗證

```js
// 階段1：基本類型檢查
defineProps({
  content: Object,
  value: [String, Number]
});

// 階段2：加入預設值
defineProps({
  content: {
    type: Object,
    default: () => ({})
  },
  value: {
    type: [String, Number],
    default: ''
  }
});

// 階段3：完整驗證
defineProps({
  content: {
    type: Object,
    required: true,
    validator: (value) => {
      return value && typeof value.title === 'string';
    }
  },
  value: {
    type: [String, Number],
    default: ''
  }
});
```

## naming rule

- JavaScript 通常使用 camelCase，子元件內的屬性命名也遵循此規則
- 父元件設定子元件屬性時，屬於 HTML attribute，建議使用 kebab-case

```js
defineProps(['innerData']); //camelCase
```
```html
<EasyCard :inner-data="data" /> <!-- HTML 屬性一律使用 kebab-case -->
<EasyCard :innerData="data" /> <!-- Vue 會轉換，但不建議 -->
```

---


# Day 14

https://hackmd.io/wIkIQO0GRjqxoO34lvdcsg

## Emits

- 對比 `props`，`emits` 是由子元件向外傳遞資訊
- 透過定義事件的方式綁定，下面是一個購物車例子
	- 子元件定義 `emit` `add-to-cart` 與方法 `addToCart`，再透過 `click` 事件去處發
	- 父元件定義方法 `handleAddToCart`，並於綁定子元件 `<Card />` 方法 `add-to-cart`

```html
<!-- 子元件(購物車) -->
<script setup>
import { defineEmits } from 'vue'; // 可省略但建議寫

// define emit event
const emit = defineEmits(['add-to-cart']);

const addToCart = (title) => {
  emit('add-to-cart', title)
}
</script>

<template>
<a @click.prevent="addToCart(title)" href="#">加入購物車</a>
</template>
```

```html
<!-- 父元件 -->
<script setup>
import Card from './Card.vue';

const handleAddToCart = (fruitTitle) => {
  alert(`您已將 ${fruitTitle} 加入購物車`);
}
</script>

<template>
	<Card @add-to-cart="handleAddToCart" />
</template>
```

- 同樣要注意命名規則，JavaScript 用 camelCase 而 HTML 屬性則為 kebab-case

> [!note] 
> `kabab` 是烤肉串的意思，字串之間用 `-` 串聯，看起來就像烤肉串

---

# Day 15

https://hackmd.io/IzZ00F_kRLS_1F7LQ-uptQ

## toast

- Bootstrap 5 元件，通常用於顯示通知

```html
<div class="position-fixed top-0 end-0 p-3" style="z-index: 1050">
	<div v-if="state.message" 
	class="toast show align-items-center text-white bg-success border-0">
		<div class="d-flex">
			<div class="toast-body">{{ state.message }}</div>
			<button type="button" 
			class="btn-close btn-close-white me-2 m-auto" 
			@click="state.message = ''">
			</button>
		</div>
	</div>
</div>
```
```js
import { reactive, ref } from 'vue'

const state = reactive({
  message: ''
})

const fruitData = ref([
  // json array
])

const addToCart = (name) => {
  state.message = `${name}`
  
  setTimeout(() => {
    state.message = ''
  }, 3000)
}
```

- 設定為 `state.message` 有資料才顯示，並於透過 `setTimeout` 於三秒後移除

## reactive vs ref

- reactive 僅可用於陣列或物件，存取不需要 `.value`

```js
const state = reactive({
  message: '',
  count: 0
})

state.message = 'Hello'
state.count++
```

```js
const message = ref('')
const count = ref(0)
const data = ref({ name: 'John' })

message.value = 'Hello'
count.value++
data.value.name = 'Jane'

// 在模板中不需要 .value
// <div>{{ message }}</div>
```

---

# Day 16

https://hackmd.io/CV305t9FSragDT7f0PWH8g

## provide & inject

- 外層元件定義要提供 `provide` 的變數或方法 (設定 key = 名稱)
- 內層元件定義要注入 `inject` 的變數或方法 (使用 key 取出)

```html
<!-- 父元件 -->
<template>
  <input type="text" v-model="msg"></input>
  <!-- 子元件 -->
  <Child16 />
</template>

<script setup>
import { ref, provide } from 'vue'
import Child16 from './components/Child16.vue';

const msg = ref('')
const resetTextbox = () => { 
  msg.value = ''
}

// 定義要提供(provide)的變數或方法，設定名稱作為 key
provide('msg', msg)
provide('clearMsg', resetTextbox)
</script>
```

```html
<!-- 子元件 -->
<template>
  <div v-if="injectMsg" class="alert alert-success">
    {{ injectMsg }}
		<button type="button" @click="injectMethod">Clear</button>
  </div>
</template>

<script setup>
import { inject } from 'vue';

// 宣告注入的變數或方法，使用 key 取出 (並給定預設值避免異常)
const injectMsg = inject('msg', '')
const injectMethod = inject('resetMsg', () => { })
```

## Symbol

- Symbol 是 ES6 引入的一種資料類型
- 每個 Symbol 都是唯一的，即使描述相同也不會相等

```js
const sym1 = Symbol('description')
const sym2 = Symbol('description')
console.log(sym1 === sym2) // false
```

- 可用於避免 provide 與 inject 定義 key 的時候衝突

```js
// store/keys.js
export const StoreKey = Symbol('store')

// main.js
import { createApp } from 'vue'
import { StoreKey } from './store/keys'
import store from './store'

const app = createApp(App)
app.provide(StoreKey, store)

// 任何子组件中
import { inject } from 'vue'
import { StoreKey } from '@/store/keys'

export default {
  setup() {
    const store = inject(StoreKey)
    return { store }
  }
}
```

---

# Day 17 

https://hackmd.io/2ONHhWGJQNGjU2ixi6i4Ag

## Axios

https://axios-http.com/docs/intro

- 基於 `Promise`，可以輕鬆地與後端 `API` 進行串接
- 比起 `AJAX` 更輕量化，寫起來更加的直接、閱讀性更好
- 提供了簡單的 `API` 執行 `GET`、`POST`、`PUT`、`DELETE` 等 `HTTP` 請求
- 使用 `Promise`，所以可以使用 `.then`、`.catch` 等非同步操作語法
- 可以在發送 `request` 前或取得 `response` 後，進行轉換資料操作

```sh
npm i axios
```

```js
import axios from 'axios'

const api = 'https://jsonplaceholder.typicode.com/posts';

axios.get(api)
  .then((res) => {
    console.log(res);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

- 常見的使用方式

```js
axios.get(url, config);
axios.post(url, data, config);
axios.put(url, data, config);
axios.delete(url, config);
axios.patch(url, data, config);
```

- Axios 的 `response` 是一個結構物件，常用屬性如下

```js
axios.get(api).then((res) => {
    console.log(res.data)       // 回應的內容，會自動轉 json
    console.log(res.config)     // 設定的請求配置
    console.log(res.status)     // 回應的狀態碼
    console.log(res.statusText) // 回應的狀態訊息 (不一定有)
    // response headers，例如可訪問 response.headers['content-type']
    console.log(res.headers)
    // 指生成此回應的原生請求，在瀏覽器上就會是 XMLHttpRequest 實例
    console.log(res.request)
  })
```

### config

```js
axios.defaults.baseURL = 'https://your-base-url';
axios.defaults.headers.common['Authorization'] = 'Bearer token';
axios.defaults.headers.post['Content-Type'] = 'application/json';
axios.defaults.timeout = 10000;

axios.get('/')
  .then(res => {
    console.log(res.data);
  })
  .catch(error => {
    console.error('Error:', error);
  });
```

### onMounted

- **確保 DOM 已準備好**：雖然資料請求不需要 DOM，但這是最佳實踐
- **生命週期清晰**：明確表示這是在元件掛載後執行的邏輯
- **避免潛在問題**：如果元件在請求完成前被銷毀，可能會有記憶體洩漏

```js
import axios from 'axios'
import { ref, onMounted } from 'vue'

const posts = ref([])
const loading = ref(true)
const api = 'https://jsonplaceholder.typicode.com/posts';

onMounted(async () => {
  try {
    const res = await axios.get(api)
    
    posts.value = res.data
  } catch (error) {
    console.error('Error:', error);
  } finally {
    loading.value = false
  }
})
```

---

# Day 18

https://hackmd.io/6BZWDKikQneK36hD1wBkxg

## 登入驗證實作

- 實作呼叫登入 API 並取得結果

<details>
<summary>官方範例(註冊)</summary>

```html
<template>
  <div id="app">
    <div class="container mt-2">
      <h2>註冊</h2>
      <form>
        <div class="form-group">
          <label for="exampleInputEmail1">Email</label>
          <input v-model="emailSignUp" type="email" class="form-control" id="exampleInputEmail1" aria-describedby="emailHelp" placeholder="請輸入信箱">
        </div>
        <div class="form-group my-3">
          <label for="exampleInputPassword1">Password</label>
          <input v-model="passwordSignUp" type="password" class="form-control" id="exampleInputPassword1" placeholder="請輸入密碼">
        </div>
        <div class="form-group my-3">
          <label for="exampleInputNickName">Nick Name</label>
          <input v-model="nickname" type="text" class="form-control" id="exampleInputNickName" placeholder="請輸入暱稱">
        </div>
        <button @click="signUp" type="button" class="btn btn-primary">註冊</button>
      </form>
      <template v-if="messageSignUp">
        <p :class="{'mt-2': true, 'text-danger': isErrorSignUp, 'text-success': !isErrorSignUp}"> {{ messageSignUp }}</p>       
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import axios from 'axios'


const api = 'https://todolist-api.hexschool.io';

const emailSignUp = ref('');
const passwordSignUp = ref('');
const nickname = ref('');
const messageSignUp = ref('');
const isErrorSignUp = ref(false);

const signUp = async () => {
  try {
    const response = await axios.post(`${api}/users/sign_up`, {
      email: emailSignUp.value,
      password: passwordSignUp.value,
      nickname: nickname.value,
    });
    messageSignUp.value = '註冊成功. UID: ' + response.data.uid;
    isErrorSignUp.value = false;
  } catch (error) {
    messageSignUp.value = '註冊失敗: ' + error.response.data.message;
    isErrorSignUp.value = true;
  }
};
</script>
```

</details>

- 六角 WebAPI

```
https://todolist-api.hexschool.io/doc/
```

- 實作登入，成功時儲存 `token`

```js
import axios from "axios";
import { ref } from "vue";

const api = 'https://todolist-api.hexschool.io';

const emailSignIn = ref('');
const passwordSignIn = ref('');

const responseMessage = ref('');
const isErrorMessage = ref(false);
const token = ref('');

const signIn = async () => {
  try {
    const res = await axios.post(`${api}/users/sign_in`, {
      "email": emailSignIn.value,
      "password": passwordSignIn.value
    })
    isErrorMessage.value = false
    console.log(res)
    token.value = res.data.token
    responseMessage.value = `登入成功: ${res.data.nickname} [token: ${token.value}]`
  }
  catch (error) {
    isErrorMessage.value = true
    console.log(error)
    responseMessage.value = `登入失敗: ${error.response.data.message}`
  }
};
```

---

# Day 19 新增資料

https://hackmd.io/naxt5IlOTqynthM9Ee5QkQ

- 需要 JWT 驗證的 API 必須帶入 token

```js
const url = "example/api";

axios.post(url, data, {
    headers: {
      Authorization: token.value,
    },
  })
  .then(response => {
    console.log('成功:', response.data);
  })
  .catch(error => {
    console.error('失敗:', error);
  });
```

```js
// 示意範例
const res = async () => {
  await axios.post(`endpoint`, { data }, { config })
    .then(res => { res.data })
    .catch(error => { error.response.data })
};
```

## 顯示訊息的範本

```html
<template v-if="messageSignIn">
	<p :class="{ 'mt-2': true, 'text-danger': isErrorSignIn, 'text-success': !isErrorSignIn }"> {{ messageSignIn }}
	</p>
</template>
```

### 傳遞參數

```js
axios.get(url[, config])
axios.post(url[, data[, config]])
```

- 這個順序是固定的
- GET 請求通常不需要傳送 body 資料，而 POST 請求需要傳送資料到伺服器

### config 常用參數

```js
{
  headers: {},           // 請求標頭
  timeout: 5000,         // 超時時間（毫秒）
  baseURL: 'https://api.example.com',  // 基礎 URL
  params: {},            // URL 參數（用於 GET）
  data: {},              // 請求體資料（用於 POST/PUT）
  responseType: 'json',  // 響應資料格式
  withCredentials: true, // 跨域請求是否帶 cookies
  validateStatus: function (status) {
    return status >= 200 && status < 300;
  },
  maxRedirects: 5,       // 最大重定向次數
  proxy: {},             // 代理設定
  auth: {                // HTTP 基本認證
    username: 'user',
    password: 'pass'
  }
}
```

### header 參數

```js
headers: {
  'Authorization': 'Bearer token...',
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'User-Agent': 'MyApp/1.0',
  'X-Requested-With': 'XMLHttpRequest',
  'Cache-Control': 'no-cache',
  'Accept-Language': 'zh-TW,zh;q=0.9',
  'X-API-Key': 'your-api-key',
  'X-CSRF-Token': 'csrf-token'
}
```

### 驗證類型

- 應該加上前綴，如果伺服器比較嚴謹可能會檢查

```js
await axios.get(`${api}/todos`, {
	headers: {
		Authorization: `Bearer ${token.value}`,  // 應該加上 Bearer 前綴
	},
})
```

- 其他驗證方式，例如 API Key、基本驗證與其他

```js
headers: {
  'X-API-Key': 'your-api-key',
  // 或
  'Authorization': 'ApiKey your-api-key'
}

// 基本驗證
// 方式一：直接在 headers
headers: {
  'Authorization': 'Basic ' + btoa('username:password')
}

// 方式二：使用 axios 的 auth 選項
auth: {
  username: 'user',
  password: 'password'
}

// Cookie
{
  withCredentials: true,  // 自動帶上 cookies
  headers: {
    'X-CSRF-Token': 'csrf-token'  // 防 CSRF 攻擊
  }
}
```

### 驗證格式

HTTP Authorization Header 的標準格式

```js
Authorization: <type> <credentials>
```

**Bearer** 是一種認證類型（Authentication Scheme），告訴伺服器這是什麼類型的 token：

- `Bearer token123` - 表示這是 Bearer token（通常是 JWT）
- `Basic dXNlcjpwYXNz` - 表示這是 Basic 認證
- `ApiKey abc123` - 表示這是 API Key

### 如果不加 Bearer 會怎樣？

1. **可能會被伺服器拒絕** - 很多伺服器會檢查格式
2. **不符合 RFC 7617 標準** - HTTP 認證標準
3. **混淆認證類型** - 伺服器不知道如何處理

不過，有些 API 可能設計得比較寬鬆，直接接受 token 也能正常運作。但建議還是遵循標準。

### header 引號

```js
// 方式一：加引號（推薦）
headers: {
  'Authorization': 'Bearer token123',
  'Content-Type': 'application/json'
}

// 方式二：不加引號（也可以）
headers: {
  Authorization: 'Bearer token123',
  ContentType: 'application/json'  // 注意：這種情況不能用 - 符號
}
```

**建議加引號的原因：**

- 可以使用 `-` 符號（如 `Content-Type`）
- 避免 JavaScript 關鍵字衝突
- 更明確表示這是字串屬性

### Custom Headers `X-`

```js
// 標準(無前綴)
headers: {
  'Authorization': 'Bearer token',
  'Content-Type': 'application/json',
  'Accept': 'application/json',
  'User-Agent': 'MyApp/1.0',
  'Cache-Control': 'no-cache'
}

// 自定義
headers: {
  'X-API-Key': 'your-api-key',        // API 金鑰
  'X-Requested-With': 'XMLHttpRequest', // AJAX 請求標識
  'X-CSRF-Token': 'csrf-token',       // CSRF 防護
  'X-User-ID': '12345',               // 用戶 ID
  'X-Client-Version': '1.2.3',       // 客戶端版本
  'X-Custom-Header': 'custom-value'   // 自定義資料
}
```

- **避免與標準 HTTP Headers 衝突**
- **表示這是應用程式特定的標頭**
- **方便識別自定義功能**

---

# Day 20

https://hackmd.io/xu7R7CD4RXyCBBA8TkElwg

## 刪除資料

```js
const url = "example/api"

axios.delete(`${url}/${ 要刪除產品的id }`, {
    headers: {
      Authorization: token.value,
    },
  })
  .then(response => {
    console.log('資料刪除成功:', response.data);
  })
  .catch(error => {
    console.error('刪除資料時發生錯誤:', error);
  });
```

## 移除陣列元素

- 常用 filter

```js
todos.value = todos.value.filter(todo => todo.id !== id)
```

- 或 findIndex 後 splice

```js
const index = todos.value.findIndex(todo => todo.id === id);
if (index !== -1) {
	todos.value.splice(index, 1);
}
```

| 比較項目          | `filter`   | `findIndex` + `splice` |
| ------------- | ---------- | ---------------------- |
| **可讀性**       | ⭐⭐⭐⭐⭐      | ⭐⭐⭐                    |
| **記憶體使用**     | 較高 - 創建新陣列 | 較低 - 修改原陣列             |
| **時間複雜度**     | 遍歷整個陣列     | 找到後立即停止                |
| **小陣列效能**     | 優秀         | 優秀                     |
| **大陣列效能**     | 普通         | 較好                     |
| **副作用風險**     | 無          | 有 (修改原陣列)              |
| **Vue 3 響應式** | ✅          | ✅                      |
| **錯誤處理**      | 找不到元素不會出錯  | 需要手動檢查 index !== -1    |
| **現代開發風格**    | ⭐⭐⭐⭐⭐      | ⭐⭐⭐                    |

---
