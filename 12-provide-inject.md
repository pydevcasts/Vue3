در Vue.js 3، برای مدیریت وضعیت (State Management) با **Composition API** می‌توان از `reactive` و `ref` استفاده کرد. در اینجا چند روش مختلف برای مدیریت وضعیت را بررسی می‌کنیم.

---

### **۱. استفاده از `reactive` برای مدیریت وضعیت پیچیده‌تر**
`reactive` یک شیء قابل واکنش ایجاد می‌کند که برای مدیریت وضعیت‌هایی با چندین ویژگی مناسب است.

```vue
<script setup>
import { reactive } from 'vue';

const state = reactive({
  count: 0,
  user: {
    name: 'Ali',
    age: 25
  }
});

const increment = () => {
  state.count++;
};

const updateName = (newName) => {
  state.user.name = newName;
};
</script>

<template>
  <div>
    <p>Count: {{ state.count }}</p>
    <button @click="increment">Increase</button>

    <p>User: {{ state.user.name }} - {{ state.user.age }}</p>
    <input v-model="state.user.name" />
  </div>
</template>
```

---

### **۲. استفاده از `ref` برای مقادیر ساده**
`ref` یک مقدار واکنشی ساده ایجاد می‌کند که برای مقدارهای منفرد مناسب است.

```vue
<script setup>
import { ref } from 'vue';

const count = ref(0);

const increment = () => {
  count.value++;
};
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increase</button>
  </div>
</template>
```

---

### **۳. استفاده از `computed` برای مقادیر وابسته**
اگر بخواهید مقداری بر اساس وضعیت محاسبه شود، می‌توانید از `computed` استفاده کنید.

```vue
<script setup>
import { ref, computed } from 'vue';

const count = ref(5);

const doubleCount = computed(() => count.value * 2);
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Double Count: {{ doubleCount }}</p>
  </div>
</template>
```

---

### **۴. مدیریت وضعیت به کمک Composition Functions (ساختارهای قابل استفاده مجدد)**
می‌توانیم وضعیت را در یک فایل جداگانه مدیریت کنیم و در کامپوننت‌های مختلف از آن استفاده کنیم.

#### **ایجاد یک Composable (useCounter.js)**
```js
// composables/useCounter.js
import { ref } from 'vue';

export function useCounter() {
  const count = ref(0);
  
  const increment = () => {
    count.value++;
  };

  return { count, increment };
}
```

#### **استفاده از آن در کامپوننت**
```vue
<script setup>
import { useCounter } from '@/composables/useCounter';

const { count, increment } = useCounter();
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <button @click="increment">Increase</button>
  </div>
</template>
```

---

### **۵. مدیریت وضعیت سراسری با `pinia` (جایگزین Vuex)**
برای مدیریت وضعیت سراسری، بهتر است از `pinia` که در Vue 3 توصیه شده است، استفاده کنیم.

#### **نصب `pinia`**
```sh
npm install pinia
```

#### **ایجاد Store**
```js
// stores/counter.js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),
  actions: {
    increment() {
      this.count++;
    }
  }
});
```

#### **استفاده در کامپوننت**
```vue
<script setup>
import { useCounterStore } from '@/stores/counter';

const counterStore = useCounterStore();
</script>

<template>
  <div>
    <p>Count: {{ counterStore.count }}</p>
    <button @click="counterStore.increment">Increase</button>
  </div>
</template>
```

# 12. ارائه و تزریق (Provide/Inject)

در این فصل، مفهوم **`provide` و `inject`** را بررسی می‌کنیم که یکی از ویژگی‌های پیشرفته Vue.js برای **مدیریت داده‌ها در کامپوننت‌های تو در تو** است.  

---

## مفهوم Provide & Inject

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

## نحوه‌ی استفاده از Provide & Inject

📌 **مثال: ارسال داده از کامپوننت والد (`App.vue`) به چندین کامپوننت فرزند**  

### تعریف `provide` در کامپوننت والد (`App.vue`)

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

### دریافت مقدار `provide` در فرزند (`ChildComponent.vue`)

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

---

## ارسال و دریافت مقادیر پیچیده

📌 **اگر مقدار `provide` یک آبجکت باشد، می‌توانیم آن را در فرزند تغییر دهیم.**  

### تغییر مقدار `provide` در فرزند

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

---

## مقایسه با Vuex و Pinia

| ویژگی             | Provide & Inject | Vuex / Pinia |
|------------------|----------------|-------------|
| پیچیدگی کم       | ✅ بله        | ❌ خیر     |
| مناسب برای داده‌های ساده | ✅ بله  | ✅ بله     |
| مدیریت وضعیت در پروژه‌های بزرگ | ❌ خیر | ✅ بله   |
| نیاز به وابستگی خارجی | ❌ ندارد | ✅ دارد  |

📌 **نکات کلیدی:**  
- برای ارسال داده در بین چندین سطح از کامپوننت‌ها، Provide & Inject گزینه مناسبی است
- برای مدیریت وضعیت پیچیده و سراسری (global state)، بهتر است از Vuex یا Pinia استفاده شود

## جمع‌بندی

✅ **`provide` و `inject`** برای ارسال و دریافت داده بین **کامپوننت‌های تو در تو** استفاده می‌شود  
✅ **مزیت اصلی آن، کاهش پیچیدگی `props drilling` و ساده‌تر شدن انتقال داده‌ها است**  
✅ **اگر مقدار `provide` یک آبجکت باشد، فرزندان می‌توانند مقدار آن را تغییر دهند**  
✅ **برای مدیریت داده‌های پیچیده و سراسری، Vuex یا Pinia گزینه‌های بهتری هستند**
