### **📌 صفحه ۴ - دایرکتیوها (Directives) در Vue.js**  

دایرکتیوها در Vue.js ویژگی‌های خاصی در HTML هستند که رفتار المان‌ها را تغییر می‌دهند. این ویژگی‌ها با `v-` شروع می‌شوند و برای اعمال دستورات مختلف مانند نمایش/مخفی کردن عناصر، اجرای حلقه‌ها، مدیریت رویدادها و موارد دیگر استفاده می‌شوند.  

---

## **۱. معرفی دایرکتیوها در Vue.js**  

دایرکتیوها ویژگی‌هایی هستند که روی **تگ‌های HTML** اعمال می‌شوند و به Vue.js می‌گویند چگونه با آن المان تعامل داشته باشد. برخی از پرکاربردترین دایرکتیوها عبارت‌اند از:  

| دایرکتیو  | کاربرد |
|-----------|--------|
| `v-if` | نمایش یا مخفی کردن عناصر براساس شرط |
| `v-else` | نمایش عنصر در صورتی که شرط `v-if` برقرار نباشد |
| `v-show` | نمایش یا مخفی کردن عنصر به‌صورت CSS |
| `v-for` | ایجاد لیست‌های تکراری (Loop) |
| `v-bind` | اتصال داده‌ها به ویژگی‌های HTML |
| `v-model` | اتصال دوطرفه داده‌ها با فرم‌ها |
| `v-on` یا `@` | مدیریت رویدادها مانند کلیک، ورود اطلاعات و غیره |
| `v-html` | درج HTML به‌صورت داینامیک |
| `v-text` | درج متن داخل المان |
| `v-cloak` | پنهان کردن عناصر تا زمانی که Vue.js بارگذاری شود |

در ادامه به بررسی این دستورات با مثال‌های عملی می‌پردازیم.

---

## **۲. شرطی‌سازی با `v-if`, `v-else` و `v-show`**  

📌 **استفاده از `v-if` و `v-else`**  

```vue
<template>
  <div>
    <h2 v-if="isLoggedIn">خوش آمدید!</h2>
    <h2 v-else>لطفاً وارد شوید.</h2>
    <button @click="toggleLogin">تغییر وضعیت ورود</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const isLoggedIn = ref(false);

    function toggleLogin() {
      isLoggedIn.value = !isLoggedIn.value;
    }

    return { isLoggedIn, toggleLogin };
  }
};
</script>
```

✅ **نتیجه:** با کلیک روی دکمه، متن نمایش داده‌شده بین **"خوش آمدید!"** و **"لطفاً وارد شوید."** تغییر می‌کند.

📌 **استفاده از `v-show` برای نمایش/مخفی کردن عناصر:**  

```vue
<template>
  <div>
    <h2 v-show="isVisible">این متن نمایش داده می‌شود</h2>
    <button @click="toggleVisibility">تغییر نمایش</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const isVisible = ref(true);

    function toggleVisibility() {
      isVisible.value = !isVisible.value;
    }

    return { isVisible, toggleVisibility };
  }
};
</script>
```

✅ **تفاوت `v-if` و `v-show`**  
- `v-if` عنصر را **از DOM حذف می‌کند** و دوباره می‌سازد (عملکرد بهتری در بارگذاری اولیه دارد).  
- `v-show` عنصر را فقط **با `display: none;` مخفی می‌کند** (بهتر برای تغییرات سریع).  

---

## **۳. حلقه‌ها با `v-for`**  

📌 **تکرار عناصر با `v-for`**  

```vue
<template>
  <ul>
    <li v-for="(user, index) in users" :key="index">
      {{ index + 1 }} - {{ user.name }} (سن: {{ user.age }})
    </li>
  </ul>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const users = ref([
      { name: "علی", age: 25 },
      { name: "زهرا", age: 30 },
      { name: "مهدی", age: 22 }
    ]);

    return { users };
  }
};
</script>
```

✅ **نتیجه:** لیستی از کاربران نمایش داده می‌شود که مقدارشان از یک آرایه گرفته شده است.

📌 **نکته:** همیشه باید **`:key="index"`** را اضافه کنیم تا Vue.js عملکرد بهتری داشته باشد.

---

## **۴. اتصال داده‌ها به ویژگی‌های HTML با `v-bind`**  

📌 **مثال تغییر تصویر با `v-bind`**  

```vue
<template>
  <div>
    <img :src="imageUrl" alt="تصویر Vue.js" width="200">
    <button @click="changeImage">تغییر تصویر</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const imageUrl = ref("https://vuejs.org/images/logo.png");

    function changeImage() {
      imageUrl.value = "https://www.w3schools.com/vue/img_vue.jpg";
    }

    return { imageUrl, changeImage };
  }
};
</script>
```

✅ **نتیجه:** با کلیک روی دکمه، تصویر تغییر می‌کند.

---

## **۵. اتصال فرم‌ها با `v-model`**  

📌 **اتصال دوطرفه مقدار `input` و `textarea`**  

```vue
<template>
  <div>
    <input v-model="name" placeholder="نام خود را وارد کنید">
    <p>سلام، {{ name }}!</p>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const name = ref("");

    return { name };
  }
};
</script>
```

✅ **نتیجه:** هر تغییری در فیلد ورودی، در لحظه در `<p>` نمایش داده می‌شود.

---

## **۶. مدیریت رویدادها با `v-on` یا `@`**  

📌 **نمونه‌ای از استفاده از `v-on` برای رویداد کلیک**  

```vue
<template>
  <button @click="showAlert">کلیک کن</button>
</template>

<script>
export default {
  methods: {
    showAlert() {
      alert("دکمه کلیک شد!");
    }
  }
};
</script>
```

✅ **نتیجه:** وقتی دکمه کلیک شود، یک پیام هشدار ظاهر می‌شود.

📌 **کوتاه‌نویسی:**  
به جای `v-on:click` می‌توان از `@click` استفاده کرد:  

```vue
<button @click="showAlert">کلیک کن</button>
```

---

## **۷. درج HTML داینامیک با `v-html`**  

📌 **مثال درج مستقیم HTML از متغیر**  

```vue
<template>
  <div v-html="htmlContent"></div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const htmlContent = ref("<h2 style='color:red;'>این متن قرمز است!</h2>");

    return { htmlContent };
  }
};
</script>
```

✅ **نکته:** `v-html` می‌تواند کد مخرب را اجرا کند، پس بهتر است فقط برای داده‌های امن استفاده شود.

---

## **📌 جمع‌بندی این صفحه**  

✅ دایرکتیوهای Vue.js ویژگی‌هایی مانند **نمایش، شرطی‌سازی، حلقه‌ها، اتصال داده‌ها، کنترل رویدادها و تغییرات DOM** را مدیریت می‌کنند.  
✅ `v-if`, `v-else`, `v-show` برای نمایش شرطی استفاده می‌شوند.  
✅ `v-for` برای تکرار لیست‌ها کاربرد دارد.  
✅ `v-bind` و `v-model` برای اتصال داده‌ها به ویژگی‌های HTML استفاده می‌شوند.  
✅ `v-on` برای مدیریت رویدادها و `v-html` برای درج HTML داینامیک به کار می‌رود.  

  

