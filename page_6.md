### **📌   Watchers, Computed Properties و Async Components در Vue.js**  

در این صفحه، سه مفهوم کلیدی Vue.js را بررسی می‌کنیم که در بهینه‌سازی عملکرد، مدیریت تغییرات داده و بارگذاری کامپوننت‌ها به ما کمک می‌کنند.  

---

## **۱. Watchers در Vue.js**  

📌 **`watch` چیست؟**  
زمانی که بخواهیم تغییر مقدار یک متغیر را **مانیتور کرده و بر اساس تغییر آن، عملی انجام دهیم**، از `watch` استفاده می‌کنیم.  

✅ **کاربردهای `watch`**  
- ارسال درخواست API پس از تغییر یک مقدار  
- نظارت بر ورودی‌های فرم و انجام عملیات بر اساس آن  
- ذخیره اطلاعات در `localStorage` هنگام تغییر مقدار  

📌 **مثال: مشاهده تغییر مقدار و انجام یک عملیات**  

```vue
<template>
  <div>
    <input v-model="name" placeholder="Enter your name" />
    <p>Hello, {{ name }}!</p>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  setup() {
    const name = ref("");

    watch(name, (newValue, oldValue) => {
      console.log(`Name changed from ${oldValue} to ${newValue}`);
    });

    return { name };
  }
};
</script>
```

✅ **نتیجه:** با هر تغییر مقدار `name`، مقدار جدید و قبلی در کنسول نمایش داده می‌شود.  

🔹 **نکته:**  
- مقدار `oldValue` در اولین اجرا `undefined` است.  
- `watch` برای متغیرهای **`ref` و `reactive`** قابل استفاده است.  

---

## **۲. Computed Properties در Vue.js**  

📌 **`computed` چیست؟**  
`computed` برای **محاسبات وابسته به داده‌ها** استفاده می‌شود. بر خلاف `methods`، مقدار خروجی `computed` **کش شده** و فقط در صورت تغییر مقادیر وابسته، دوباره محاسبه می‌شود.  

📌 **مثال: محاسبه طول نام کاربر به صورت خودکار**  

```vue
<template>
  <div>
    <input v-model="name" placeholder="Enter your name" />
    <p>Character count: {{ nameLength }}</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const name = ref("");

    const nameLength = computed(() => {
      return name.value.length;
    });

    return { name, nameLength };
  }
};
</script>
```

✅ **نتیجه:** با تغییر مقدار `name`، مقدار `nameLength` به طور خودکار به‌روزرسانی شده ولی **تنها زمانی محاسبه می‌شود که مقدار `name` تغییر کند**.

🔹 **نکته:**  
- اگر مقدار مورد نظر در `computed` تغییر نکند، مقدار کش شده قبلی برگردانده می‌شود.  
- از `computed` زمانی استفاده کنید که مقدار محاسبه شده به داده‌های دیگری وابسته باشد.  

---

## **۳. بارگذاری کامپوننت‌های Async در Vue.js**  

📌 **چرا از `Async Components` استفاده کنیم؟**  
وقتی یک برنامه بزرگ داریم، بهتر است برخی از **کامپوننت‌ها را فقط در صورت نیاز بارگذاری کنیم** تا از افزایش حجم اولیه برنامه جلوگیری شود.  

✅ **کاربردهای `Async Components`**  
- کاهش حجم اولیه بارگذاری  
- بهینه‌سازی عملکرد با تقسیم کد (Code Splitting)  
- بارگذاری بخش‌هایی از برنامه فقط هنگام نیاز  

📌 **مثال: بارگذاری Async یک کامپوننت**  

### **📍 ایجاد کامپوننت `HeavyComponent.vue`**  

```vue
<template>
  <div>
    <h2>This is a heavy component!</h2>
  </div>
</template>
```

### **📍 استفاده از `defineAsyncComponent` در `App.vue`**  

```vue
<template>
  <button @click="showComponent = true">Load Component</button>
  <Suspense>
    <template #default>
      <AsyncComponent v-if="showComponent" />
    </template>
    <template #fallback>
      <p>Loading...</p>
    </template>
  </Suspense>
</template>

<script>
import { ref, defineAsyncComponent } from 'vue';

export default {
  components: {
    AsyncComponent: defineAsyncComponent(() => import('./HeavyComponent.vue'))
  },
  setup() {
    const showComponent = ref(false);
    return { showComponent };
  }
};
</script>
```

✅ **نتیجه:**  
- با کلیک روی دکمه، `HeavyComponent.vue` بارگیری شده و نمایش داده می‌شود.  
- هنگام بارگیری، متن `"Loading..."` نمایش داده می‌شود.  
- بهینه‌سازی بارگذاری باعث بهبود سرعت اجرای برنامه خواهد شد.  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`watch`** برای **مانیتور کردن تغییرات داده‌ها** و اجرای عملیات خاص استفاده می‌شود.  
✅ **`computed`** برای **محاسبات وابسته به داده‌ها** به کار می‌رود و مقدار آن **کش شده** و فقط در صورت تغییر مقدار وابسته، به‌روزرسانی می‌شود.  
✅ **`Async Components`** برای **بارگذاری تنبل (Lazy Loading)** و **بهینه‌سازی حجم برنامه** کاربرد دارد.  
