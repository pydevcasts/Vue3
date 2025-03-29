### **📌 صفحه ۱۲ - مدیریت درخواست‌های غیرهمزمان (Async & Await) در Vue.js**  

در این صفحه، به بررسی **`async` و `await`** در Vue.js می‌پردازیم. این ویژگی‌ها به ما کمک می‌کنند که **درخواست‌های غیرهمزمان (async requests)** را مدیریت کنیم و تجربه بهتری در کار با **API‌ها، دریافت و ارسال داده‌ها و عملیات زمان‌بر** داشته باشیم.  

---

## **۱. عملیات همزمان (Synchronous) vs. غیرهمزمان (Asynchronous)**  

📌 **مشکل: درخواست‌های API ممکن است چند ثانیه طول بکشند. چطور بدون مسدود شدن صفحه، نتیجه را دریافت کنیم؟**  

🛑 **در عملیات همزمان (Synchronous):**  
- **دستورات پشت سر هم اجرا می‌شوند.**  
- اگر یک درخواست زمان‌بر باشد، **بقیه کدها متوقف می‌شوند.**  

✅ **در عملیات غیرهمزمان (Asynchronous):**  
- درخواست‌های زمان‌بر (مثل **دریافت داده از API**) **صفحه را متوقف نمی‌کنند.**  
- **کدهای بعدی اجرا می‌شوند و نتیجه بعداً دریافت می‌شود.**  

---

## **۲. استفاده از `async` و `await` در Vue.js**  

📌 **در Vue 3 می‌توانیم از `async` و `await` در `setup()` یا داخل متدهای کامپوننت استفاده کنیم.**  

### **📍 ۱. درخواست API با `fetch()` در `setup()`**  

```vue
<template>
  <div>
    <button @click="fetchData">Load Data</button>
    <p v-if="loading">Loading...</p>
    <p v-else>{{ data }}</p>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const data = ref(null);
    const loading = ref(false);

    const fetchData = async () => {
      loading.value = true;
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts/1");
        const result = await response.json();
        data.value = result.title;
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        loading.value = false;
      }
    };

    return { data, loading, fetchData };
  }
};
</script>
```

✅ **در این مثال:**  
- با کلیک روی دکمه، تابع `fetchData()` اجرا می‌شود.  
- مقدار `loading` برای نمایش پیام **"Loading..."** تغییر می‌کند.  
- **از `await fetch(url)` برای دریافت داده‌ها استفاده شده است.**  
- در **بلاک `finally` مقدار `loading` را `false` می‌کنیم** تا پیام "Loading..." ناپدید شود.  

---

## **۳. اجرای درخواست API در `onMounted()`**  

📌 **اگر بخواهیم درخواست داده هنگام بارگذاری صفحه انجام شود، از `onMounted()` استفاده می‌کنیم.**  

```vue
<template>
  <div>
    <p v-if="loading">Loading...</p>
    <p v-else>{{ data }}</p>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';

export default {
  setup() {
    const data = ref(null);
    const loading = ref(false);

    const fetchData = async () => {
      loading.value = true;
      try {
        const response = await fetch("https://jsonplaceholder.typicode.com/posts/2");
        const result = await response.json();
        data.value = result.title;
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        loading.value = false;
      }
    };

    onMounted(fetchData); // درخواست هنگام بارگذاری کامپوننت اجرا شود

    return { data, loading };
  }
};
</script>
```

✅ **در این مثال:**  
- درخواست API در `onMounted()` اجرا می‌شود.  
- **نیازی به کلیک دکمه نیست و داده هنگام بارگذاری صفحه دریافت می‌شود.**  

---

## **۴. مدیریت چند درخواست همزمان با `Promise.all()`**  

📌 **گاهی نیاز داریم همزمان چندین API را درخواست کنیم و منتظر دریافت همه آن‌ها باشیم.**  

### **📍 ۳. درخواست همزمان چند API و نمایش داده‌ها**  

```vue
<template>
  <div>
    <button @click="fetchAllData">Load All Data</button>
    <p v-if="loading">Loading...</p>
    <ul v-else>
      <li v-for="(item, index) in data" :key="index">{{ item.title }}</li>
    </ul>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const data = ref([]);
    const loading = ref(false);

    const fetchAllData = async () => {
      loading.value = true;
      try {
        const [res1, res2] = await Promise.all([
          fetch("https://jsonplaceholder.typicode.com/posts/1").then(res => res.json()),
          fetch("https://jsonplaceholder.typicode.com/posts/2").then(res => res.json())
        ]);
        data.value = [res1, res2];
      } catch (error) {
        console.error("Error fetching data:", error);
      } finally {
        loading.value = false;
      }
    };

    return { data, loading, fetchAllData };
  }
};
</script>
```

✅ **در این مثال:**  
- **از `Promise.all()` برای اجرای همزمان دو درخواست API استفاده شده است.**  
- **فقط زمانی که هر دو درخواست دریافت شوند، داده‌ها نمایش داده می‌شوند.**  

---

## **۵. خطایابی درخواست‌های `async` در Vue.js**  

📌 **چگونه خطاهای درخواست‌های `async` را مدیریت کنیم؟**  
1️⃣ **استفاده از `try...catch` برای مدیریت خطاها**  
2️⃣ **نمایش پیام خطا برای کاربران**  

```vue
<template>
  <div>
    <button @click="fetchData">Fetch Data</button>
    <p v-if="loading">Loading...</p>
    <p v-if="error" style="color: red;">{{ error }}</p>
    <p v-else>{{ data }}</p>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const data = ref(null);
    const loading = ref(false);
    const error = ref(null);

    const fetchData = async () => {
      loading.value = true;
      error.value = null;
      try {
        const response = await fetch("https://invalid-url.com/api");
        if (!response.ok) throw new Error("Failed to fetch data");
        const result = await response.json();
        data.value = result.title;
      } catch (err) {
        error.value = err.message;
      } finally {
        loading.value = false;
      }
    };

    return { data, loading, error, fetchData };
  }
};
</script>
```

✅ **در این مثال:**  
- اگر درخواست موفق نباشد، **پیام خطا نمایش داده می‌شود.**  
- **متغیر `error` برای نمایش پیام خطا در UI استفاده شده است.**  

---

## **📌 جمع‌بندی این صفحه**  

✅ `async` و `await` برای مدیریت درخواست‌های غیرهمزمان استفاده می‌شوند.  
✅ از `fetch()` برای دریافت داده از APIها استفاده می‌کنیم.  
✅ می‌توان درخواست API را **هنگام بارگذاری صفحه (`onMounted`) یا با کلیک دکمه** اجرا کرد.  
✅ برای اجرای همزمان چند درخواست، **از `Promise.all()` استفاده می‌کنیم.**  
✅ با `try...catch` می‌توان **خطاها را مدیریت و به کاربر نمایش داد.**  

