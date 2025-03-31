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
