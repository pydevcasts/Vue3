# 🚀 مدیریت وضعیت سراسری در Vue 3 با **Pinia** و **Vuex**  

در Vue 3، برای مدیریت وضعیت سراسری دو روش محبوب وجود دارد:  
1️⃣ **Pinia** (جایگزین رسمی و مدرن Vuex)  
2️⃣ **Vuex** (روش سنتی‌تر و پرکاربرد در پروژه‌های قدیمی)  

در این مقاله، ابتدا **Pinia** را بررسی می‌کنیم، سپس **Vuex** را معرفی کرده و در پایان، یک **جمع‌بندی مقایسه‌ای** انجام می‌دهیم تا ببینیم کدام روش برای پروژه شما مناسب‌تر است. 🎯  

---

## 🔥 **۱. مدیریت وضعیت با Pinia (جدیدترین روش پیشنهادی Vue)**  

### ✅ **چرا باید از Pinia استفاده کنیم؟**  

✔️ **ساده‌تر از Vuex** 🎯  
✔️ **پرفورمنس بهتر و سبک‌تر** ⚡  
✔️ **بدون نیاز به Mutations** ⏩  
✔️ **پشتیبانی عالی از TypeScript** 🏆  
✔️ **کاملاً رسمی برای Vue 3** ✅  

### 📌 **۱. نصب Pinia**  
ابتدا **Pinia** را در پروژه Vue 3 نصب کنید:  

```sh
npm install pinia
```

سپس در **`main.js`** مقداردهی اولیه کنید:  

```js
import { createPinia } from 'pinia'
import { createApp } from 'vue'
import App from './App.vue'

const app = createApp(App)
app.use(createPinia()) // 📌 فعال کردن Pinia
app.mount('#app')
```

---

### 📌 **۲. ایجاد یک Store در Pinia**  
حالا یک **استور (Store)** برای مدیریت وضعیت ایجاد می‌کنیم. مثلا یک **استور شمارنده** در مسیر **`stores/counter.js`**:

```js
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++
    },
    decrement() {
      this.count--
    }
  },
  getters: {
    doubleCount: (state) => state.count * 2
  }
})
```

---

### 📌 **۳. استفاده از Pinia در کامپوننت‌ها**  
اکنون استور را در یک **کامپوننت Vue** استفاده می‌کنیم:

```vue
<script setup>
import { useCounterStore } from '@/stores/counter'

const counterStore = useCounterStore()
</script>

<template>
  <div>
    <p>🔢 مقدار شمارنده: {{ counterStore.count }}</p>
    <p>✨ مقدار دوبرابر: {{ counterStore.doubleCount }}</p>
    <button @click="counterStore.increment">➕ افزایش</button>
    <button @click="counterStore.decrement">➖ کاهش</button>
  </div>
</template>
```

🚀 **و تمام! حالا شما یک مدیریت وضعیت ساده و سریع در Vue 3 دارید.**  

---

## 🔥 **۲. مدیریت وضعیت با Vuex (روش قدیمی‌تر اما هنوز پرکاربرد)**  

Vuex تا قبل از Vue 3، **استاندارد مدیریت وضعیت در Vue** بود. اما با معرفی Pinia، بسیاری از پروژه‌ها به سمت آن مهاجرت کرده‌اند.  

### 📌 **۱. نصب Vuex**  
برای نصب Vuex 4 (مخصوص Vue 3) دستور زیر را اجرا کنید:  

```sh
npm install vuex@next
```

سپس در **`main.js`** آن را مقداردهی اولیه کنید:

```js
import { createStore } from 'vuex'
import { createApp } from 'vue'
import App from './App.vue'

const store = createStore({
  state() {
    return { count: 0 }
  },
  mutations: {
    increment(state) {
      state.count++
    },
    decrement(state) {
      state.count--
    }
  },
  actions: {
    asyncIncrement({ commit }) {
      setTimeout(() => {
        commit('increment')
      }, 1000)
    }
  },
  getters: {
    doubleCount: (state) => state.count * 2
  }
})

const app = createApp(App)
app.use(store)
app.mount('#app')
```

---

### 📌 **۲. استفاده از Vuex در کامپوننت‌ها**  

```vue
<script setup>
import { useStore } from 'vuex'
import { computed } from 'vue'

const store = useStore()
const count = computed(() => store.state.count)
const doubleCount = computed(() => store.getters.doubleCount)

const increment = () => store.commit('increment')
const decrement = () => store.commit('decrement')
</script>

<template>
  <div>
    <p>🔢 مقدار شمارنده: {{ count }}</p>
    <p>✨ مقدار دوبرابر: {{ doubleCount }}</p>
    <button @click="increment">➕ افزایش</button>
    <button @click="decrement">➖ کاهش</button>
  </div>
</template>
```

🚀 **و تمام! این روش مدیریت وضعیت با Vuex را در Vue 3 پیاده‌سازی می‌کند.**  

---

## 🎯 **۳. مقایسه نهایی: Pinia یا Vuex؟ کدام را انتخاب کنیم؟**  

✅ **اگر پروژه شما جدید است و از Vue 3 استفاده می‌کنید، Pinia انتخاب بهتری است.**  
❌ **اگر پروژه‌تان قدیمی است و Vuex از قبل در آن پیاده‌سازی شده، نیازی به مهاجرت ندارید، مگر اینکه بخواهید کدتان را بهینه‌تر کنید.**  

| ویژگی       | ✅ **Pinia** | ❌ **Vuex** |
|------------|------------|------------|
| سادگی و یادگیری آسان | ⭐⭐⭐⭐ | ⭐⭐ |
| عملکرد (Performance) | ⚡ خیلی سریع | 🐢 کندتر |
| نیاز به Mutations | ❌ ندارد | ✅ نیاز دارد |
| پشتیبانی از TypeScript | ✅ بله | 🚫 پیچیده‌تر |
| پیشنهاد رسمی برای Vue 3 | ✅ بله | ❌ خیر (منسوخ شده) |

🔥 **پس، کدام را انتخاب می‌کنید؟**  
➤ اگر پروژه‌ای جدید دارید، **Pinia بهترین گزینه است** 🎯  
➤ اگر روی پروژه‌ای قدیمی با Vuex کار می‌کنید، **مهاجرت الزامی نیست، اما پیشنهاد می‌شود** 🏆  
