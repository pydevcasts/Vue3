### **📌 صفحه 20 - آشنایی با Composableها در Vue 3**
 
در این صفحه، **Composableها** را در Vue 3 بررسی می‌کنیم و یاد می‌گیریم که چگونه با استفاده از **توابع ترکیبی (Composition Functions)**، **کدهای قابل استفاده مجدد** ایجاد کنیم.
  
## **۱. Composable چیست؟**
 
📌 **Composableها توابعی هستند که منطق قابل استفاده مجدد را در Vue 3 مدیریت می‌کنند.** ✅ در Vue 3، از **ترکیب API (Composition API)** برای مدیریت وضعیت و منطق در کامپوننت‌ها استفاده می‌شود. ✅ **Composableها کمک می‌کنند که کدهای تمیز، ماژولار و سازمان‌یافته داشته باشیم.**
 
📌 **چرا از Composable استفاده کنیم؟** ✔ **کاهش پیچیدگی کامپوننت‌ها**: کامپوننت‌های Vue را ساده‌تر و خواناتر می‌کند. ✔ **استفاده مجدد از کد**: می‌توان یک منطق را در چندین کامپوننت مختلف استفاده کرد. ✔ **بهبود تست‌پذیری**: توابع composable مستقل از UI هستند و راحت‌تر تست می‌شوند.
  
## **۲. ایجاد اولین Composable**
 
📌 یک composable ساده برای مدیریت **حالت (`state`) و تغییر مقدار آن** ایجاد می‌کنیم.
 
✅ **ایجاد یک فایل `useCounter.js` در مسیر `src/composables/`**
 `import { ref } from 'vue';  export function useCounter() {   const count = ref(0);    function increment() {     count.value++;   }    function decrement() {     count.value--;   }    return { count, increment, decrement }; } ` 
✅ **توضیح کد:** ✔ `ref(0)` مقدار `count` را به‌صورت واکنش‌گرا (`reactive`) تعریف می‌کند. ✔ `increment` و `decrement` مقدار `count` را افزایش یا کاهش می‌دهند. ✔ در انتها، **مقادیر `count` و توابع را برمی‌گردانیم تا در کامپوننت‌ها استفاده شوند.**
  
## **۳. استفاده از Composable در یک کامپوننت**
 
📌 **حال این `useCounter.js` را در یک کامپوننت مانند `Counter.vue` استفاده می‌کنیم.**
 
✅ **فایل `Counter.vue`**
 `<template>   <div>     <h2>Count: {{ count }}</h2>     <button @click="increment">+</button>     <button @click="decrement">-</button>   </div> </template>  <script> import { useCounter } from '@/composables/useCounter';  export default {   setup() {     const { count, increment, decrement } = useCounter();     return { count, increment, decrement };   } }; </script>  <style> button {   margin: 5px;   padding: 10px; } </style> ` 
✅ **توضیح کد:** ✔ تابع `useCounter()` را ایمپورت کرده و مقدار `count` و توابع را از آن دریافت می‌کنیم. ✔ در `template` مقدار `count` نمایش داده شده و دکمه‌ها برای افزایش و کاهش مقدار آن استفاده می‌شوند.
  
## **۴. ارسال پارامتر به Composableها**
 
📌 **می‌توانیم مقدار اولیه را به `useCounter` ارسال کنیم.**
 
✅ **ویرایش `useCounter.js` برای دریافت مقدار اولیه:**
 `export function useCounter(initialValue = 0) {   const count = ref(initialValue);    function increment() {     count.value++;   }    function decrement() {     count.value--;   }    return { count, increment, decrement }; } ` 
✅ **استفاده از مقدار اولیه در `Counter.vue`**
 `<script> import { useCounter } from '@/composables/useCounter';  export default {   setup() {     const { count, increment, decrement } = useCounter(10); // مقدار اولیه ۱۰     return { count, increment, decrement };   } }; </script> ` 
✅ **توضیح کد:** ✔ حالا می‌توان هنگام فراخوانی `useCounter(10)` مقدار اولیه را تعیین کرد.
  
## **۵. استفاده از Watch و Computed در Composable**
 
📌 **می‌توان در Composableها از `watch` و `computed` هم استفاده کرد.**
 
✅ **مثال: `useCounter.js` با `computed` و `watch`**
 `import { ref, computed, watch } from 'vue';  export function useCounter(initialValue = 0) {   const count = ref(initialValue);    function increment() {     count.value++;   }    function decrement() {     count.value--;   }    const doubleCount = computed(() => count.value * 2);    watch(count, (newVal, oldVal) => {     console.log(`Count changed from ${oldVal} to ${newVal}`);   });    return { count, increment, decrement, doubleCount }; } ` 
✅ **توضیح کد:** ✔ `doubleCount` مقدار `count` را دو برابر کرده و مقدار آن **واکنش‌گرا (Reactive)** است. ✔ `watch` مقدار `count` را مشاهده کرده و تغییرات آن را در `console` نمایش می‌دهد.
 
✅ **استفاده در `Counter.vue`**
 `<h2>Count: {{ count }}</h2> <h3>Double Count: {{ doubleCount }}</h3> `  
## **۶. ارسال درخواست API در Composableها**
 
📌 **می‌توان از Composableها برای دریافت داده از API استفاده کرد.**
 
✅ **مثال: ایجاد `useFetch.js` برای مدیریت درخواست‌های API**
 `import { ref, onMounted } from 'vue';  export function useFetch(url) {   const data = ref(null);   const error = ref(null);   const loading = ref(true);    async function fetchData() {     try {       const response = await fetch(url);       data.value = await response.json();     } catch (err) {       error.value = err;     } finally {       loading.value = false;     }   }    onMounted(fetchData);    return { data, error, loading, fetchData }; } ` 
✅ **استفاده در `Users.vue`**
 `<template>   <div>     <h2>Users List</h2>     <p v-if="loading">Loading...</p>     <p v-if="error">Error: {{ error }}</p>     <ul v-if="data">       <li v-for="user in data" :key="user.id">{{ user.name }}</li>     </ul>   </div> </template>  <script> import { useFetch } from '@/composables/useFetch';  export default {   setup() {     const { data, error, loading } = useFetch('https://jsonplaceholder.typicode.com/users');     return { data, error, loading };   } }; </script> ` 
✅ **توضیح کد:** ✔ `useFetch.js` یک درخواست `fetch` به URL مشخص‌شده ارسال کرده و داده را واکنش‌گرا (`reactive`) مدیریت می‌کند. ✔ `Users.vue` داده‌های دریافت‌شده را نمایش می‌دهد.
  
## **📌 جمع‌بندی این صفحه**
 
✅ **Composableها توابعی برای مدیریت منطق در Vue 3 هستند.** ✅ **با `ref`، `reactive`، `computed` و `watch` می‌توان منطق را در Composableها مدیریت کرد.** ✅ **Composableها کمک می‌کنند که کامپوننت‌ها ساده‌تر، تمیزتر و ماژولار باشند.** ✅ **می‌توان از Composableها برای ارسال درخواست‌های API استفاده کرد.**
  
