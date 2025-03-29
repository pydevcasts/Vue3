### **📌 مفاهیم پیشرفته در Vue.js: Teleport, Provide & Inject, Mixins**  

در این صفحه، به سراغ مفاهیم پیشرفته‌تر Vue.js می‌رویم که به شما کمک می‌کنند اپلیکیشن‌های مقیاس‌پذیرتر و سازمان‌یافته‌تر ایجاد کنید.  

---

## **۱. استفاده از `Teleport` در Vue.js**  

📌 **`Teleport` چیست؟**  
گاهی اوقات لازم است یک بخش از کامپوننت را **به خارج از ساختار اصلی DOM** منتقل کنیم. برای مثال، ممکن است بخواهیم یک `modal` یا `tooltip` را **مستقیماً داخل `<body>`** قرار دهیم تا در سطح بالاتری از DOM باشد.  

✅ برای این کار از **`<Teleport>`** استفاده می‌کنیم.  

📌 **مثال: نمایش یک مودال در `<body>`**  

```vue
<template>
  <button @click="showModal = true">نمایش مودال</button>

  <Teleport to="body">
    <div v-if="showModal" class="modal">
      <div class="modal-content">
        <h2>عنوان مودال</h2>
        <p>این یک محتوای مودال است.</p>
        <button @click="showModal = false">بستن</button>
      </div>
    </div>
  </Teleport>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const showModal = ref(false);
    return { showModal };
  }
};
</script>

<style>
.modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-content {
  background: white;
  padding: 20px;
  border-radius: 5px;
  text-align: center;
}
</style>
```

✅ **نتیجه:** دکمه‌ای برای نمایش مودال داریم که وقتی کلیک شود، مودال مستقیماً به `<body>` اضافه شده و نمایش داده می‌شود.

---

## **۲. مدیریت داده‌های سراسری با `Provide & Inject`**  

📌 **`Provide & Inject` چیست؟**  
گاهی اوقات می‌خواهیم داده‌ای را از یک **کامپوننت والد (Parent)** به کامپوننت‌های **چندین سطح پایین‌تر (Child Components)** منتقل کنیم، بدون اینکه مجبور شویم `props` را در هر سطح ارسال کنیم.  

✅ **برای این کار از `provide` و `inject` استفاده می‌کنیم.**  

📌 **مثال: ارسال داده از والد به فرزند**  

### **📍 کامپوننت والد (App.vue):**  

```vue
<template>
  <div>
    <h2>نام کاربر: {{ userName }}</h2>
    <ChildComponent />
  </div>
</template>

<script>
import { provide, ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent },
  setup() {
    const userName = ref("علی");
    provide("userName", userName); // ارسال مقدار به فرزندان
    return { userName };
  }
};
</script>
```

### **📍 کامپوننت فرزند (`ChildComponent.vue`):**  

```vue
<template>
  <h3>نام دریافت شده در فرزند: {{ receivedUserName }}</h3>
</template>

<script>
import { inject } from 'vue';

export default {
  setup() {
    const receivedUserName = inject("userName"); // دریافت مقدار از والد
    return { receivedUserName };
  }
};
</script>
```

✅ **نتیجه:** مقدار `userName` از **والد به فرزند ارسال شده است، بدون استفاده از `props`.**

🔹 **نکته:** `Provide & Inject` زمانی مفید است که داده‌ها به چندین کامپوننت تو در تو نیاز باشد، مثلاً در **مدیریت تم‌ها، تنظیمات زبان و اطلاعات کاربر.**  

---

## **۳. استفاده از `Mixins` برای مدیریت کدهای تکراری**  

📌 **`Mixins` چیست؟**  
در برنامه‌های بزرگ ممکن است **کدهای مشترکی** در چندین کامپوننت داشته باشیم. برای جلوگیری از تکرار، این منطق را در یک **Mixin** قرار داده و در کامپوننت‌های مختلف استفاده می‌کنیم.  

📌 **مثال: یک Mixin برای مدیریت نام کاربر**  

### **📍 ایجاد فایل `userMixin.js`**  

```javascript
import { ref } from 'vue';

export default {
  setup() {
    const userName = ref("زهرا");
    
    function changeName(newName) {
      userName.value = newName;
    }

    return { userName, changeName };
  }
};
```

### **📍 استفاده از `Mixin` در یک کامپوننت**  

```vue
<template>
  <div>
    <h2>نام کاربر: {{ userName }}</h2>
    <button @click="changeName('محمد')">تغییر نام</button>
  </div>
</template>

<script>
import userMixin from './userMixin.js';

export default {
  setup() {
    return userMixin.setup();
  }
};
</script>
```

✅ **نتیجه:** مقدار `userName` و تابع `changeName` در چندین کامپوننت بدون نیاز به کپی کردن کد استفاده می‌شود.

---

## **📌 جمع‌بندی این صفحه**  

✅ **`Teleport`** به ما کمک می‌کند که برخی از بخش‌های کامپوننت را به سطح بالاتر در DOM منتقل کنیم، مانند مودال‌ها و تولتیپ‌ها.  
✅ **`Provide & Inject`** امکان ارسال داده از **کامپوننت والد به فرزندان چند سطح پایین‌تر** را فراهم می‌کند، بدون استفاده از `props`.  
✅ **`Mixins`** برای **مدیریت کدهای تکراری** و استفاده مجدد از توابع در کامپوننت‌های مختلف کاربرد دارد.  
