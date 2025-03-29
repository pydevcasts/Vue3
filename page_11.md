### **📌 صفحه ۱۱ - آشنایی با Watchers در Vue.js**  

در این صفحه، به بررسی **Watchers** در Vue.js می‌پردازیم. این ویژگی به ما کمک می‌کند **تغییرات در مقادیر متغیرها را تشخیص داده و به آن‌ها واکنش نشان دهیم.**  

---

## **۱. Watchers چیست و چرا استفاده می‌شود؟**  

📌 **مشکل: چگونه تغییرات یک متغیر را در لحظه بررسی کنیم؟**  
گاهی اوقات نیاز داریم که هر **زمان مقدار یک متغیر تغییر کرد، عملیات خاصی اجرا شود.** برای مثال:  
✅ بررسی **ورودی کاربر** و انجام اعتبارسنجی (`validation`) در لحظه  
✅ مشاهده **تغییرات API و اجرای درخواست‌های جدید**  
✅ کنترل مقدار **یک متغیر وابسته به سایر متغیرها**  

🔹 **راه‌حل؟ استفاده از Watchers!**  
✅ `watch()` به ما اجازه می‌دهد **تغییرات در داده‌های ری‌اکتیو را گوش داده و در صورت تغییر، یک تابع اجرا کنیم.**  

---

## **۲. نحوه‌ی استفاده از Watchers در Vue.js**  

📌 **مثال: مشاهده تغییرات مقدار ورودی و نمایش پیغام مناسب**  

### **📍 ۱. استفاده از `watch()` در `setup()`**  

```vue
<template>
  <div>
    <input v-model="name" placeholder="Enter your name" />
    <p>{{ message }}</p>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  setup() {
    const name = ref('');
    const message = ref('');

    // مشاهده تغییرات مقدار name
    watch(name, (newValue) => {
      if (newValue.length < 3) {
        message.value = "Name is too short!";
      } else {
        message.value = `Hello, ${newValue}!`;
      }
    });

    return { name, message };
  }
};
</script>
```

✅ **در این مثال:**  
- مقدار `name` را در `watch()` نظارت می‌کنیم.  
- هر زمان مقدار `name` تغییر کند، مقدار `message` نیز بر اساس آن تغییر خواهد کرد.  

---

## **۳. استفاده از Watchers برای مقادیر پیچیده (آبجکت و آرایه‌ها)**  

📌 **اگر مقدار `watch()` یک آبجکت یا آرایه باشد، Vue تغییرات را به‌صورت پیش‌فرض تشخیص نمی‌دهد. برای حل این مشکل از `deep: true` استفاده می‌کنیم.**  

### **📍 ۲. مشاهده تغییرات در یک آبجکت**  

```vue
<template>
  <div>
    <p>Age: {{ person.age }}</p>
    <button @click="person.age++">Increase Age</button>
  </div>
</template>

<script>
import { reactive, watch } from 'vue';

export default {
  setup() {
    const person = reactive({ name: "Ali", age: 25 });

    watch(
      person,
      (newValue) => {
        console.log("Person updated:", newValue);
      },
      { deep: true }
    );

    return { person };
  }
};
</script>
```

✅ **در این مثال:**  
- از `reactive` برای تعریف آبجکت `person` استفاده شده است.  
- چون Vue به‌صورت پیش‌فرض تغییرات درون آبجکت را تشخیص نمی‌دهد، `deep: true` اضافه شده است.  

---

## **۴. مشاهده تغییرات مقدار قبلی و مقدار جدید در Watchers**  

📌 **گاهی اوقات نیاز داریم مقدار قبلی و مقدار جدید را همزمان مقایسه کنیم.**  

### **📍 ۳. نمایش مقدار قبلی و جدید در کنسول**  

```vue
<template>
  <div>
    <input v-model="count" type="number" />
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  setup() {
    const count = ref(0);

    watch(
      count,
      (newValue, oldValue) => {
        console.log(`Previous: ${oldValue}, New: ${newValue}`);
      }
    );

    return { count };
  }
};
</script>
```

✅ **در این مثال:**  
- مقدار قبلی و جدید `count` در کنسول نمایش داده می‌شود.  

---

## **۵. تفاوت Watchers و Computed در Vue.js**  

📌 **چه زمانی از `watch()` استفاده کنیم و چه زمانی از `computed()`؟**  

| ویژگی            | Watch | Computed |
|-----------------|--------|----------|
| نظارت بر تغییرات یک مقدار در لحظه | ✅ بله | ❌ خیر |
| مقدار بازگشتی دارد | ❌ خیر | ✅ بله |
| اجرای تابع هنگام تغییر مقدار | ✅ بله | ❌ خیر |
| مناسب برای پردازش‌های پیچیده و وابسته | ✅ بله | ❌ خیر |

✅ **نتیجه:**  
- **اگر نیاز داریم مقدار بازگشتی داشته باشیم، `computed` مناسب است.**  
- **اگر می‌خواهیم به تغییر مقدار واکنش نشان دهیم، `watch()` را استفاده می‌کنیم.**  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`watch()` برای نظارت بر تغییرات یک مقدار ری‌اکتیو و اجرای عملیات خاص استفاده می‌شود.**  
✅ **می‌توانیم مقدار جدید و مقدار قبلی را در `watch()` دریافت کنیم.**  
✅ **برای آبجکت‌ها و آرایه‌های پیچیده، نیاز به `deep: true` داریم.**  
✅ **برای مقدار وابسته و بازگشتی، بهتر است از `computed` به‌جای `watch()` استفاده کنیم.**  

