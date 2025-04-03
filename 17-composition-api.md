### **📌 صفحه ۱۷ - مدیریت وضعیت با Composition API**

در این صفحه، به بررسی **Composition API** در Vue 3 می‌پردازیم که یک روش جدید و قدرتمند برای مدیریت منطق و وضعیت در کامپوننت‌هاست.

---

## **۱. Composition API چیست؟**

📌 **Composition API** یک روش جدید در Vue 3 است که به ما اجازه می‌دهد منطق کامپوننت‌ها را به شکل بهتری سازماندهی کنیم.

✅ **مزایای Composition API:**
- **کد تمیزتر و قابل استفاده مجدد**
- **تایپ‌اسکریپت ساپورت بهتر**
- **انعطاف‌پذیری بیشتر در سازماندهی کد**
- **عملکرد بهتر در مقایسه با Options API**

---

## **۲. استفاده از `setup()`**

📌 `setup()` نقطه ورود اصلی برای استفاده از Composition API است.

```vue
<template>
  <div>
    <h2>Counter: {{ count }}</h2>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <p>Double: {{ doubleCount }}</p>
  </div>
</template>

<script>
import { ref, computed } from 'vue'

export default {
  setup() {
    // تعریف state
    const count = ref(0)

    // متدها
    const increment = () => count.value++
    const decrement = () => count.value--

    // computed properties
    const doubleCount = computed(() => count.value * 2)

    // بازگرداندن مقادیر مورد نیاز در template
    return {
      count,
      increment,
      decrement,
      doubleCount
    }
  }
}
</script>
```

---

## **۳. Reactive References با `ref()`**

📌 `ref()` برای ایجاد مقادیر واکنش‌پذیر استفاده می‌شود.

```vue
<script>
import { ref, onMounted } from 'vue'

export default {
  setup() {
    const name = ref('')
    const age = ref(25)

    onMounted(() => {
      name.value = 'Ali'
    })

    return { name, age }
  }
}
</script>
```

---

## **۴. Reactive Objects با `reactive()`**

📌 `reactive()` برای ایجاد آبجکت‌های واکنش‌پذیر استفاده می‌شود.

```vue
<template>
  <div>
    <h2>{{ user.name }}</h2>
    <p>Age: {{ user.age }}</p>
    <button @click="updateUser">Update User</button>
  </div>
</template>

<script>
import { reactive } from 'vue'

export default {
  setup() {
    const user = reactive({
      name: 'Ali',
      age: 25
    })

    const updateUser = () => {
      user.age++
      user.name = user.name + '!'
    }

    return { user, updateUser }
  }
}
</script>
```

---

## **۵. Lifecycle Hooks در Composition API**

📌 در Composition API، هوک‌های چرخه حیات با پیشوند `on` استفاده می‌شوند.

```vue
<script>
import { onMounted, onUpdated, onUnmounted } from 'vue'

export default {
  setup() {
    onMounted(() => {
      console.log('Component mounted!')
    })

    onUpdated(() => {
      console.log('Component updated!')
    })

    onUnmounted(() => {
      console.log('Component will unmount!')
    })
  }
}
</script>
```

---

## **۶. Watch و WatchEffect**

📌 برای نظارت بر تغییرات داده‌ها از `watch` و `watchEffect` استفاده می‌کنیم.

```vue
<script>
import { ref, watch, watchEffect } from 'vue'

export default {
  setup() {
    const searchQuery = ref('')
    const results = ref([])

    // با watch
    watch(searchQuery, async (newQuery) => {
      if (newQuery.length > 3) {
        const response = await fetch(`/api/search?q=${newQuery}`)
        results.value = await response.json()
      }
    })

    // با watchEffect
    watchEffect(() => {
      console.log('Current query:', searchQuery.value)
      console.log('Results count:', results.value.length)
    })

    return { searchQuery, results }
  }
}
</script>
```

---

## **۷. Composables (توابع ترکیبی)**

📌 یکی از قدرتمندترین ویژگی‌های Composition API، امکان ایجاد توابع ترکیبی است.

```js
// useCounter.js
import { ref, computed } from 'vue'

export function useCounter(initialValue = 0) {
  const count = ref(initialValue)
  const doubleCount = computed(() => count.value * 2)

  function increment() {
    count.value++
  }

  function decrement() {
    count.value--
  }

  return {
    count,
    doubleCount,
    increment,
    decrement
  }
}
```

```vue
// Counter.vue
<template>
  <div>
    <h2>Count: {{ count }}</h2>
    <p>Double: {{ doubleCount }}</p>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>

<script>
import { useCounter } from './useCounter'

export default {
  setup() {
    const { count, doubleCount, increment, decrement } = useCounter(10)
    return { count, doubleCount, increment, decrement }
  }
}
</script>
```

---

## **📌 جمع‌بندی این صفحه**  

✅ **Composition API** یک روش قدرتمند برای سازماندهی منطق در Vue 3 است.
✅ با استفاده از `setup()`، `ref()` و `reactive()` می‌توانیم وضعیت کامپوننت را مدیریت کنیم.
✅ هوک‌های چرخه حیات، `watch` و `watchEffect` برای کنترل رفتار کامپوننت استفاده می‌شوند.
✅ **Composables** امکان استفاده مجدد از منطق را فراهم می‌کنند.

---
