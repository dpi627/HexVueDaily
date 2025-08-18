#vue #hex 

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

