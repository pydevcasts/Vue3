
# 19. مدیریت وضعیت در Vue.js

در این فصل، با روش‌های مختلف **مدیریت وضعیت (State Management)** در Vue.js آشنا می‌شویم. مدیریت وضعیت یکی از مهم‌ترین بخش‌های توسعه برنامه‌های بزرگ است.

## روش‌های مدیریت وضعیت در Vue.js

📌 **در Vue.js چندین روش برای مدیریت وضعیت داریم:**

1. **Composition API** (برای وضعیت‌های ساده)
2. **Pinia** (پیشنهاد رسمی برای Vue 3)
3. **Vuex** (روش قدیمی‌تر)

## ۱. مدیریت وضعیت با Composition API

📌 **برای پروژه‌های کوچک، می‌توانیم از Composition API استفاده کنیم:**

```javascript
// stores/counter.js
import { ref, computed } from 'vue'

export function useCounter() {
  const count = ref(0)
  const doubleCount = computed(() => count.value * 2)

  function increment() {
    count.value++
  }

  function decrement() {
    count.value--
  }

  return {
    count,
    doubleCount,
    increment,
    decrement
  }
}
```

### استفاده در کامپوننت:

```vue
<template>
  <div>
    <p>شمارنده: {{ count }}</p>
    <p>مقدار دوبرابر: {{ doubleCount }}</p>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>

<script setup>
import { useCounter } from '@/stores/counter'

const { count, doubleCount, increment, decrement } = useCounter()
</script>
```

## ۲. مدیریت وضعیت با Pinia

📌 **Pinia روش پیشنهادی برای مدیریت وضعیت در Vue 3 است.**

### نصب Pinia:

```bash
npm install pinia
```

### راه‌اندازی در `main.js`:

```javascript
import { createApp } from 'vue'
import { createPinia } from 'pinia'
import App from './App.vue'

const app = createApp(App)
app.use(createPinia())
app.mount('#app')
```

### تعریف Store:

```javascript
// stores/counter.js
import { defineStore } from 'pinia'

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0,
    name: 'Counter'
  }),

  getters: {
    doubleCount: (state) => state.count * 2,
    countPlusName: (state) => `${state.count} - ${state.name}`
  },

  actions: {
    increment() {
      this.count++
    },
    async fetchCount() {
      const response = await fetch('/api/count')
      this.count = await response.json()
    }
  }
})
```

### استفاده در کامپوننت:

```vue
<template>
  <div>
    <p>شمارنده: {{ counter.count }}</p>
    <p>نام: {{ counter.name }}</p>
    <p>مقدار دوبرابر: {{ counter.doubleCount }}</p>
    <button @click="counter.increment">افزایش</button>
    <button @click="counter.fetchCount">دریافت از سرور</button>
  </div>
</template>

<script setup>
import { useCounterStore } from '@/stores/counter'

const counter = useCounterStore()
</script>
```

## ۳. مدیریت وضعیت با Vuex

📌 **Vuex روش قدیمی‌تر مدیریت وضعیت است که هنوز در پروژه‌های قدیمی استفاده می‌شود.**

### نصب Vuex:

```bash
npm install vuex@next
```

### راه‌اندازی Store:

```javascript
// store/index.js
import { createStore } from 'vuex'

export default createStore({
  state: {
    count: 0
  },
  mutations: {
    increment(state) {
      state.count++
    }
  },
  actions: {
    async fetchCount({ commit }) {
      const response = await fetch('/api/count')
      const count = await response.json()
      commit('setCount', count)
    }
  },
  getters: {
    doubleCount: state => state.count * 2
  }
})
```

### استفاده در کامپوننت:

```vue
<template>
  <div>
    <p>شمارنده: {{ $store.state.count }}</p>
    <p>مقدار دوبرابر: {{ $store.getters.doubleCount }}</p>
    <button @click="$store.commit('increment')">افزایش</button>
    <button @click="$store.dispatch('fetchCount')">دریافت از سرور</button>
  </div>
</template>
```

## مقایسه روش‌ها

📌 **هر روش مزایا و معایب خود را دارد:**

| ویژگی | Composition API | Pinia | Vuex |
|-------|----------------|--------|------|
| پیچیدگی | کم | متوسط | زیاد |
| مناسب برای | پروژه‌های کوچک | پروژه‌های متوسط و بزرگ | پروژه‌های قدیمی |
| DevTools | ✅ | ✅ | ✅ |
| TypeScript | عالی | عالی | متوسط |
| یادگیری | آسان | متوسط | سخت |

## بهترین شیوه‌ها

✅ **توصیه‌های کلیدی برای مدیریت وضعیت:**

1. برای پروژه‌های کوچک از Composition API استفاده کنید
2. برای پروژه‌های متوسط و بزرگ از Pinia استفاده کنید
3. وضعیت‌ها را به صورت منطقی دسته‌بندی کنید
4. از TypeScript برای امنیت بیشتر استفاده کنید
5. از DevTools برای دیباگ کردن استفاده کنید

## جمع‌بندی

✅ **Vue.js روش‌های مختلفی برای مدیریت وضعیت ارائه می‌دهد**
✅ **Pinia جایگزین مدرن و پیشنهادی برای Vuex است**
✅ **برای پروژه‌های کوچک، Composition API کافی است**
✅ **انتخاب روش مناسب به اندازه و نیازهای پروژه بستگی دارد**

