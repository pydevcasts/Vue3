### **📌 صفحه 16 - آشنایی با Teleport در Vue.js**  

در این صفحه، درباره‌ی ویژگی **Teleport** در Vue.js صحبت خواهیم کرد. این قابلیت به ما اجازه می‌دهد **المان‌های HTML را در مکانی متفاوت از ساختار DOM کامپوننت، رندر کنیم**.  

---

## **۱. تعریف Teleport چیست و چرا از آن استفاده کنیم؟**  

📌 **مشکل رندر کردن عناصر خاص در Vue.js**  
در حالت عادی، Vue.js تمام عناصر و کامپوننت‌ها را **درون محدوده‌ی کامپوننت والد** رندر می‌کند. اما در بعضی موارد، نیاز داریم که برخی عناصر مانند:  
✅ **مودال‌ها (Modals)**  
✅ **پنجره‌های اعلان (Notifications)**  
✅ **تولتیپ‌ها (Tooltips)**  
✅ **دیالوگ‌ها (Dialogs)**  
... را **در بخشی جداگانه از DOM**، مثلاً مستقیماً در `<body>` رندر کنیم.  

🔹 **راه‌حل؟ استفاده از `Teleport`!**  
✅ `Teleport` این امکان را می‌دهد که یک **کامپوننت درون یک بخش خاص از DOM، خارج از والد اصلی‌اش نمایش داده شود**.  

---

## **۲. نحوه‌ی استفاده از Teleport در Vue.js**  

📌 **مثال: نمایش یک دیالوگ با استفاده از `Teleport`**  

### **📍 ۱. استفاده از `Teleport` در `App.vue`**  

```vue
<template>
  <div>
    <h2>Parent Component</h2>
    <button @click="showModal = true">Open Modal</button>

    <teleport to="body">
      <div v-if="showModal" class="modal">
        <div class="modal-content">
          <h3>Modal Title</h3>
          <p>This is a modal using Teleport.</p>
          <button @click="showModal = false">Close</button>
        </div>
      </div>
    </teleport>
  </div>
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

✅ **در این مثال:**  
- دکمه‌ی **"Open Modal"** باعث نمایش مودال می‌شود.  
- `teleport to="body"` **محتوای مودال را مستقیماً درون `<body>` قرار می‌دهد**.  
- وقتی `showModal = true` شود، **مودال نمایش داده می‌شود**.  
- با کلیک روی دکمه‌ی **"Close"**، مقدار `showModal` تغییر کرده و مودال بسته می‌شود.  

---

## **۳. استفاده از Teleport برای نمایش نوتیفیکیشن‌ها**  

📌 **مثال: نمایش نوتیفیکیشن در بالای صفحه با `Teleport`**  

```vue
<template>
  <div>
    <button @click="showNotification = true">Show Notification</button>

    <teleport to="body">
      <div v-if="showNotification" class="notification">
        This is a notification!
        <button @click="showNotification = false">Dismiss</button>
      </div>
    </teleport>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const showNotification = ref(false);

    return { showNotification };
  }
};
</script>

<style>
.notification {
  position: fixed;
  top: 20px;
  right: 20px;
  background: #007bff;
  color: white;
  padding: 10px 20px;
  border-radius: 5px;
}
</style>
```

✅ **در این مثال:**  
- دکمه‌ی **"Show Notification"** باعث نمایش نوتیفیکیشن می‌شود.  
- `teleport to="body"` باعث می‌شود که نوتیفیکیشن مستقیماً در `<body>` قرار بگیرد.  
- بعد از کلیک روی **"Dismiss"**، مقدار `showNotification` تغییر کرده و نوتیفیکیشن ناپدید می‌شود.  

---

## **۴. بررسی تفاوت `Teleport` و `Portal` در Vue.js 2**  

📌 **در Vue 2، ویژگی مشابهی به نام `Portal` در پکیج `portal-vue` وجود داشت.**  
🔹 اما در Vue 3، `Teleport` **به‌صورت داخلی پشتیبانی می‌شود و دیگر نیازی به پکیج‌های اضافی ندارد**.  

| ویژگی           | Portal در Vue 2 | Teleport در Vue 3 |
|----------------|----------------|----------------|
| نیاز به نصب پکیج اضافی | ✅ بله (`portal-vue`) | ❌ خیر |
| یکپارچگی با Vue | ❌ خیر (پکیج جداگانه) | ✅ بله (در هسته Vue) |
| استفاده آسان | ❌ نیاز به پیکربندی بیشتر | ✅ بله (ساده و مستقیم) |

✅ **نتیجه:** اگر از Vue 3 استفاده می‌کنید، **`Teleport` را جایگزین `Portal` کنید**!  

---

## **📌 جمع‌بندی این صفحه**  

✅ ** در واقع `Teleport` به ما اجازه می‌دهد یک المان را در بخشی از DOM خارج از کامپوننت اصلی رندر کنیم.**  
✅ **این ویژگی برای ساخت مودال‌ها، نوتیفیکیشن‌ها و دیالوگ‌های جداگانه بسیار کاربردی است.**  
✅ **این ویژگی جایگزین `Portal` در Vue 3 شده و نیاز به پکیج‌های اضافی ندارد.**  
✅ **با `teleport to="body"` می‌توان محتوای یک کامپوننت را مستقیماً درون `<body>` قرار داد.**  

---
