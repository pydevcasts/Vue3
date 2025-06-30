# **📌 صفحه 6 - مبانی کامپوننت‌ها در Vue.js**

در این صفحه، به بررسی **مبانی کامپوننت‌ها** در Vue.js می‌پردازیم. کامپوننت‌ها بخش‌های **قابل‌استفاده مجدد از رابط کاربری** هستند که می‌توانند داده‌ها و رویدادها را مدیریت کنند. این صفحه شامل موضوعات زیر است:

✅ **تعریف و ثبت کامپوننت‌ها**
✅ **ارسال داده با Props**
✅ **مدیریت رویدادها در کامپوننت‌ها**

---

## **۱. تعریف و ثبت کامپوننت‌ها**

📌 در Vue.js، یک **کامپوننت** یک بخش **مجزا از رابط کاربری** است که می‌توان آن را در بخش‌های مختلف پروژه استفاده کرد.

✅ **ساخت یک کامپوننت ساده:**
ابتدا یک فایل جدید برای کامپوننت ایجاد کنید.

📁 **ساختار فایل‌ها:**

```
components/
  HelloWorld.vue
```

✅ **کد `HelloWorld.vue`:**

```vue
<template>
  <h3>{{ props.username }}</h3>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps(["username"])

// or 
// const props = defineProps({
//   username: {
//     type: String,
//     required: true
//   },
// });
</script>
```

✅ **استفاده از این کامپوننت در `App.vue`:**

```vue
<script setup>
import HelloWorld from './components/HelloWorld.vue'
</script>

<template>
  <HelloWorld username="Vue.js"/>
</template>
```

📌 **توضیح:**
🔹 مقدار `name` با `Props` دریافت شده و درون `h1` نمایش داده می‌شود.
🔹 این کامپوننت **مستقل** است و می‌توان آن را در چندین بخش استفاده کرد.

---

## **۲. ارسال داده به کامپوننت‌ها با `Props`**

📌 **Props** به ما اجازه می‌دهند تا مقادیر را از **والد (Parent)** به **فرزند (Child)** ارسال کنیم.

✅ **مثال:**
📁 **`UserProfile.vue`**

```vue
<script setup>
defineProps(['username', 'age'])
</script>

<template>
  <div>
    <h2>نام کاربری: {{ username }}</h2>
    <p>سن: {{ age }}</p>
  </div>
</template>
```

✅ **استفاده از کامپوننت در `App.vue`:**

```vue
<script setup>
import UserProfile from './components/UserProfile.vue'
</script>

<template>
  <UserProfile username="Ali" :age="25"/>
</template>
```

📌 **توضیح:**
🔹 مقدار `username="Ali"` و مقدار `age` به‌صورت `:age="25"` ارسال شده است.
🔹 برای `age` از `:` استفاده کردیم زیرا یک مقدار **عددی** است.

---
✅ **مثال دیگر:**
```vue
<template>
  <div class="hello">
    <h3>سلام {{ props.username }}!</h3>
    <div class="profile-info" v-if="showInfo">
      <p>سن: {{ props.age }}</p>
      <p>شهر: {{ props.city }}</p>
    </div>
    <button @click="toggleInfo">
      {{ showInfo ? 'مخفی کردن اطلاعات' : 'نمایش اطلاعات' }}
    </button>
    <div class="message" v-if="message">
      {{ message }}
    </div>
  </div>
</template>

<script setup>
import { ref, defineProps } from 'vue';

// تعریف props با استفاده از defineProps
const props = defineProps({
  username: {
    type: String,
    required: true,
    default: 'کاربر'
  },
  age: {
    type: Number,
    default: 25
  },
  city: {
    type: String,
    default: 'تهران'
  }
})

// تعریف متغیرهای واکنش‌گرا
const showInfo = ref(false)
const message = ref('')

// تابع تغییر وضعیت نمایش اطلاعات
const toggleInfo = () => {
  showInfo.value = !showInfo.value
  message.value = showInfo.value ? 'اطلاعات نمایش داده شد' : ''
  setTimeout(() => {
    message.value = ''
  }, 2000)
}
</script>

<style scoped>
.hello {
  text-align: center;
  padding: 20px;
}

.profile-info {
  margin: 15px 0;
}

.message {
  margin-top: 15px;
}
</style>
```
✅ **استفاده از کامپوننت در `App.vue`:**

```vue
<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <HelloWorld 
  username="علی"
  :age="30"
  city="اصفهان"
/>
</template>
```
---

## **۳. مدیریت رویدادها در کامپوننت‌ها**

📌 برای مدیریت رویدادها، از `$emit` برای ارسال رویداد از **فرزند به والد** استفاده می‌شود.

✅ **مثال:**
📁 **`Counter.vue` (کامپوننت شمارنده)**

```vue
<script setup>
import { ref, defineEmits } from 'vue'

const count = ref(0)
const emit = defineEmits(['increment'])

const increment = () => {
  count.value++
  emit('increment', count.value)
}
</script>

<template>
  <button @click="increment">افزایش: {{ count }}</button>
</template>
```

✅ **استفاده از این کامپوننت در `App.vue`:**

```vue
<script setup>
import Counter from './components/Counter.vue'
import { ref } from 'vue'

const totalCount = ref(0)

const handleIncrement = (newCount) => {
  totalCount.value = newCount
}
</script>

<template>
  <h1>شمارنده کل: {{ totalCount }}</h1>
  <Counter @increment="handleIncrement"/>
</template>
```

📌 **توضیح:**
🔹 وقتی دکمه کلیک شود، مقدار `count` افزایش یافته و به **والد** ارسال می‌شود.
🔹 `@increment="handleIncrement"` مقدار شمارنده را به‌روزرسانی می‌کند.

---

## **📌 جمع‌بندی این صفحه**

✅ **کامپوننت‌ها** بخش‌های **مستقل و قابل استفاده مجدد** از پروژه هستند.
✅ با **Props** داده‌ها را از **والد به فرزند** ارسال می‌کنیم.
✅ با `$emit` و `defineEmits` رویدادها را از **فرزند به والد** ارسال می‌کنیم.

---
