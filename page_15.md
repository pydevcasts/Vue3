### **📌 صفحه ۱۵ - `async` و مدیریت داده‌های ناهمگام در Vue.js**  

در این صفحه، به بررسی روش‌های مدیریت داده‌های **ناهمگام (Asynchronous)** در Vue.js می‌پردازیم. برای دریافت داده‌ها از APIها یا انجام عملیات‌هایی که زمان‌بر هستند، باید از **`async/await`** و روش‌های مرتبط استفاده کنیم.  

---

## **۱. معرفی `async/await` در Vue.js**  

📌 **در Vue 3 می‌توان از `async/await` برای مدیریت درخواست‌های API، دریافت داده‌ها و سایر عملیات‌های غیرهم‌زمان استفاده کرد.**  

✅ **ساختار کلی `async/await`**  

```javascript
async function fetchData() {
  try {
    let response = await fetch("https://jsonplaceholder.typicode.com/posts");
    let data = await response.json();
    console.log(data);
  } catch (error) {
    console.error("Error fetching data:", error);
  }
}

fetchData();
```

✅ **توضیح کد:**  
1️⃣ **`fetchData` یک تابع `async` است که درخواست دریافت داده را ارسال می‌کند.**  
2️⃣ **`await` منتظر دریافت داده از API می‌ماند و برنامه را متوقف نمی‌کند.**  
3️⃣ در صورت بروز خطا، **بلاک `catch` اجرا شده و خطا نمایش داده می‌شود.**  

---

## **۲. استفاده از `async` در `setup()` - دریافت داده در Vue.js**  

📍 **فایل `App.vue`**  

```vue
<template>
  <div>
    <h2>Posts</h2>
    <ul>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
    <button @click="fetchPosts">Fetch Posts</button>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const posts = ref([]);

    const fetchPosts = async () => {
      try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts");
        posts.value = await response.json();
      } catch (error) {
        console.error("Error fetching posts:", error);
      }
    };

    return { posts, fetchPosts };
  }
};
</script>
```

✅ **توضیح کد:**  
1️⃣ **`posts` یک مقدار ری‌اکتیو است که لیست پست‌ها را ذخیره می‌کند.**  
2️⃣ تابع `fetchPosts` یک درخواست **`fetch`** را ارسال کرده و نتیجه را در **`posts.value`** ذخیره می‌کند.  
3️⃣ **پس از کلیک روی دکمه، داده‌ها دریافت شده و در لیست نمایش داده می‌شوند.**  

---

## **۳. مدیریت وضعیت بارگذاری (Loading State) در درخواست‌های `async`**  

📌 هنگام دریافت داده‌ها، ممکن است **چند ثانیه طول بکشد تا پاسخ از سرور برگردد.** در این مدت می‌توان **یک پیام "در حال بارگذاری"** نمایش داد.  

📍 **فایل `App.vue`**  

```vue
<template>
  <div>
    <h2>Posts</h2>
    <button @click="fetchPosts" :disabled="loading">
      {{ loading ? "Loading..." : "Fetch Posts" }}
    </button>
    <p v-if="loading">Loading posts...</p>
    <ul v-else>
      <li v-for="post in posts" :key="post.id">{{ post.title }}</li>
    </ul>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const posts = ref([]);
    const loading = ref(false);

    const fetchPosts = async () => {
      loading.value = true;
      try {
        let response = await fetch("https://jsonplaceholder.typicode.com/posts");
        posts.value = await response.json();
      } catch (error) {
        console.error("Error fetching posts:", error);
      } finally {
        loading.value = false;
      }
    };

    return { posts, fetchPosts, loading };
  }
};
</script>
```

✅ **توضیح کد:**  
1️⃣ مقدار **`loading`** برای نمایش وضعیت "در حال بارگذاری" اضافه شده است.  
2️⃣ هنگام دریافت داده‌ها، مقدار **`loading.value = true`** تنظیم می‌شود.  
3️⃣ پس از دریافت داده‌ها یا در صورت بروز خطا، مقدار **`loading.value = false`** می‌شود.  
4️⃣ دکمه در هنگام بارگذاری **غیرفعال** شده و متن آن تغییر می‌کند.  

---

## **۴. اجرای `async` در `onMounted` - دریافت داده در هنگام لود شدن کامپوننت**  

📌 در بسیاری از موارد، **داده‌ها هنگام لود شدن کامپوننت باید دریافت شوند.** برای این کار می‌توان **از `onMounted()`** استفاده کرد.  

📍 **فایل `App.vue`**  

```vue
<template>
  <div>
    <h2>Users</h2>
    <ul>
      <li v-for="user in users" :key="user.id">{{ user.name }}</li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from "vue";

export default {
  setup() {
    const users = ref([]);

    const fetchUsers = async () => {
      try {
        let response = await fetch("https://jsonplaceholder.typicode.com/users");
        users.value = await response.json();
      } catch (error) {
        console.error("Error fetching users:", error);
      }
    };

    onMounted(fetchUsers);

    return { users };
  }
};
</script>
```

✅ **توضیح کد:**  
1️⃣ هنگام **لود شدن کامپوننت، تابع `fetchUsers` به‌طور خودکار اجرا می‌شود.**  
2️⃣ داده‌های کاربران از API دریافت شده و در لیست نمایش داده می‌شود.  
3️⃣ این روش زمانی مفید است که بخواهیم **داده‌ها هنگام نمایش کامپوننت دریافت شوند.**  

---

## **۵. استفاده از `async` در `computed` - پردازش داده‌های ناهمگام**  

📌 در برخی موارد، می‌توان پردازش‌های ناهمگام را در **`computed`** انجام داد.  

📍 **مثال: فیلتر کردن داده‌ها پس از دریافت از API**  

```vue
<script>
import { ref, computed, onMounted } from "vue";

export default {
  setup() {
    const users = ref([]);
    const searchQuery = ref("");

    const fetchUsers = async () => {
      let response = await fetch("https://jsonplaceholder.typicode.com/users");
      users.value = await response.json();
    };

    onMounted(fetchUsers);

    const filteredUsers = computed(() => {
      return users.value.filter(user =>
        user.name.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });

    return { users, searchQuery, filteredUsers };
  }
};
</script>
```

✅ **توضیح کد:**  
1️⃣ مقدار **`searchQuery`** برای ذخیره متن جستجو اضافه شده است.  
2️⃣ در **`computed`** لیست کاربران فیلتر می‌شود تا فقط مواردی که شامل متن جستجو هستند نمایش داده شوند.  
3️⃣ داده‌ها **هنگام لود شدن کامپوننت دریافت می‌شوند.**  

---

## **📌 جمع‌بندی این صفحه**  

✅ **`async/await` برای دریافت داده‌های ناهمگام در Vue.js استفاده می‌شود.**  
✅ **می‌توان `async` را در `setup()`، `onMounted()` و `computed` استفاده کرد.**  
✅ **برای نمایش وضعیت بارگذاری، مقدار `loading` را مدیریت می‌کنیم.**  
✅ **برای دریافت داده در هنگام لود شدن کامپوننت، از `onMounted` استفاده می‌شود.**  

