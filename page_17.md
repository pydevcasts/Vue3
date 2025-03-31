### **📌 صفحه ۱۷ - مدیریت مسیرها با Vue Router در Vue 3**  

در این صفحه، نحوه **مدیریت مسیرها (Routing)** در Vue 3 را بررسی می‌کنیم و یاد می‌گیریم که چگونه با استفاده از **Vue Router**، مسیرهای مختلفی برای صفحات و کامپوننت‌های خود تعریف کنیم.  

---

## **۱. مقدمه‌ای بر Vue Router**  

📌 Vue Router یک **کتابخانه رسمی برای مدیریت مسیرها در Vue.js** است که امکان ایجاد **مسیرهای چندصفحه‌ای (SPA - Single Page Application)** را فراهم می‌کند.  

✅ **ویژگی‌های Vue Router:**  
✔ مسیریابی بین صفحات بدون نیاز به بارگذاری مجدد (`SPA`).  
✔ امکان استفاده از **پارامترها در آدرس (`params` و `query`)**.  
✔ پشتیبانی از **مسیرهای پویا (Dynamic Routing)**.  
✔ استفاده از **حالت `history` یا `hash` برای مدیریت URL**.  
✔ پشتیبانی از **مسیرهای تو در تو (Nested Routes)**.  
✔ امکان **محافظت از مسیرها (`Navigation Guards`)**.  

---

## **۲. نصب و راه‌اندازی Vue Router**  

📌 اگر Vue Router را هنگام ایجاد پروژه Vue 3 نصب نکرده‌اید، می‌توانید با دستور زیر آن را نصب کنید:  

```sh
npm install vue-router
```

✅ سپس، فایل مسیرهای برنامه را ایجاد کرده و Vue Router را تنظیم می‌کنیم.  

---

## **۳. تعریف و پیکربندی مسیرها در Vue 3**  

📌 ابتدا در پوشه `src` یک فایل جدید به نام `router.js` یا `router/index.js` ایجاد کنید و مسیرهای برنامه را در آن تعریف کنید.  

✅ **مثال: تنظیم مسیرهای اولیه در `router/index.js`**  

```js
import { createRouter, createWebHistory } from 'vue-router';
import Home from '../views/Home.vue';
import About from '../views/About.vue';

const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
];

const router = createRouter({
  history: createWebHistory(),
  routes
});

export default router;
```

✅ **توضیح کد:**  
✔ مسیر `/` صفحه `Home.vue` را نمایش می‌دهد.  
✔ مسیر `/about` صفحه `About.vue` را نمایش می‌دهد.  
✔ `createWebHistory()` از **History API مرورگر** برای مدیریت مسیرها استفاده می‌کند.  

---

## **۴. اضافه کردن Vue Router به پروژه Vue 3**  

📌 پس از تعریف مسیرها، باید `router` را در **`main.js`** تنظیم کنیم:  

✅ **ویرایش `main.js` برای افزودن Vue Router:**  

```js
import { createApp } from 'vue';
import App from './App.vue';
import router from './router'; // اضافه کردن فایل روتر

const app = createApp(App);
app.use(router); // ثبت روتر در برنامه
app.mount('#app');
```

✅ **توضیح کد:**  
✔ روتر را در Vue ثبت کرده و از آن در کل پروژه استفاده می‌کنیم.  

---

## **۵. نحوه استفاده از `<router-view>` و `<router-link>`**  

📌 در فایل `App.vue` باید `router-view` را قرار دهیم تا **کامپوننت‌های مربوط به مسیرهای مختلف نمایش داده شوند**.  

✅ **ویرایش `App.vue` برای نمایش مسیرها:**  

```vue
<template>
  <div>
    <nav>
      <router-link to="/">Home</router-link>
      <router-link to="/about">About</router-link>
    </nav>
    <router-view></router-view>
  </div>
</template>

<style>
nav a {
  margin-right: 15px;
  text-decoration: none;
  color: blue;
}
</style>
```

