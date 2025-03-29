### **📌 صفحه ۹ - آشنایی با Provide & Inject در Vue.js**  

در این صفحه، مفهوم **`provide` و `inject`** را بررسی می‌کنیم که یکی از ویژگی‌های پیشرفته Vue.js برای **مدیریت داده‌ها در کامپوننت‌های تو در تو** است.  

---

## **۱. Provide & Inject چیست؟**  

📌 **مشکل `props drilling` در Vue.js**  
در Vue.js، اگر داده‌ای را از یک کامپوننت والد (`Parent`) به چندین سطح از کامپوننت‌های فرزند (`Child`) ارسال کنیم، مجبوریم از **props** استفاده کنیم. اما اگر سطح تو در تویی زیاد باشد، این کار **پیچیده و سخت** خواهد شد.  

🔹 **راه‌حل؟ استفاده از Provide & Inject!**  
✅ `provide` به **والد اجازه می‌دهد داده‌ای را برای تمام فرزندان قابل دسترس کند**.  
✅ `inject` به **فرزندان اجازه می‌دهد داده‌ی موردنظر را از والد دریافت کنند**، بدون نیاز به `props`.  

📌 **مزایای استفاده از Provide & Inject**  
- **کاهش پیچیدگی انتقال داده‌ها بین چندین سطح کامپوننت**  
- **عدم نیاز به ارسال props در تمام سطوح**  
- **مدیریت ساده‌تر داده‌ها در پروژه‌های بزرگ**  

---

## **۲. نحوه‌ی استفاده از Provide & Inject در Vue.js**  

📌 **مثال: ارسال داده از کامپوننت والد (`App.vue`) به چندین کامپوننت فرزند**  

### **📍 ۱. تعریف `provide` در کامپوننت والد (`App.vue`)**  

```vue
<template>
  <div>
    <h2>Parent Component</h2>
    <ChildComponent />
  </div>
</template>

<script>
import { provide, ref } from 'vue';
import ChildComponent from './ChildComponent.vue';

export default {
  components: { ChildComponent },
  setup() {
    const message = ref("Hello from Parent!");
    
    // ارسال مقدار برای تمام فرزندان
    provide('sharedMessage', message);

    return { message };
  }
};
</script>
```

✅ **در `App.vue` مقدار `message` را با `provide` در دسترس فرزندان قرار می‌دهیم.**  

---

### **📍 ۲. دریافت مقدار `provide` در فرزند (`ChildComponent.vue`)**  

```vue
<template>
  <div>
    <h3>Child Component</h3>
    <p>Received Message: {{ sharedMessage }}</p>
  </div>
</template>

<script>
import { inject } from 'vue';

export default {
  setup() {
    // دریافت مقدار از provide والد
    const sharedMessage = inject('sharedMessage');

    return { sharedMessage };
  }
};
</script>
```

✅ **در `ChildComponent.vue` مقدار `sharedMessage` را از `provide` دریافت کرده و نمایش می‌دهیم.**  

---

## **۳. ارسال و دریافت مقادیر پیچیده در Provide & Inject**  

📌 **اگر مقدار `provide` یک آبجکت باشد، می‌توانیم آن را در فرزند تغییر دهیم.**  

### **📍 ۱. تغییر مقدار `provide` در فرزند (`ChildComponent.vue`)**  

```vue
<template>
  <div>
    <h3>Child Component</h3>
    <p>Message: {{ sharedData.message }}</p>
    <button @click="sharedData.message = 'Updated by Child!'">Update Message</button>
  </div>
</template>

<script>
import { inject } from 'vue';

export default {
  setup() {
    const sharedData = inject('sharedData');

    return { sharedData };
  }
};
</script>
```

✅ **در این مثال، مقدار `sharedData.message` از `provide` دریافت شده و با کلیک روی دکمه تغییر می‌کند.**  

---

## **۴. مقایسه Provide & Inject با Vuex و Pinia**  

| ویژگی             | Provide & Inject | Vuex / Pinia |
|------------------|----------------|-------------|
| پیچیدگی کم       | ✅ بله        | ❌ خیر     |
| مناسب برای داده‌های ساده | ✅ بله  | ✅ بله     |
| مدیریت وضعیت در پروژه‌های بزرگ | ❌ خیر | ✅ بله   |
| نیاز به وابستگی خارجی | ❌ ندارد | ✅ دارد  |

📌 **نتیجه:**  
- **برای ارسال داده در بین چندین سطح از کامپوننت‌ها**، Provide & Inject گزینه مناسبی است.  
- **برای مدیریت وضعیت پیچیده و سراسری (global state)**، بهتر است از Vuex یا Pinia استفاده شود.  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`provide` و `inject`** برای ارسال و دریافت داده بین **کامپوننت‌های تو در تو** استفاده می‌شود.  
✅ **مزیت اصلی آن، کاهش پیچیدگی `props drilling` و ساده‌تر شدن انتقال داده‌ها است.**  
✅ **اگر مقدار `provide` یک آبجکت باشد، فرزندان می‌توانند مقدار آن را تغییر دهند.**  
✅ **برای مدیریت داده‌های پیچیده و سراسری، Vuex یا Pinia گزینه‌های بهتری هستند.**  
