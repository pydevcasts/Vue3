### **📌 صفحه ۱۳ - استفاده از `Teleport` در Vue.js**  

در این صفحه، به بررسی **`Teleport`** در Vue.js 3 می‌پردازیم. **`Teleport`** به ما امکان می‌دهد که محتوای یک کامپوننت را **خارج از سلسله‌مراتب معمول DOM** قرار دهیم. این ویژگی زمانی مفید است که بخواهیم **المان‌هایی مانند مودال‌ها (modals)، اعلان‌ها (notifications) یا منوهای کشویی (dropdowns)** را در بخش‌های خاصی از صفحه مانند **`<body>`** رندر کنیم.  

---

## **۱. `Teleport` چیست و چرا مهم است؟**  

📌 در Vue.js، تمام عناصر معمولاً درون یک **کامپوننت والد (parent component)** قرار می‌گیرند. اما گاهی **بهتر است برخی از عناصر مستقیماً داخل `<body>` قرار گیرند** تا مشکلات **z-index، استایل‌دهی و نمایش صحیح در بالاترین سطح DOM** را حل کنیم.  

✅ **مثال کاربردی:** مودال‌هایی که در یک **کامپوننت داخلی** تعریف شده‌اند، اما می‌خواهیم آن‌ها را خارج از محدوده همان کامپوننت و مستقیماً در `<body>` نمایش دهیم.  

---

## **۲. استفاده از `Teleport` در Vue.js**  

📌 **مراحل پیاده‌سازی یک مودال با `Teleport`**  

✅ **مرحله ۱: افزودن `Teleport` در کامپوننت مودال**  

**📍 فایل `Modal.vue`**  

```vue
<template>
  <teleport to="body">
    <div v-if="isOpen" class="modal-overlay">
      <div class="modal">
        <h3>Modal Title</h3>
        <p>This is a teleport modal!</p>
        <button @click="closeModal">Close</button>
      </div>
    </div>
  </teleport>
</template>

<script>
import { defineProps, defineEmits } from 'vue';

export default {
  props: {
    isOpen: Boolean
  },
  emits: ['close'],
  setup(props, { emit }) {
    const closeModal = () => {
      emit("close");
    };
    return { closeModal };
  }
};
</script>

<style scoped>
.modal-overlay {
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

.modal {
  background: white;
  padding: 20px;
  border-radius: 5px;
  box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
}
</style>
```

✅ **توضیح این کامپوننت:**  
1️⃣ **از `teleport` برای انتقال مودال به `body` استفاده شده است.**  
2️⃣ **یک `prop` به نام `isOpen` تعریف شده است تا مشخص کند که مودال باز باشد یا خیر.**  
3️⃣ **با کلیک روی دکمه `Close`، `emit("close")` اجرا شده و مودال بسته می‌شود.**  

---

✅ **مرحله ۲: استفاده از `Modal.vue` در کامپوننت اصلی**  

**📍 فایل `App.vue`**  

```vue
<template>
  <div>
    <h1>Welcome to Vue 3 Teleport Example</h1>
    <button @click="showModal = true">Open Modal</button>
    
    <Modal :isOpen="showModal" @close="showModal = false" />
  </div>
</template>

<script>
import { ref } from 'vue';
import Modal from './Modal.vue';

export default {
  components: { Modal },
  setup() {
    const showModal = ref(false);
    return { showModal };
  }
};
</script>
```

✅ **توضیح این کامپوننت:**  
1️⃣ **یک دکمه برای باز کردن مودال اضافه شده است.**  
2️⃣ **زمانی که `showModal = true` شود، مودال نمایش داده می‌شود.**  
3️⃣ **با کلیک روی دکمه "Close"، مقدار `showModal` دوباره `false` شده و مودال بسته می‌شود.**  

---

## **۳. چرا `Teleport` مهم است؟**  

📌 **مزایای استفاده از `Teleport` در Vue 3:**  
✅ **جلوگیری از مشکلات `z-index`** (مثلاً در نمایش مودال روی سایر عناصر صفحه)  
✅ **سازگاری بهتر با فریم‌ورک‌های CSS مانند Bootstrap و Tailwind**  
✅ **قرار دادن مودال‌ها یا منوهای کشویی در موقعیت مناسب درون `body`**  
✅ **عدم تأثیر `overflow: hidden` یا `position: relative` از والدهای داخلی**  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`Teleport`** در Vue 3 به ما امکان می‌دهد **محتوای کامپوننت را خارج از والد اصلی در `body` قرار دهیم.**  
✅ **از `teleport` برای نمایش مودال‌ها، اعلان‌ها و منوهای کشویی در جای مناسب صفحه استفاده می‌کنیم.**  
✅ **استفاده از `Teleport` باعث جلوگیری از مشکلات `z-index` و استایل‌دهی نادرست می‌شود.**  