✅ **توضیح کد:**  
✔ **`<router-view>`** کامپوننت مربوط به مسیر فعلی را نمایش می‌دهد.  
✔ **`<router-link>`** برای جابه‌جایی بین صفحات بدون بارگذاری مجدد استفاده می‌شود.  

---

## **۶. مسیرهای پویا (Dynamic Routes) و پارامترها**  

📌 گاهی نیاز داریم که مسیرها دارای **پارامتر متغیر** باشند، مثل نمایش اطلاعات یک کاربر خاص (`/user/5`).  

✅ **تعریف مسیرهای پویا در `router/index.js`**  

```js
const routes = [
  { path: '/user/:id', component: UserProfile },
];
```

✅ **دسترسی به مقدار `id` در `UserProfile.vue`**  

```vue
<template>
  <div>
    <h2>User ID: {{ userId }}</h2>
  </div>
</template>

<script>
import { useRoute } from 'vue-router';

export default {
  setup() {
    const route = useRoute();
    return { userId: route.params.id };
  }
};
</script>
```

✅ **توضیح کد:**  
✔ مقدار `id` را از مسیر استخراج کرده و نمایش می‌دهیم.  

---

## **۷. مسیرهای تو در تو (Nested Routes)**  

📌 برخی مسیرها دارای زیرمسیر (Subroute) هستند، مانند یک داشبورد با چندین بخش (`/dashboard/profile`, `/dashboard/settings`).  

✅ **تعریف مسیرهای تو در تو در `router/index.js`**  

```js
const routes = [
  {
    path: '/dashboard',
    component: Dashboard,
    children: [
      { path: 'profile', component: Profile },
      { path: 'settings', component: Settings },
    ],
  },
];
```

✅ **در `Dashboard.vue` باید `<router-view>` اضافه شود تا زیرمسیرها نمایش داده شوند.**  

```vue
<template>
  <div>
    <h1>Dashboard</h1>
    <router-link to="/dashboard/profile">Profile</router-link>
    <router-link to="/dashboard/settings">Settings</router-link>
    <router-view></router-view>
  </div>
</template>
```

✅ **توضیح کد:**  
✔ `Dashboard` دارای مسیرهای داخلی برای `Profile` و `Settings` است.  
✔ `<router-view>` مسیر فرعی فعال را نمایش می‌دهد.  

---

## **۸. محافظت از مسیرها (Navigation Guards)**  

📌 گاهی نیاز داریم که دسترسی به برخی مسیرها را محدود کنیم، مثل **مسیرهای مخصوص کاربران لاگین‌شده**.  

✅ **ایجاد یک گارد برای محافظت از مسیرهای خاص:**  

```js
router.beforeEach((to, from, next) => {
  const isAuthenticated = false; // مقدار واقعی را از سیستم احراز هویت بگیرید
  if (to.path === '/dashboard' && !isAuthenticated) {
    next('/login'); // اگر کاربر لاگین نیست، به صفحه لاگین هدایت شود
  } else {
    next();
  }
});
```

✅ **توضیح کد:**  
✔ قبل از تغییر مسیر، چک می‌شود که آیا کاربر احراز هویت شده است یا خیر.  
✔ اگر لاگین نباشد و قصد ورود به `dashboard` را داشته باشد، به صفحه لاگین هدایت می‌شود.  

---

## **📌 جمع‌بندی این صفحه**  

✅ Vue Router امکان مدیریت مسیرهای مختلف در Vue را فراهم می‌کند.  
✅ از `createRouter` و `createWebHistory` برای تعریف مسیرها استفاده می‌شود.  
✅ با `router-view` و `router-link` می‌توان مسیرها را نمایش و جابه‌جا کرد.  
✅ مسیرهای پویا (`Dynamic Routes`) و تو در تو (`Nested Routes`) را می‌توان مدیریت کرد.  
✅ می‌توان از **گاردهای مسیریابی (`Navigation Guards`)** برای کنترل دسترسی به مسیرها استفاده کرد.  
