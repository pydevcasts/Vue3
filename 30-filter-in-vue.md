# **📌 صفحه ۳۰ - فیلترها (Filters) در Vue.js**

در این صفحه، به بررسی **فیلترها (Filters)** در Vue.js می‌پردازیم و یاد می‌گیریم که چگونه می‌توان از آن‌ها برای **فرمت‌دهی داده‌ها** در قالب (Template) استفاده کرد.

✅ **فیلتر چیست و چرا از آن استفاده می‌کنیم؟**
✅ **تعریف و استفاده از فیلترها در Vue.js**
✅ **فیلترهای محلی و سراسری**
✅ **ترکیب فیلترها برای پردازش پیشرفته**
✅ **محدودیت‌های فیلترها و جایگزین‌های مدرن**

---

## **۱. فیلتر چیست و چرا از آن استفاده می‌کنیم؟**

🔹 فیلترها در Vue.js برای **پردازش و فرمت‌دهی داده‌ها** در داخل قالب (Template) استفاده می‌شوند.
🔹 به‌عنوان مثال، اگر بخواهیم **تاریخ‌ها، متن‌ها یا مقادیر عددی** را قبل از نمایش تغییر دهیم، فیلترها به ما کمک می‌کنند.

📌 **مثال: تبدیل متن به حروف بزرگ با فیلتر**

```vue
<p>{{ message | uppercase }}</p>
```

✅ مقدار `message` از طریق **فیلتر `uppercase`** پردازش شده و به **حروف بزرگ** تبدیل می‌شود.

---

## **۲. تعریف و استفاده از فیلترها در Vue.js**

### **📌 تعریف فیلتر در Vue 2**

🔹 در Vue 2، می‌توان فیلترها را **در سطح سراسری (Global)** یا **در سطح محلی (Local)** تعریف کرد.

📌 **۱. تعریف فیلتر سراسری (Global Filter)**
**در این روش، فیلتر در کل برنامه قابل استفاده است.**

```js
Vue.filter('uppercase', function(value) {
  if (!value) return ''
  return value.toString().toUpperCase()
})
```

✅ اکنون می‌توانیم در هر کامپوننت از این فیلتر استفاده کنیم:

```vue
<p>{{ 'hello world' | uppercase }}</p>
```

🔹 خروجی: **HELLO WORLD**

📌 **۲. تعریف فیلتر محلی (Local Filter) در کامپوننت**
🔹 در این روش، فیلتر فقط در همان کامپوننت قابل استفاده است.

```vue
<script>
export default {
  filters: {
    capitalize(value) {
      if (!value) return ''
      return value.charAt(0).toUpperCase() + value.slice(1)
    }
  }
}
</script>

<template>
  <p>{{ 'vue js' | capitalize }}</p>
</template>
```

🔹 خروجی: **Vue js**

---

## **۳. ترکیب فیلترها در Vue.js**

🔹 فیلترها را می‌توان **زنجیره‌ای ترکیب** کرد تا چندین عملیات روی یک مقدار انجام شود.
🔹 برای این کار، کافی است فیلترها را با `|` جدا کنیم.

📌 **مثال: ترکیب دو فیلتر برای تبدیل متن به حروف بزرگ و سپس اضافه کردن سه نقطه**

```vue
Vue.filter('uppercase', function(value) {
  return value.toUpperCase()
})

Vue.filter('truncate', function(value, length) {
  return value.length > length ? value.substring(0, length) + '...' : value
})
```

✅ اکنون می‌توان از هر دو فیلتر به‌صورت ترکیبی استفاده کرد:

```vue
<p>{{ 'Hello Vue.js Filters' | uppercase | truncate(10) }}</p>
```

🔹 خروجی: **HELLO VUE...**

---

## **۴. جایگزین‌های مدرن برای فیلترها در Vue 3**

🔴 **در Vue 3 فیلترها حذف شده‌اند!**
🔹 به جای فیلترها، از **Computed Properties** یا **Methods** استفاده می‌کنیم.

📌 **روش اول: استفاده از Computed Property**

```vue
<script setup>
import { computed, ref } from 'vue'

const message = ref('hello vue')

const uppercaseMessage = computed(() => {
  return message.value.toUpperCase()
})
</script>

<template>
  <p>{{ uppercaseMessage }}</p>
</template>
```

🔹 خروجی: **HELLO VUE**

📌 **روش دوم: استفاده از متدها (Methods)**

```vue
<script setup>
import { ref } from 'vue'

const message = ref('hello vue')

const uppercase = (text) => {
  return text.toUpperCase()
}
</script>

<template>
  <p>{{ uppercase(message) }}</p>
</template>
```

🔹 خروجی: **HELLO VUE**

---

## **📌 جمع‌بندی این صفحه**

✅ **فیلترها** در Vue 2 برای **فرمت‌دهی داده‌ها** در قالب استفاده می‌شدند.
✅ دو نوع فیلتر داریم: **سراسری (Global) و محلی (Local)**.
✅ فیلترها می‌توانند **ترکیب شوند** و چندین عملیات را روی مقدار انجام دهند.
✅ **در Vue 3 فیلترها حذف شده‌اند** و باید از **Computed Properties یا Methods** به‌عنوان جایگزین استفاده کنیم.

---
