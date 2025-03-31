### **📌 صفحه ۱۹ - `Watchers` در Vue 3**  

در این صفحه، به بررسی **`watch` و `watchEffect`** در Vue 3 می‌پردازیم که برای **مشاهده و واکنش به تغییرات داده‌ها** استفاده می‌شوند.  

---

## **۱. `Watchers` چیست و چرا استفاده می‌شود؟**  

📌 گاهی در Vue نیاز داریم که **تغییرات متغیرها را بررسی کرده و بر اساس آن‌ها عملی انجام دهیم**. به عنوان مثال، اگر کاربر مقدار یک `input` را تغییر دهد، ما می‌توانیم فوراً یک عملیات خاص (مانند ارسال داده به سرور) انجام دهیم.  

✅ **کاربردهای `watch` و `watchEffect` در Vue 3:**  
✔ نظارت بر تغییرات `ref` و `reactive`  
✔ درخواست‌های `API` هنگام تغییر مقدار متغیر  
✔ اجرای عملیات‌های غیرهمزمان هنگام تغییر داده  

---

## **۲. `watch` در Vue 3**  

📌 **`watch` زمانی استفاده می‌شود که بخواهیم تغییر یک متغیر خاص را دنبال کنیم.**  

### **📌 مثال: بررسی تغییر مقدار `count` و نمایش مقدار جدید**  

```vue
<template>
  <div>
    <h2>Watch Example</h2>
    <p>Count: {{ count }}</p>
    <button @click="count++">Increase Count</button>
  </div>
</template>

<script>
import { ref, watch } from "vue";

export default {
  setup() {
    const count = ref(0);

    watch(count, (newValue, oldValue) => {
      console.log(`Count changed from ${oldValue} to ${newValue}`);
    });

    return { count };
  },
};
</script>
```

✅ **توضیح کد:**  
✔ مقدار `count` هنگام کلیک افزایش می‌یابد.  
✔ **`watch(count, (newValue, oldValue) => {...})`** به Vue می‌گوید که تغییرات `count` را مشاهده کند.  
✔ در **کنسول لاگ می‌شود که مقدار `count` چگونه تغییر کرده است.**  

---

## **۳. `watch` برای `reactive` (چندین مقدار)**  

📌 **برای مشاهده تغییرات در یک `reactive` که شامل چندین مقدار است، از یک تابع استفاده می‌کنیم.**  

### **📌 مثال: مشاهده تغییرات در یک آبجکت `reactive`**  

```vue
<template>
  <div>
    <h2>Reactive Watch Example</h2>
    <p>Name: {{ user.name }}</p>
    <p>Age: {{ user.age }}</p>
    <button @click="user.age++">Increase Age</button>
  </div>
</template>

<script>
import { reactive, watch } from "vue";

export default {
  setup() {
    const user = reactive({ name: "Ali", age: 25 });

    watch(
      () => user.age,
      (newAge, oldAge) => {
        console.log(`Age changed from ${oldAge} to ${newAge}`);
      }
    );

    return { user };
  },
};
</script>
```

✅ **توضیح کد:**  
✔ مقدار `user.age` هنگام کلیک افزایش می‌یابد.  
✔ از `watch(() => user.age, (newAge, oldAge) => {...})` استفاده شده تا تغییر مقدار `age` بررسی شود.  
✔ مقدار جدید و قدیمی در کنسول نمایش داده می‌شود.  

---

## **۴. `watchEffect` در Vue 3**  

📌 **`watchEffect` بدون نیاز به مشخص کردن متغیر خاص، تغییرات هر متغیری که داخلش استفاده شود را دنبال می‌کند.**  

### **📌 مثال: استفاده از `watchEffect` برای نظارت بر چندین مقدار**  

```vue
<template>
  <div>
    <h2>watchEffect Example</h2>
    <p>Message: {{ message }}</p>
    <p>Count: {{ count }}</p>
    <button @click="count++">Increase Count</button>
  </div>
</template>

<script>
import { ref, watchEffect } from "vue";

export default {
  setup() {
    const count = ref(0);
    const message = ref("Hello Vue!");

    watchEffect(() => {
      console.log(`Count is now ${count.value}`);
    });

    return { count, message };
  },
};
</script>
```

✅ **توضیح کد:**  
✔ مقدار `count` هنگام کلیک افزایش می‌یابد.  
✔ `watchEffect(() => {...})` به صورت خودکار **هر تغییری که در `count` رخ دهد را تشخیص داده و اجرا می‌شود.**  
✔ نیازی به مشخص کردن `count` در `watchEffect` نیست.  

---

## **۵. تفاوت `watch` و `watchEffect`**  

✅ **`watch`** → **فقط** برای متغیرهای خاص که به آن‌ها اشاره شده است اجرا می‌شود.  
✅ **`watchEffect`** → **هر متغیری که داخلش استفاده شده باشد، به صورت خودکار دنبال می‌کند.**  

### **📌 جدول مقایسه `watch` و `watchEffect`**  

| ویژگی         | `watch` | `watchEffect` |
|--------------|--------|--------------|
| نیاز به مشخص کردن متغیر؟ | ✅ بله | ❌ خیر |
| پشتیبانی از مقدار قدیم و جدید؟ | ✅ بله | ❌ خیر |
| اجرا شدن هنگام مقداردهی اولیه؟ | ❌ خیر | ✅ بله |

---

## **📌 جمع‌بندی این صفحه**  

✅ **`watch` برای مشاهده و واکنش به تغییر مقدار یک متغیر خاص استفاده می‌شود.**  
✅ **`watchEffect` به‌طور خودکار هر متغیری که داخلش استفاده شود را بررسی می‌کند.**  
✅ **`watch` مقدار قدیمی و جدید را ارائه می‌دهد، اما `watchEffect` این قابلیت را ندارد.**  
✅ **از `watchEffect` زمانی استفاده کنید که نیاز به مانیتورینگ کلی متغیرها بدون مشخص کردن نامشان دارید.**  

