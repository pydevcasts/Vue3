### **📌 صفحه 3 - سینتکس (Syntax) در Vue.js**

در این صفحه، **سینتکس‌های مختلف در Vue.js** را بررسی می‌کنیم. این بخش شامل نحوه استفاده از **دستورها، متغیرها، داده‌ها، رویدادها، شرط‌ها، حلقه‌ها، و روش‌های مختلف مدیریت داده‌ها** در Vue.js خواهد بود.

---

## **۱. مقداردهی داده‌ها با `data`**

📌 در Vue.js، داده‌های کامپوننت در بخش `data` تعریف می‌شوند و می‌توانند درون `template` استفاده شوند.

✅ **مثال:**

```vue
<script setup>
import { ref } from 'vue'

const message = ref('سلام Vue.js!')
</script>

<template>
  <p>{{ message }}</p>
</template>
```

📌 **توضیح:**
🔹 `ref()` برای ایجاد متغیرهای **ری‌اکتیو** (Reactive) استفاده می‌شود.
🔹 مقدار `message` در `template` با دو آکولاد (`{{ }}`) نمایش داده می‌شود.

---

## **۲. بایند کردن مقدار با `v-bind`**

📌 **`v-bind` برای متصل کردن مقادیر جاوااسکریپت به ویژگی‌های (Attributes) عناصر HTML استفاده می‌شود.**

✅ **مثال:**

```vue
<script setup>
import { ref } from 'vue'

const imageUrl = ref('https://picsum.photos/200')
</script>

<template>
  <img :src="imageUrl" alt="تصویر تصادفی">
</template>
```

📌 **توضیح:**
🔹 مقدار `imageUrl` به ویژگی `src` عنصر `<img>` متصل شده است.
🔹 `v-bind:src` کوتاه‌شده به `:src` نوشته شده است.

---

## **۳. مدیریت رویدادها با `v-on` یا `@`**

📌 **برای مدیریت رویدادها در Vue از `v-on` یا علامت `@` استفاده می‌شود.**

✅ **مثال:**

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

const increment = () => {
  count.value++
}
</script>

<template>
  <button @click="increment">افزایش: {{ count }}</button>
</template>
```

📌 **توضیح:**
🔹 `@click="increment"` باعث می‌شود هنگام کلیک، تابع `increment` اجرا شود.
🔹 مقدار `count` هنگام کلیک افزایش پیدا می‌کند.

---

## **۴. شرط‌گذاری با `v-if`، `v-else-if` و `v-else`**

📌 **برای نمایش یا مخفی کردن عناصر به‌صورت شرطی از `v-if` و `v-else` استفاده می‌کنیم.**

✅ **مثال:**

```vue
<script setup>
import { ref } from 'vue'

const isLoggedIn = ref(false)
</script>

<template>
  <button @click="isLoggedIn = !isLoggedIn">
    تغییر وضعیت
  </button>

  <p v-if="isLoggedIn">خوش آمدید!</p>
  <p v-else>لطفاً وارد شوید.</p>
</template>
```

📌 **توضیح:**
🔹 مقدار `isLoggedIn` تعیین می‌کند که کدام پاراگراف نمایش داده شود.
🔹 با کلیک روی دکمه، مقدار تغییر کرده و متن به‌روزرسانی می‌شود.

---

## **۵. حلقه‌ها با `v-for`**

📌 **برای تکرار آیتم‌ها از `v-for` استفاده می‌شود.**

✅ **مثال:**

```vue
<script setup>
import { ref } from 'vue'

const items = ref(['Vue.js', 'React', 'Angular'])
</script>

<template>
  <ul>
    <li v-for="(item, index) in items" :key="index">
      {{ item }}
    </li>
  </ul>
</template>
```

📌 **توضیح:**
🔹 `v-for` روی آرایه `items` حلقه می‌زند.
🔹 مقدار `:key="index"` برای بهینه‌سازی عملکرد استفاده می‌شود.

---

## **۶. دریافت مقدار ورودی از کاربر با `v-model`**

📌 **برای دریافت مقدار از `input` و ذخیره آن در متغیر، از `v-model` استفاده می‌شود.**

✅ **مثال:**

```vue
<script setup>
import { ref } from 'vue'

const username = ref('')
</script>

<template>
  <input v-model="username" placeholder="نام خود را وارد کنید">
  <p>نام شما: {{ username }}</p>
</template>
```

📌 **توضیح:**
🔹 مقدار ورودی کاربر به `username` متصل است.
🔹 هر تغییری در `input`، مقدار `username` را به‌روز می‌کند.

---

## **۷. محاسبات (Computed Properties) با `computed`**

📌 **وقتی نیاز داریم مقدار جدیدی بر اساس مقادیر دیگر محاسبه کنیم، از `computed` استفاده می‌کنیم.**

✅ **مثال:**

```vue
<script setup>
import { ref, computed } from 'vue'

const price = ref(100)
const quantity = ref(2)

const total = computed(() => price.value * quantity.value)
</script>

<template>
  <p>قیمت: {{ price }} دلار</p>
  <p>تعداد: {{ quantity }}</p>
  <p>جمع کل: {{ total }} دلار</p>
</template>
```

📌 **توضیح:**
🔹 مقدار `total` همیشه به‌صورت خودکار بر اساس `price` و `quantity` محاسبه می‌شود.

---

## **۸. اجرای کدهای خاص هنگام تغییر مقدار متغیر با `watch`**

📌 **اگر نیاز داشته باشیم هنگام تغییر مقدار یک متغیر، یک تابع اجرا شود، از `watch` استفاده می‌کنیم.**

✅ **مثال:**

```vue
<script setup>
import { ref, watch } from 'vue'

const count = ref(0)

watch(count, (newValue, oldValue) => {
  console.log(`مقدار شمارنده از ${oldValue} به ${newValue} تغییر کرد.`)
})
</script>

<template>
  <button @click="count++">افزایش: {{ count }}</button>
</template>
```

📌 **توضیح:**
🔹 هر بار مقدار `count` تغییر کند، مقدار جدید و قبلی در `console.log` نمایش داده می‌شود.

---

## **📌 جمع‌بندی این صفحه**

✅ **Vue.js سینتکس ساده و منعطفی دارد که کار با آن را بسیار راحت می‌کند.**
✅ **مقداردهی داده‌ها با `data` و `ref()` انجام می‌شود.**
✅ **رویدادها با `@click` و `v-on` مدیریت می‌شوند.**
✅ **شرط‌ها با `v-if` و حلقه‌ها با `v-for` کنترل می‌شوند.**
✅ **داده‌ها را می‌توان با `v-model` به ورودی‌ها متصل کرد.**
✅ **`computed` برای مقادیر محاسباتی و `watch` برای نظارت بر تغییرات داده‌ها استفاده می‌شود.**

---
