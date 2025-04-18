### **📌 شروع کار با Vue.js**  

در این بخش، وارد دنیای عملی Vue.js می‌شویم و اولین پروژه خود را با این فریمورک راه‌اندازی می‌کنیم. ابتدا **نصب Vue.js** را یاد می‌گیریم و سپس یک مثال ساده از **ساختار کلی یک پروژه Vue.js** را بررسی خواهیم کرد.  

---

### **۱. راه‌های مختلف نصب Vue.js**  

برای استفاده از Vue.js، چندین روش وجود دارد. در اینجا، سه روش رایج را معرفی می‌کنیم:  

#### **🔹 روش ۱: استفاده از CDN (ساده‌ترین روش برای شروع)**  
اگر می‌خواهید Vue.js را بدون نیاز به تنظیمات خاص اجرا کنید، می‌توانید آن را از طریق **CDN** در فایل HTML خود اضافه کنید.  

📌 **کد نمونه برای افزودن Vue.js از طریق CDN:**  

```html
<!DOCTYPE html>
<html lang="fa">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>اولین پروژه Vue.js</title>
    <!-- اضافه کردن Vue.js از CDN -->
    <script src="https://unpkg.com/vue@3/dist/vue.global.js"></script>
</head>
<body>
    <div id="app">
        <h1>{{ message }}</h1>
    </div>

    <script>
        const app = Vue.createApp({
            data() {
                return {
                    message: "سلام Vue.js!"
                };
            }
        });
        app.mount('#app');
    </script>
</body>
</html>
```

✅ **نتیجه:** وقتی این کد را در مرورگر اجرا کنید، پیغام **"سلام Vue.js!"** در صفحه نمایش داده می‌شود.

---

#### **🔹 روش ۲: نصب Vue.js با NPM (مناسب برای پروژه‌های بزرگ‌تر)**  
اگر قصد دارید یک پروژه **حرفه‌ای و ساختاریافته** داشته باشید، بهتر است از **Node.js و NPM** استفاده کنید.

📌 **مراحل نصب Vue.js با NPM:**  

۱️⃣ ابتدا **Node.js** را نصب کنید. (می‌توانید از [سایت رسمی Node.js](https://nodejs.org/) آن را دانلود کنید.)  
۲️⃣ سپس، در ترمینال دستور زیر را اجرا کنید تا Vue.js را نصب کنید:

```sh
npm create vue@latest my-vue-app
cd my-vue-app
npm install
npm run dev
```

✅ حالا می‌توانید برنامه را در مرورگر مشاهده کنید.

---

#### **🔹 روش ۳: استفاده از Vite (سریع‌ترین روش برای پروژه‌های Vue 3)**  
**Vite** یک ابزار مدرن برای **ساخت پروژه‌های Vue.js 3** است که سرعت بیشتری نسبت به Webpack دارد.

📌 **مراحل راه‌اندازی پروژه با Vite:**  

```sh
npm create vite@latest my-vue-app --template vue
cd my-vue-app
npm install
npm run dev
```

✅ حالا سرور لوکال اجرا شده و می‌توانید برنامه را ببینید.

---

### **۲. ساختار کلی یک پروژه Vue.js**  

بعد از نصب، نگاهی به **فایل‌های مهم** در یک پروژه Vue.js خواهیم داشت.  

📂 **ساختار کلی پروژه:**  

```
my-vue-app/
│── node_modules/       # وابستگی‌های پروژه
│── public/             # فایل‌های استاتیک مانند تصاویر و فونت‌ها
│── src/                # کد اصلی پروژه
│   │── assets/         # فایل‌های CSS و تصاویر
│   │── components/     # کامپوننت‌های Vue
│   │── App.vue         # کامپوننت اصلی برنامه
│   │── main.js         # فایل اصلی برای اجرا
│── index.html          # صفحه اصلی پروژه
│── package.json        # اطلاعات پروژه و وابستگی‌ها
│── vite.config.js      # تنظیمات Vite
```

📌 **فایل‌های کلیدی:**  
✔ **`main.js`** → نقطه شروع پروژه و اتصال Vue.js به HTML  
✔ **`App.vue`** → کامپوننت اصلی برنامه  
✔ **`components/`** → جایی که کامپوننت‌های مختلف Vue را می‌سازیم  

---

### **۳. ایجاد اولین کامپوننت در Vue.js**  

در Vue.js همه چیز بر اساس **کامپوننت‌ها** ساخته می‌شود. حالا یک **کامپوننت ساده** ایجاد می‌کنیم که نام کاربر را نمایش دهد.

📂 در مسیر **`src/components/HelloUser.vue`** یک فایل جدید با این محتوا ایجاد کنید:

```vue
<template>
  <div>
    <h2>سلام {{ name }}!</h2>
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "علی"
    };
  }
};
</script>

<style scoped>
h2 {
  color: blue;
}
</style>
```

✅ این کامپوننت مقدار **`name`** را نمایش می‌دهد.

---

### **۴. اضافه کردن کامپوننت به `App.vue`**  

حالا برای نمایش این کامپوننت، آن را به `App.vue` اضافه می‌کنیم:

```vue
<template>
  <div>
    <h1>به Vue.js خوش آمدید!</h1>
    <HelloUser />
  </div>
</template>

<script>
import HelloUser from './components/HelloUser.vue';

export default {
  components: {
    HelloUser
  }
};
</script>
```

✅ بعد از اجرای پروژه، پیام خوشامدگویی و نام **"علی"** نمایش داده خواهد شد.

---

### **📌 جمع‌بندی این صفحه**  

✅ Vue.js را می‌توان از طریق **CDN، NPM یا Vite** نصب کرد.  
✅ ساختار کلی پروژه Vue.js شامل **کامپوننت‌ها، App.vue، main.js و...** است.  
✅ کامپوننت‌ها هسته اصلی Vue.js هستند و می‌توان آن‌ها را به پروژه اضافه کرد.  

🚀 **در صفحه بعد، با **ری‌اکتیو بودن داده‌ها** در Vue.js آشنا می‌شویم و یاد می‌گیریم چطور داده‌ها را به‌صورت پویا تغییر دهیم!**  
