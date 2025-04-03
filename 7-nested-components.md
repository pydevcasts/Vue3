# **📌 صفحه 7 - کامپوننت‌های تودرتو و ارتباط بین آن‌ها در Vue.js**

در این صفحه، با نحوه **ساختاردهی کامپوننت‌ها** و ارتباط بین **والد (Parent) و فرزند (Child)** آشنا می‌شویم. همچنین، یک **مثال عملی** برای درک بهتر پیاده‌سازی می‌کنیم.

✅ **ساختاردهی کامپوننت‌ها**
✅ **ارتباط بین کامپوننت‌های والد و فرزند**
✅ **مثال عملی: ساخت یک لیست و آیتم‌های آن**

---

## **۱. ساختاردهی کامپوننت‌ها در Vue.js**

در پروژه‌های Vue.js، **تقسیم‌بندی کامپوننت‌ها** باعث خوانایی و نگه‌داری بهتر کد می‌شود.

📁 **ساختار پیشنهادی برای پروژه:**

```
components/
  List.vue
  ListItem.vue
App.vue
```

🔹 `List.vue` کامپوننت **والد** خواهد بود که لیستی از آیتم‌ها را مدیریت می‌کند.
🔹 `ListItem.vue` کامپوننت **فرزند** است که یک آیتم را نمایش می‌دهد.

---

## **۲. ارتباط والد و فرزند در Vue.js**

### **ارسال داده از والد به فرزند (`Props`)**

📌 والد می‌تواند با استفاده از **Props** داده‌ها را به فرزند ارسال کند.

✅ **ایجاد `ListItem.vue` برای نمایش یک آیتم:**
📁 `components/ListItem.vue`

```vue
<script setup>
defineProps(['title', 'description'])
</script>

<template>
  <div class="item">
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
  </div>
</template>

<style scoped>
.item {
  border: 1px solid #ccc;
  padding: 10px;
  margin: 5px;
  border-radius: 5px;
}
</style>
```

🔹 این کامپوننت یک **عنوان (title)** و **توضیح (description)** دریافت می‌کند.
🔹 داده‌ها از **کامپوننت والد** دریافت شده و نمایش داده می‌شوند.

---

### **مدیریت لیست در `List.vue` (کامپوننت والد)**

📁 `components/List.vue`

```vue
<script setup>
import ListItem from './ListItem.vue'

const items = [
  { id: 1, title: "آیتم اول", description: "توضیحات مربوط به آیتم اول" },
  { id: 2, title: "آیتم دوم", description: "توضیحات مربوط به آیتم دوم" },
  { id: 3, title: "آیتم سوم", description: "توضیحات مربوط به آیتم سوم" }
]
</script>

<template>
  <div>
    <h2>لیست آیتم‌ها</h2>
    <ListItem 
      v-for="item in items" 
      :key="item.id" 
      :title="item.title" 
      :description="item.description" 
    />
  </div>
</template>
```

🔹 این کامپوننت لیستی از **آیتم‌ها** را مدیریت کرده و برای هر **آیتم یک کامپوننت فرزند (`ListItem`)** ایجاد می‌کند.
🔹 داده‌ها از والد (`List.vue`) به فرزند (`ListItem.vue`) ارسال می‌شوند.

---

## **۳. ارتباط فرزند با والد (`Emit`)**

📌 **فرزند می‌تواند یک رویداد را اجرا کرده و اطلاعات را به والد ارسال کند.**
✅ **اضافه کردن دکمه حذف در `ListItem.vue`:**

📁 **ویرایش `ListItem.vue` برای ارسال رویداد به والد**

```vue
<script setup>
defineProps(['title', 'description'])
const emit = defineEmits(['remove'])

const removeItem = () => {
  emit('remove')
}
</script>

<template>
  <div class="item">
    <h3>{{ title }}</h3>
    <p>{{ description }}</p>
    <button @click="removeItem">❌ حذف</button>
  </div>
</template>
```

🔹 این کد یک دکمه **حذف** اضافه کرده که هنگام کلیک، یک **رویداد (`remove`)** به والد ارسال می‌کند.

✅ **مدیریت حذف آیتم در `List.vue`:**

📁 **ویرایش `List.vue` برای دریافت رویداد از فرزند**

```vue
<script setup>
import { ref } from 'vue'
import ListItem from './ListItem.vue'

const items = ref([
  { id: 1, title: "آیتم اول", description: "توضیحات مربوط به آیتم اول" },
  { id: 2, title: "آیتم دوم", description: "توضیحات مربوط به آیتم دوم" },
  { id: 3, title: "آیتم سوم", description: "توضیحات مربوط به آیتم سوم" }
])

const removeItem = (index) => {
  items.value.splice(index, 1)
}
</script>

<template>
  <div>
    <h2>لیست آیتم‌ها</h2>
    <ListItem 
      v-for="(item, index) in items" 
      :key="item.id" 
      :title="item.title" 
      :description="item.description" 
      @remove="removeItem(index)" 
    />
  </div>
</template>
```

🔹 **فرزند** هنگام کلیک روی دکمه، `remove` را اجرا کرده و به **والد** اطلاع می‌دهد.
🔹 **والد (`List.vue`)** مقدار `index` را دریافت کرده و آیتم را از لیست حذف می‌کند.

---

## **📌 جمع‌بندی این صفحه**

✅ **ساختاردهی کامپوننت‌ها** باعث افزایش خوانایی و مقیاس‌پذیری پروژه می‌شود.
✅ **Props** برای ارسال داده از **والد به فرزند** استفاده می‌شود.
✅ **Emit** برای ارسال رویداد از **فرزند به والد** کاربرد دارد.
✅ یک **مثال عملی** برای ایجاد یک **لیست داینامیک** پیاده‌سازی کردیم.

---
