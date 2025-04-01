# **📌 صفحه 13 - مدیریت رویدادهای ناهمگام (Async) در Vue.js**  

در این صفحه، به **مفاهیم ناهمگام (Asynchronous)** در Vue.js می‌پردازیم و روش‌هایی برای **مدیریت درخواست‌های HTTP، توابع async/await** و نحوه‌ی نمایش داده‌های واکشی‌شده را بررسی خواهیم کرد.  

✅ **مبانی برنامه‌نویسی ناهمگام در Vue.js**  
✅ **کار با توابع `async` و `await`**  
✅ **مدیریت درخواست‌های HTTP در Vue.js**  
✅ **مثال عملی: واکشی داده‌ها از API و نمایش آن در کامپوننت**  

---

## **۱. برنامه‌نویسی ناهمگام چیست؟**  

🔹 در **برنامه‌نویسی همگام (Synchronous)** دستورات به ترتیب اجرا می‌شوند و هر خط برنامه باید قبل از اجرای خط بعدی کامل شود.  
🔹 در **برنامه‌نویسی ناهمگام (Asynchronous)**، دستورات می‌توانند بدون متوقف کردن اجرای سایر بخش‌های برنامه اجرا شوند.  

✅ Vue.js از **توابع `async` و `await`** و همچنین از **Promise‌ها** برای مدیریت عملیات ناهمگام استفاده می‌کند.  

---

## **۲. استفاده از `async` و `await` در Vue.js**  

توابع **`async`** به ما امکان می‌دهند که **دستورات ناهمگام** را به صورت **خوانا و خطی** بنویسیم.  

📌 **نمونه‌ی ساده‌ی استفاده از `async` و `await`**  
```javascript
async function fetchData() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts/1')
    const data = await response.json()
    console.log(data)
  } catch (error) {
    console.error('خطا در دریافت داده:', error)
  }
}

fetchData()
```
🔹 `fetchData` یک **تابع ناهمگام** است که داده‌ها را از یک API دریافت می‌کند.  
🔹 `await` درخواست را متوقف می‌کند تا پاسخ از سرور دریافت شود.  
🔹 در صورت بروز خطا، `try...catch` برای مدیریت خطا استفاده می‌شود.  

---

## **۳. مدیریت درخواست‌های HTTP در Vue.js**  

Vue.js برای مدیریت درخواست‌های HTTP از ابزارهایی مانند:  
✅ **fetch API** (ساده و بدون نیاز به کتابخانه اضافی)  
✅ **Axios** (یک کتابخانه محبوب برای درخواست‌های HTTP)  

📌 **مقایسه‌ی Fetch API و Axios**  

| ویژگی | Fetch API | Axios |
|---|---|---|
| پشتیبانی از `async/await` | ✅ | ✅ |
| نیاز به تبدیل پاسخ به JSON | ✅ | ❌ (اتوماتیک) |
| پشتیبانی از `timeout` | ❌ | ✅ |
| مدیریت خطای پیشرفته | ❌ | ✅ |

---

## **۴. مثال: واکشی داده‌ها از API و نمایش آن در یک کامپوننت**  

در این مثال، یک کامپوننت ایجاد می‌کنیم که لیستی از کاربران را از **API** دریافت کرده و نمایش می‌دهد.  

📁 `UsersList.vue`  
```vue
<script setup>
import { ref, onMounted } from 'vue'

const users = ref([])
const loading = ref(true)
const error = ref(null)

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    if (!response.ok) {
      throw new Error('دریافت داده‌ها با مشکل مواجه شد')
    }
    users.value = await response.json()
  } catch (err) {
    error.value = err.message
  } finally {
    loading.value = false
  }
}

onMounted(fetchUsers)
</script>

<template>
  <div>
    <h2>👥 لیست کاربران</h2>
    <p v-if="loading">⏳ در حال بارگیری...</p>
    <p v-if="error" class="error">❌ {{ error }}</p>
    <ul v-if="users.length">
      <li v-for="user in users" :key="user.id">
        {{ user.name }} - {{ user.email }}
      </li>
    </ul>
  </div>
</template>

<style>
.error {
  color: red;
}
</style>
```

✅ **توضیح کد:**  
🔹 از **`ref`** برای مدیریت داده‌های کاربران و وضعیت بارگیری استفاده شده است.  
🔹 **تابع `fetchUsers`** داده‌ها را به صورت ناهمگام واکشی می‌کند.  
🔹 در صورت خطا، پیام خطا در متغیر `error` ذخیره می‌شود.  
🔹 در **`onMounted`** تابع `fetchUsers` اجرا می‌شود تا هنگام بارگذاری کامپوننت، داده‌ها را دریافت کند.  

---

## **📌 جمع‌بندی این صفحه**  

✅ **برنامه‌نویسی ناهمگام** در Vue.js با استفاده از **`async` و `await`** انجام می‌شود.  
✅ **درخواست‌های HTTP** را می‌توان با **Fetch API** یا **Axios** مدیریت کرد.  
✅ در **مثال عملی**، داده‌های کاربران را از API واکشی و در کامپوننت نمایش دادیم.  

---
