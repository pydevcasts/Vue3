### **📌 صفحه 5 - مدیریت فرم‌ها و ورودی‌ها در Vue.js**  

در این صفحه، به بررسی **مدیریت فرم‌ها و ورودی‌ها** در Vue.js می‌پردازیم. یاد می‌گیریم چگونه داده‌های فرم را با Vue.js مدیریت کنیم، **ورودی‌های مختلف را مقداردهی کنیم، چک‌باکس و رادیو باتن‌ها را کنترل کنیم و فرم‌ها را اعتبارسنجی کنیم.**  

---

## **۱. اتصال داده‌ها به ورودی‌ها با `v-model`**  

📌 در Vue.js، می‌توان با استفاده از `v-model` مقدار ورودی‌ها را به داده‌ها متصل کرد. این ویژگی باعث می‌شود مقدار ورودی به‌صورت **دوطرفه (Two-way Binding)** به‌روزرسانی شود.  

✅ **مثال:**  
```vue
<script setup>
import { ref } from 'vue'

const username = ref('')
</script>

<template>
  <label>نام کاربری:</label>
  <input v-model="username" placeholder="نام خود را وارد کنید">
  <p>نام وارد شده: {{ username }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدار `username` در `input` و `p` همزمان نمایش داده می‌شود.  
🔹 هر تغییری در فیلد ورودی، مقدار `username` را به‌روزرسانی می‌کند.  

---

## **۲. کار با چک‌باکس‌ها در Vue.js**  

📌 چک‌باکس‌ها را می‌توان به **یک مقدار بولی (`true` یا `false`)** متصل کرد یا **چند مقدار را در آرایه ذخیره کرد.**  

✅ **چک‌باکس ساده (یک مقدار بولی):**  
```vue
<script setup>
import { ref } from 'vue'

const isChecked = ref(false)
</script>

