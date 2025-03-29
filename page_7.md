### **📌 صفحه ۷ - مفهوم Slots و مدیریت بسته‌ها (Packages) در Vue.js**  

در این صفحه، دو مفهوم مهم را بررسی خواهیم کرد:  
✅ استفاده از **Slots** برای ایجاد کامپوننت‌های انعطاف‌پذیر  
✅ نحوه مدیریت **Packages** و نصب پکیج‌های مورد نیاز در Vue.js  

---

## **۱. استفاده از Slots در Vue.js**  

📌 **`Slots` چیست؟**  
`Slots` به ما این امکان را می‌دهد که **محتوای دلخواهی را از والد به فرزند ارسال کنیم** و بدون نیاز به `props`، از ساختارهای انعطاف‌پذیر استفاده کنیم.  

✅ **کاربردهای `Slots`**  
- ایجاد **کامپوننت‌های قابل استفاده مجدد** مانند `Card`، `Modal` و `Tabs`  
- **ارسال محتوای دلخواه از والد به فرزند** بدون محدودیت  
- **مدیریت بهتر استایل و ساختار HTML** در کامپوننت‌ها  

📌 **مثال: ایجاد یک کامپوننت `Card` با استفاده از Slots**  

### **📍 ایجاد `CardComponent.vue`**  

```vue
<template>
  <div class="card">
    <h2><slot name="title"></slot></h2>
    <p><slot></slot></p>
  </div>
</template>

<style>
.card {
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 2px 2px 10px rgba(0, 0, 0, 0.1);
}
</style>
```

### **📍 استفاده از `CardComponent` در `App.vue`**  

```vue
<template>
  <CardComponent>
    <template #title>Vue.js Slots</template>
    این یک مثال برای استفاده از Slots در Vue.js است.
  </CardComponent>
</template>

<script>
import CardComponent from './CardComponent.vue';

export default {
  components: { CardComponent }
};
</script>
```

✅ **نتیجه:**  
- `slot name="title"` مقدار `<template #title>` را نمایش می‌دهد.  
- `slot` بدون نام، سایر محتوا را نمایش می‌دهد.  

---

## **۲. مدیریت بسته‌ها (Packages) در Vue.js**  

📌 **چرا به `Packages` نیاز داریم؟**  
بسته‌ها (Packages) مجموعه‌ای از کتابخانه‌ها و ابزارهای اضافی هستند که برای **افزودن ویژگی‌های خاص** به Vue.js استفاده می‌شوند.  

✅ **کاربردهای `Packages`**  
- استفاده از **UI Libraries** مانند Vuetify و Element Plus  
- مدیریت درخواست‌های API با **Axios**  
- استفاده از **Vue Router** برای مسیر‌یابی  
- مدیریت حالت با **Pinia یا Vuex**  

---

### **📍 نصب و مدیریت بسته‌ها در Vue.js**  

📌 **نصب پکیج با `npm` یا `yarn`**  
برای نصب یک پکیج، از `npm` یا `yarn` استفاده می‌کنیم:  

```bash
npm install axios
```
یا  
```bash
yarn add axios
```

📌 **حذف یک پکیج**  
اگر بخواهید پکیجی را حذف کنید:  

```bash
npm uninstall axios
```
یا  
```bash
yarn remove axios
```

📌 **مثال: استفاده از `Axios` برای دریافت داده‌ها از API**  

### **📍 نصب Axios**  

```bash
npm install axios
```

### **📍 دریافت داده‌ها از API در `App.vue`**  

```vue
<template>
  <div>
    <h2>Users List</h2>
    <ul>
      <li v-for="user in users" :key="user.id">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue';
import axios from 'axios';

export default {
  setup() {
    const users = ref([]);

    onMounted(async () => {
      const response = await axios.get('https://jsonplaceholder.typicode.com/users');
      users.value = response.data;
    });

    return { users };
  }
};
</script>
```

✅ **نتیجه:** پس از بارگذاری صفحه، اطلاعات کاربران از API دریافت شده و نمایش داده می‌شود.  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`Slots`** برای **ایجاد کامپوننت‌های انعطاف‌پذیر** و ارسال محتوای دلخواه از والد به فرزند استفاده می‌شود.  
✅ **`Packages`** در Vue.js به ما کمک می‌کنند تا امکانات اضافی مانند **Axios، Vue Router و Pinia** را به پروژه خود اضافه کنیم.  
✅ **`npm` و `yarn`** ابزارهای مدیریت بسته‌ها هستند که امکان **نصب، حذف و به‌روزرسانی پکیج‌ها** را فراهم می‌کنند.  

