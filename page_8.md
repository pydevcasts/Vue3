### **📌 صفحه ۸ - آشنایی با Vite و مزایای آن در Vue.js**  

در این صفحه، درباره‌ی **Vite** و اینکه چرا جایگزین **Vue CLI** شده است، صحبت خواهیم کرد. همچنین نحوه‌ی **نصب و راه‌اندازی یک پروژه Vue.js با Vite** را بررسی خواهیم کرد.  

---

## **۱. Vite چیست و چرا از آن استفاده کنیم؟**  

📌 **Vite** یک ابزار **Build** و **Dev Server** مدرن برای پروژه‌های **Vue.js، React و Vanilla JavaScript** است که جایگزین **Vue CLI** شده است.  

✅ **چرا Vite بهتر از Vue CLI است؟**  
- 🚀 **سرعت فوق‌العاده بالا** در اجرا و Build  
- 🛠 **Hot Module Replacement (HMR) سریع** برای به‌روزرسانی سریع کد بدون نیاز به رفرش صفحه  
- 📦 **پشتیبانی از ES Modules** برای بهینه‌سازی کد  
- 🔥 **سازگاری بالا با Vue 3 و TypeScript**  
- 🏗 **کاهش زمان کامپایل و ساخت (Build Time)**  

---

## **۲. نصب و راه‌اندازی پروژه Vue.js با Vite**  

📌 **چگونه یک پروژه Vue.js را با Vite ایجاد کنیم؟**  

✅ **گام ۱: ایجاد پروژه جدید با Vite**  
دستور زیر را در **ترمینال** اجرا کنید:  

```bash
npm create vite@latest my-vue-app --template vue
```

✅ **گام ۲: ورود به پوشه‌ی پروژه**  

```bash
cd my-vue-app
```

✅ **گام ۳: نصب وابستگی‌های پروژه**  

```bash
npm install
```

✅ **گام ۴: اجرای پروژه**  

```bash
npm run dev
```

🔹 **پس از اجرای این دستور، Vite یک سرور توسعه اجرا کرده و پروژه در `http://localhost:5173` در دسترس خواهد بود.**  

---

## **۳. بررسی ساختار پروژه در Vite**  

بعد از اجرای دستورات بالا، ساختار پروژه به شکل زیر خواهد بود:  

```
my-vue-app
│── node_modules/
│── public/
│── src/
│   ├── assets/
│   ├── components/
│   ├── App.vue
│   ├── main.js
│── index.html
│── package.json
│── vite.config.js
│── README.md
```

✅ **تفاوت‌های کلیدی Vite با Vue CLI:**  
- در **Vite**، فایل `vite.config.js` به جای `vue.config.js` استفاده می‌شود.  
- نیازی به تنظیمات پیچیده Webpack نیست.  
- **سرعت اجرای پروژه بسیار بالاتر است!**  

---

## **۴. تنظیمات `vite.config.js` در Vue.js**  

📌 **فایل `vite.config.js` برای پیکربندی تنظیمات Vite استفاده می‌شود.**  

مثال: **فعال‌سازی Alias در Vite**  

```js
import { defineConfig } from 'vite';
import vue from '@vitejs/plugin-vue';

export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': '/src'
    }
  }
});
```

✅ **نتیجه:**  
اکنون می‌توانیم مسیر فایل‌ها را به این صورت وارد کنیم:  

```js
import MyComponent from '@/components/MyComponent.vue';
```

---

## **۵. نحوه‌ی اضافه کردن پکیج‌ها به پروژه Vite**  

📌 **همانند Vue CLI، می‌توان از `npm` یا `yarn` برای نصب پکیج‌ها استفاده کرد.**  

✅ **مثال: نصب `Axios` در Vite**  

```bash
npm install axios
```

✅ **مثال: استفاده از `Axios` در پروژه**  

```js
import axios from 'axios';

axios.get('https://jsonplaceholder.typicode.com/posts')
  .then(response => console.log(response.data));
```

---

## **📌 جمع‌بندی این صفحه**  

✅ **Vite** یک **Dev Server سریع** و جایگزین Vue CLI است که مزایای زیادی دارد.  
✅ **با `npm create vite@latest my-vue-app --template vue` می‌توان یک پروژه Vue.js را با Vite ایجاد کرد.**  
✅ **ساختار پروژه در Vite ساده‌تر و عملکرد آن بهتر از Vue CLI است.**  
✅ **فایل `vite.config.js` برای مدیریت تنظیمات پروژه استفاده می‌شود.**  
✅ **با `npm install` می‌توان پکیج‌های مختلف را به پروژه Vite اضافه کرد.**  
