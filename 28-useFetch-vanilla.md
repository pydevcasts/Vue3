### **📌 صفحه 28 - پیاده‌سازی `useFetch` بدون Vue.js (Vanilla JavaScript)**  

در این صفحه، نحوه پیاده‌سازی `useFetch` بدون استفاده از Vue.js را بررسی می‌کنیم. این روش در **Vanilla JavaScript** پیاده‌سازی شده و می‌توان از آن در هر پروژه‌ای استفاده کرد.  

---

## **۱. پیاده‌سازی `useFetch` بدون Vue.js**  

📌 در Vue 3 ما از **`ref`** و **`reactive`** برای ایجاد **متغیرهای واکنش‌گرا** استفاده می‌کنیم، اما در **Vanilla JavaScript** این مفهوم وجود ندارد.  
✅ در اینجا از `useState` ساده با `async/await` برای **مدیریت درخواست‌های API** استفاده می‌کنیم.  

✅ **ایجاد فایل `useFetch.js` در پروژه**  

```js
export function useFetch(url) {
  let data = null;
  let error = null;
  let loading = true;

  async function fetchData() {
    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      data = await response.json();
    } catch (err) {
      error = err.message;
    } finally {
      loading = false;
    }
  }

  return { data, error, loading, fetchData };
}
```

✅ **توضیح کد:**  
✔ `data`: داده دریافت‌شده از API  
✔ `error`: پیام خطا در صورت وجود  
✔ `loading`: وضعیت در حال دریافت اطلاعات  
✔ `fetchData()`: تابعی که درخواست API را ارسال می‌کند  

---

## **۲. استفاده از `useFetch` در یک پروژه بدون Vue.js**  

📌 در اینجا، این تابع را در یک فایل HTML ساده استفاده می‌کنیم.  

✅ **ایجاد `index.html`**  

```html
<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>useFetch بدون Vue</title>
</head>
<body>
    <h2>لیست کاربران</h2>
    <button id="fetchButton">دریافت اطلاعات</button>
    <p id="loadingText">در حال بارگذاری...</p>
    <p id="errorText" style="color: red;"></p>
    <ul id="userList"></ul>

    <script type="module">
        import { useFetch } from './useFetch.js';

        document.getElementById('fetchButton').addEventListener('click', async () => {
            const { data, error, loading, fetchData } = useFetch('https://jsonplaceholder.typicode.com/users');

            document.getElementById('loadingText').style.display = 'block';
            document.getElementById('errorText').textContent = '';

            await fetchData();

            if (error) {
                document.getElementById('errorText').textContent = error;
            } else {
                document.getElementById('userList').innerHTML = data.map(user => `<li>${user.name}</li>`).join('');
            }

            document.getElementById('loadingText').style.display = 'none';
        });
    </script>
</body>
</html>
```

✅ **توضیح کد:**  
✔ هنگام کلیک روی **دکمه "دریافت اطلاعات"**، تابع `fetchData()` اجرا می‌شود.  
✔ داده‌های دریافتی در یک `ul` نمایش داده می‌شوند.  
✔ در صورت وجود خطا، پیام خطا نمایش داده می‌شود.  
✔ `loadingText` وضعیت در حال بارگذاری را نشان می‌دهد.  

---

## **۳. مقایسه `useFetch` در Vue.js و Vanilla JavaScript**  

| ویژگی | Vue.js | Vanilla JavaScript |
|--------|--------|------------------|
| **واکنش‌گرایی** | `ref` و `reactive` | متغیرهای معمولی |
| **مدیریت وضعیت (State Management)** | `ref`, `computed`, `watch` | متغیر ساده |
| **چرخه عمر (Lifecycle)** | `onMounted()` | اجرای تابع به‌صورت دستی |
| **نمایش داده در UI** | قالب `template` | `document.getElementById()` |

📌 **نتیجه:**  
✅ در Vue.js، `useFetch` با `ref` و `onMounted()` واکنش‌گرا می‌شود.  
✅ در Vanilla JavaScript، باید از **`fetch`، `async/await` و `document API`** استفاده کنیم.  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`useFetch` در Vanilla JavaScript قابل پیاده‌سازی است.**  
✅ **برای دریافت داده‌ها از `fetch` و `async/await` استفاده می‌کنیم.**  
✅ **مدیریت UI بدون Vue.js باید با `document API` انجام شود.**  
✅ **در Vue.js `useFetch` به کمک `ref` و `onMounted()` واکنش‌گرا می‌شود.**  

---
