### **📌 صفحه ۱5 - انیمیشن‌ها و ترانزیشن‌ها در Vue 3**  

در این صفحه، ویژگی **انیمیشن‌ها (Animations) و ترانزیشن‌ها (Transitions)** در Vue 3 را بررسی می‌کنیم و یاد می‌گیریم که چگونه به عناصر Vue جلوه‌های حرکتی اضافه کنیم.  

---

## **۱. مقدمه‌ای بر انیمیشن‌ها و ترانزیشن‌ها در Vue 3**  

📌 در Vue 3، می‌توانیم از **ویژگی داخلی `Transition`** برای ایجاد افکت‌های حرکتی هنگام **اضافه شدن، حذف شدن یا تغییر وضعیت یک عنصر در `DOM`** استفاده کنیم.  

✅ **ترانزیشن (Transition)**: مخصوص تغییر وضعیت عناصر مانند **ورود (`enter`) و خروج (`leave`) از صفحه است.  
✅ **انیمیشن (Animation)**: مخصوص جلوه‌های پیچیده‌تر است و می‌توان با استفاده از **CSS animations** یا **JavaScript hooks** آن را کنترل کرد.  

---

## **۲. استفاده از `Transition` برای انیمیشن‌های ساده**  

📌 Vue یک کامپوننت داخلی به نام **`<Transition>`** دارد که می‌توانیم آن را دور یک عنصر قرار دهیم تا هنگام ظاهر شدن یا محو شدن، انیمیشن اجرا شود.  

✅ **مثال: افکت ظاهر و محو شدن متن با `Transition`**  

```vue
<template>
  <div>
    <button @click="show = !show">Toggle Text</button>
    <Transition name="fade">
      <p v-if="show" class="message">Hello Vue 3!</p>
    </Transition>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const show = ref(false);
    return { show };
  },
};
</script>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
```

✅ **توضیح کد:**  
✔ دکمه روی کلیک مقدار `show` را تغییر می‌دهد.  
✔ عنصر `<p>` فقط زمانی که `show` مقدار `true` داشته باشد نمایش داده می‌شود.  
✔ کلاس‌های `fade-enter-active` و `fade-leave-active` یک افکت **تغییر شفافیت (opacity) در 0.5 ثانیه** ایجاد می‌کنند.  
✔ هنگام ورود (`fade-enter`)، مقدار `opacity` از `0` به `1` می‌رسد.  
✔ هنگام خروج (`fade-leave-to`)، مقدار `opacity` دوباره به `0` کاهش می‌یابد.  

---

## **۳. افکت‌های چندگانه ورود و خروج**  

📌 می‌توانیم برای ورود (`enter`) و خروج (`leave`) انیمیشن‌های متفاوتی تعریف کنیم.  

✅ **مثال: ترانزیشن متفاوت هنگام ورود و خروج**  

```vue
<template>
  <div>
    <button @click="showBox = !showBox">Toggle Box</button>
    <Transition name="slide">
      <div v-if="showBox" class="box"></div>
    </Transition>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const showBox = ref(false);
    return { showBox };
  },
};
</script>

<style>
.slide-enter-active {
  animation: slide-in 0.5s ease-out;
}
.slide-leave-active {
  animation: slide-out 0.5s ease-in;
}

@keyframes slide-in {
  from {
    transform: translateX(-100px);
    opacity: 0;
  }
  to {
    transform: translateX(0);
    opacity: 1;
  }
}

@keyframes slide-out {
  from {
    transform: translateX(0);
    opacity: 1;
  }
  to {
    transform: translateX(100px);
    opacity: 0;
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: royalblue;
  margin-top: 20px;
}
</style>
```

✅ **توضیح کد:**  
✔ هنگام ورود، جعبه از سمت چپ به جای اصلی خود حرکت می‌کند (`slide-in`).  
✔ هنگام خروج، جعبه به سمت راست حرکت کرده و محو می‌شود (`slide-out`).  
✔ از `@keyframes` برای تعریف انیمیشن‌ها استفاده شده است.  

---

## **۴. استفاده از `TransitionGroup` برای لیست‌ها**  

📌 اگر بخواهیم چندین آیتم با **افکت‌های جداگانه** وارد یا خارج شوند، از **`TransitionGroup`** استفاده می‌کنیم.  

✅ **مثال: لیست آیتم‌های متحرک**  

```vue
<template>
  <div>
    <button @click="addItem">Add Item</button>
    <button @click="removeItem">Remove Item</button>
    
    <TransitionGroup name="list">
      <p v-for="item in items" :key="item" class="list-item">{{ item }}</p>
    </TransitionGroup>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  setup() {
    const items = ref([1, 2, 3]);

    const addItem = () => {
      items.value.push(items.value.length + 1);
    };

    const removeItem = () => {
      items.value.pop();
    };

    return { items, addItem, removeItem };
  },
};
</script>

<style>
.list-enter-active, .list-leave-active {
  transition: all 0.5s ease;
}

.list-enter {
  opacity: 0;
  transform: translateY(-20px);
}

.list-leave-to {
  opacity: 0;
  transform: translateY(20px);
}

.list-item {
  background: lightblue;
  margin: 5px;
  padding: 10px;
  border-radius: 5px;
  text-align: center;
}
</style>
```

✅ **توضیح کد:**  
✔ هنگام اضافه شدن آیتم جدید، **آیتم‌ها با یک افکت ظاهر می‌شوند**.  
✔ هنگام حذف، **آیتم‌ها با افکت ناپدید می‌شوند**.  
✔ از `TransitionGroup` برای مدیریت تغییرات در لیست استفاده شده است.  

---

## **۵. ترکیب Vue با کتابخانه‌های انیمیشن‌سازی (GSAP, Animate.css)**  

📌 علاوه بر `Transition`، می‌توانیم از **کتابخانه‌های شخص ثالث** مانند **GSAP یا Animate.css** برای ایجاد انیمیشن‌های پیچیده استفاده کنیم.  

✅ **نصب `GSAP` در Vue 3:**  
```sh
npm install gsap
```

✅ **مثال: انیمیشن با GSAP**  

```vue
<template>
  <button @click="animateBox">Animate Box</button>
  <div ref="box" class="box"></div>
</template>

<script>
import { ref, onMounted } from "vue";
import gsap from "gsap";

export default {
  setup() {
    const box = ref(null);

    const animateBox = () => {
      gsap.to(box.value, { x: 200, duration: 1, ease: "power1.out" });
    };

    return { box, animateBox };
  },
};
</script>

<style>
.box {
  width: 100px;
  height: 100px;
  background: tomato;
  margin-top: 20px;
}
</style>
```

✅ **توضیح کد:**  
✔ `GSAP` برای متحرک‌سازی یک جعبه استفاده شده است.  
✔ با کلیک روی دکمه، جعبه به سمت راست حرکت می‌کند.  

---

## **📌 جمع‌بندی این صفحه**  

✅ Vue 3 دارای ویژگی داخلی `Transition` و `TransitionGroup` برای افکت‌های حرکتی است.  
✅ می‌توان از CSS animations و `@keyframes` برای انیمیشن‌های سفارشی استفاده کرد.  
✅ برای انیمیشن‌های پیشرفته‌تر، کتابخانه‌هایی مانند `GSAP` یا `Animate.css` کاربردی هستند.  
