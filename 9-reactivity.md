### **📌 صفحه 9 - ری‌اکتیو بودن داده‌ها در Vue.js**  

در این صفحه، با یکی از ویژگی‌های کلیدی Vue.js یعنی **ری‌اکتیو بودن داده‌ها (Reactivity)** آشنا خواهیم شد. این ویژگی باعث می‌شود که هر تغییری در داده‌ها، به‌طور خودکار در صفحه نمایش اعمال شود.  

---

### **۱. مفهوم ری‌اکتیویتی در Vue.js چیست؟**  

ری‌اکتیویتی در Vue.js به این معناست که **وقتی مقدار یک متغیر تغییر کند، رابط کاربری (UI) به‌طور خودکار به‌روزرسانی می‌شود.** به این ترتیب نیازی به اعمال تغییرات دستی در DOM نداریم.  

✅ **مثال ساده:**  
در کد زیر، مقدار **`message`** را تغییر می‌دهیم و Vue.js به‌طور خودکار مقدار جدید را در صفحه نمایش می‌دهد:  

```vue
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="changeMessage">تغییر متن</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "سلام Vue.js!"
    };
  },
  methods: {
    changeMessage() {
      this.message = "متن تغییر کرد!";
    }
  }
};
</script>
```

✅ **نتیجه:** وقتی روی دکمه کلیک کنید، مقدار `message` تغییر کرده و در صفحه نمایش داده می‌شود.

---

### **۲. ری‌اکتیویتی با `ref()` در Vue 3**  

در Vue 3، برای ایجاد متغیرهای **ری‌اکتیو** از `ref()` استفاده می‌کنیم. این قابلیت در **Composition API** معرفی شده است.  

📌 **کد نمونه برای استفاده از `ref()` در Vue 3:**  

```vue
<template>
  <div>
    <h2>عدد: {{ count }}</h2>
    <button @click="increaseCount">افزایش عدد</button>
  </div>
</template>

<script>
import { ref } from 'vue';

export default {
  setup() {
    const count = ref(0);

    function increaseCount() {
      count.value++;  // مقدار متغیر را افزایش می‌دهیم
    }

    return { count, increaseCount };
  }
};
</script>
```

✅ **نتیجه:** وقتی روی دکمه کلیک کنید، مقدار `count` افزایش می‌یابد و در صفحه نمایش داده می‌شود.

📌 **نکته:** در `ref()`, باید مقدار متغیر را با `.value` تغییر دهیم.

---

### **۳. ری‌اکتیویتی با `reactive()` در Vue 3**  

علاوه بر `ref()`, می‌توان از `reactive()` برای ایجاد **آبجکت‌های ری‌اکتیو** استفاده کرد.  

📌 **کد نمونه برای استفاده از `reactive()`:**  

```vue
<template>
  <div>
    <h2>نام: {{ user.name }}</h2>
    <h2>سن: {{ user.age }}</h2>
    <button @click="increaseAge">افزایش سن</button>
  </div>
</template>

<script>
import { reactive } from 'vue';

export default {
  setup() {
    const user = reactive({
      name: "علی",
      age: 25
    });

    function increaseAge() {
      user.age++;
    }

    return { user, increaseAge };
  }
};
</script>
```

✅ **نتیجه:** با کلیک روی دکمه، مقدار `age` افزایش می‌یابد و در صفحه نمایش داده می‌شود.

📌 **نکته:** در `reactive()`, نیازی به `.value` نداریم و می‌توانیم مقدار متغیرها را مستقیماً تغییر دهیم.

---

### **۴. مقایسه `ref()` و `reactive()`**  

| ویژگی            | `ref()` | `reactive()` |
|----------------|--------|-------------|
| **مناسب برای** | مقادیر ساده مانند `string`, `number`, `boolean` | آبجکت‌های پیچیده و چندمقداری |
| **نحوه استفاده** | `count.value = 5` | `user.age = 30` |
| **پشتیبانی در HTML** | نیاز به `.value` | نیازی به `.value` ندارد |
| **ترکیب با Composition API** | پرکاربرد | برای آبجکت‌ها بهتر است |

✅ **نتیجه:** اگر مقدار شما **ساده** است، از `ref()` استفاده کنید. اما اگر یک **آبجکت پیچیده** دارید، `reactive()` انتخاب بهتری است.

---

### **۵. استفاده از `watch()` برای نظارت بر تغییرات داده‌ها**  

در برخی مواقع، نیاز داریم که تغییر مقدار یک متغیر را **کنترل کنیم و براساس آن یک عملیات خاص انجام دهیم.** در این موارد، از `watch()` استفاده می‌کنیم.  

📌 **کد نمونه برای استفاده از `watch()`:**  

```vue
<template>
  <div>
    <h2>عدد: {{ count }}</h2>
    <button @click="increaseCount">افزایش عدد</button>
  </div>
</template>

<script>
import { ref, watch } from 'vue';

export default {
  setup() {
    const count = ref(0);

    function increaseCount() {
      count.value++;
    }

    watch(count, (newVal, oldVal) => {
      console.log(`مقدار قبلی: ${oldVal}, مقدار جدید: ${newVal}`);
    });

    return { count, increaseCount };
  }
};
</script>
```

✅ **نتیجه:** هر بار که مقدار `count` تغییر کند، مقدار قبلی و جدید در کنسول نمایش داده می‌شود.

📌 **نکته:** `watch()` برای نظارت بر تغییرات **یک مقدار خاص** استفاده می‌شود.

---

### **۶. استفاده از `computed()` برای محاسبات خودکار**  

`computed()` زمانی استفاده می‌شود که بخواهیم **یک مقدار وابسته به سایر متغیرها را محاسبه کنیم** و به‌طور خودکار به‌روزرسانی شود.

📌 **مثال محاسبه خودکار مقدار کل قیمت:**  

```vue
<template>
  <div>
    <h2>تعداد: {{ quantity }}</h2>
    <h2>قیمت واحد: {{ price }} تومان</h2>
    <h2>قیمت کل: {{ totalPrice }} تومان</h2>
    <button @click="increaseQuantity">افزایش تعداد</button>
  </div>
</template>

<script>
import { ref, computed } from 'vue';

export default {
  setup() {
    const quantity = ref(1);
    const price = ref(10000);

    const totalPrice = computed(() => {
      return quantity.value * price.value;
    });

    function increaseQuantity() {
      quantity.value++;
    }

    return { quantity, price, totalPrice, increaseQuantity };
  }
};
</script>
```

✅ **نتیجه:** وقتی دکمه را بزنید، مقدار `quantity` افزایش یافته و **`totalPrice` به‌طور خودکار محاسبه و نمایش داده می‌شود.**

📌 **نکته:** `computed()` فقط وقتی اجرا می‌شود که مقدار متغیرهای وابسته تغییر کند.

---

### **📌 جمع‌بندی این صفحه**  

✅ در Vue.js داده‌ها به‌صورت **ری‌اکتیو (Reactive)** هستند و با تغییر مقدار، UI به‌طور خودکار به‌روزرسانی می‌شود.  
✅ برای مدیریت داده‌ها می‌توان از `ref()` و `reactive()` استفاده کرد.  
✅ با `watch()` می‌توانیم تغییرات یک مقدار را نظارت کنیم.  
✅ با `computed()` می‌توان مقادیر وابسته را محاسبه کرد.  