<template>
  <label>
    <input type="checkbox" v-model="isChecked">
    آیا موافق هستید؟
  </label>
  <p>وضعیت: {{ isChecked ? 'بله' : 'خیر' }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدار `isChecked` مقدار `true` یا `false` را در خود ذخیره می‌کند.  

✅ **چندین چک‌باکس (انتخاب چند گزینه و ذخیره در آرایه):**  
```vue
<script setup>
import { ref } from 'vue'

const selectedItems = ref([])
</script>

<template>
  <label><input type="checkbox" value="Vue" v-model="selectedItems"> Vue.js</label>
  <label><input type="checkbox" value="React" v-model="selectedItems"> React</label>
  <label><input type="checkbox" value="Angular" v-model="selectedItems"> Angular</label>

  <p>انتخاب شده: {{ selectedItems.join(', ') }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدارهای انتخاب‌شده در آرایه `selectedItems` ذخیره می‌شوند.  
🔹 هر بار که یک گزینه انتخاب یا حذف شود، مقدار آرایه تغییر می‌کند.  

---

## **۳. کار با دکمه‌های رادیویی (Radio Buttons)**  

📌 در Vue.js، دکمه‌های رادیویی می‌توانند به یک مقدار متغیر متصل شوند.  

✅ **مثال:**  
```vue
<script setup>
import { ref } from 'vue'

const selectedOption = ref('')
</script>

<template>
  <label><input type="radio" value="مرد" v-model="selectedOption"> مرد</label>
  <label><input type="radio" value="زن" v-model="selectedOption"> زن</label>

  <p>انتخاب شما: {{ selectedOption }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدار `selectedOption` با انتخاب گزینه تغییر می‌کند.  
🔹 فقط یک مقدار می‌تواند انتخاب شود.  

---

## **۴. کار با `select` و گزینه‌های انتخابی (`option`)**  

📌 در Vue.js، می‌توان مقدار انتخاب‌شده را مستقیماً در یک متغیر ذخیره کرد.  

✅ **مثال:**  
```vue
<script setup>
import { ref } from 'vue'

const selectedCity = ref('')
</script>

<template>
  <label>انتخاب شهر:</label>
  <select v-model="selectedCity">
    <option disabled value="">یک گزینه انتخاب کنید</option>
    <option value="تهران">تهران</option>
    <option value="مشهد">مشهد</option>
    <option value="اصفهان">اصفهان</option>
  </select>

  <p>شهر انتخاب شده: {{ selectedCity }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدار `selectedCity` با انتخاب گزینه تغییر می‌کند.  
🔹 مقدار `disabled` در اولین گزینه مانع از انتخاب آن به‌عنوان مقدار پیش‌فرض می‌شود.  

✅ **انتخاب چند گزینه (Multiple Select):**  
```vue
<script setup>
import { ref } from 'vue'

const selectedCities = ref([])
</script>

<template>
  <label>انتخاب شهرها:</label>
  <select v-model="selectedCities" multiple>
    <option value="تهران">تهران</option>
    <option value="مشهد">مشهد</option>
    <option value="اصفهان">اصفهان</option>
  </select>

  <p>شهرهای انتخاب شده: {{ selectedCities.join(', ') }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدار `selectedCities` یک **آرایه** است که مقدارهای انتخاب‌شده را ذخیره می‌کند.  

---

## **۵. اعتبارسنجی فرم‌ها در Vue.js**  

📌 **برای اعتبارسنجی ورودی‌ها می‌توان از شرط‌ها استفاده کرد.**  

✅ **مثال:**  
```vue
<script setup>
import { ref } from 'vue'

const email = ref('')
const errorMessage = ref('')

const validateEmail = () => {
  if (!email.value.includes('@')) {
    errorMessage.value = 'ایمیل نامعتبر است!'
  } else {
    errorMessage.value = ''
  }
}
</script>

<template>
  <label>ایمیل:</label>
  <input v-model="email" @input="validateEmail">
  <p style="color: red;" v-if="errorMessage">{{ errorMessage }}</p>
</template>
```

📌 **توضیح:**  
🔹 مقدار `email` در `input` ذخیره می‌شود.  
🔹 تابع `validateEmail` بررسی می‌کند که مقدار ورودی شامل `@` باشد.  

---

## **۶. ارسال فرم و مدیریت `submit`**  

📌 **برای ارسال فرم و جلوگیری از بارگذاری مجدد صفحه، می‌توان از رویداد `submit` استفاده کرد.**  

✅ **مثال:**  
```vue
<script setup>
import { ref } from 'vue'

const username = ref('')
const password = ref('')
const message = ref('')

const submitForm = () => {
  if (!username.value || !password.value) {
    message.value = 'لطفاً تمام فیلدها را پر کنید.'
    return
  }
  message.value = `ورود موفقیت‌آمیز: ${username.value}`
}
</script>

<template>
  <form @submit.prevent="submitForm">
    <label>نام کاربری:</label>
    <input v-model="username">

    <label>رمز عبور:</label>
    <input type="password" v-model="password">

    <button type="submit">ورود</button>
  </form>

  <p>{{ message }}</p>
</template>
```

📌 **توضیح:**  
🔹 رویداد `@submit.prevent` باعث می‌شود فرم بدون **بارگذاری مجدد** ارسال شود.  
🔹 مقدار `username` و `password` بررسی می‌شوند و در صورت خالی بودن، پیام خطا نمایش داده می‌شود.  

---

## **📌 جمع‌بندی این صفحه**  

✅ اتصال فرم‌ها با `v-model` انجام می‌شود.  
✅ **چک‌باکس‌ها، دکمه‌های رادیویی و لیست‌های انتخابی** مقدار خود را در متغیرها ذخیره می‌کنند.  
✅ می‌توان **اعتبارسنجی فرم‌ها** را با شرط‌ها انجام داد.  
✅ رویداد `@submit.prevent` برای **مدیریت ارسال فرم بدون بارگذاری مجدد** استفاده می‌شود.  

---
