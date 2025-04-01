# 18. مسیریابی با Vue Router

در این فصل، با **Vue Router** آشنا می‌شویم که کتابخانه‌ی رسمی مسیریابی برای Vue.js است. Vue Router به ما اجازه می‌دهد **مسیرهای مختلف برنامه را مدیریت کنیم** و برای هر مسیر، کامپوننت مناسب را نمایش دهیم.

## نصب و راه‌اندازی Vue Router

📌 **نصب Vue Router با npm:**
```bash
npm install vue-router@4
```

### پیکربندی اولیه در `main.js`:

```javascript
import { createApp } from 'vue'
import { createRouter, createWebHistory } from 'vue-router'
import App from './App.vue'
import Home from './views/Home.vue'
import About from './views/About.vue'

const router = createRouter({
  history: createWebHistory(),
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Home
    },
    {
      path: '/about',
      name: 'About',
      component: About
    }
  ]
})

const app = createApp(App)
app.use(router)
app.mount('#app')
```

### استفاده در `App.vue`:

```vue
<template>
  <div>
    <nav>
      <router-link to="/">خانه</router-link> |
      <router-link to="/about">درباره ما</router-link>
    </nav>

    <router-view></router-view>
  </div>
</template>
```

## مسیریابی پویا (Dynamic Routes)

📌 **برای مسیرهای پویا از پارامتر استفاده می‌کنیم:**

```javascript
const routes = [
  {
    path: '/user/:id',
    name: 'User',
    component: User
  }
]
```

### دسترسی به پارامترها در کامپوننت:

```vue
<template>
  <div>
    <h2>پروفایل کاربر {{ $route.params.id }}</h2>
  </div>
</template>

<script>
import { useRoute } from 'vue-router'

export default {
  setup() {
    const route = useRoute()
    console.log(route.params.id)
  }
}
</script>
```

## مسیریابی تو در تو (Nested Routes)

📌 **می‌توانیم مسیرها را به صورت تو در تو تعریف کنیم:**

```javascript
const routes = [
  {
    path: '/user/:id',
    component: User,
    children: [
      {
        path: 'profile',
        component: UserProfile
      },
      {
        path: 'posts',
        component: UserPosts
      }
    ]
  }
]
```

### استفاده در کامپوننت والد:

```vue
<template>
  <div>
    <h2>پروفایل کاربر {{ $route.params.id }}</h2>
    
    <nav>
      <router-link :to="`/user/${$route.params.id}/profile`">پروفایل</router-link> |
      <router-link :to="`/user/${$route.params.id}/posts`">پست‌ها</router-link>
    </nav>

    <router-view></router-view>
  </div>
</template>
```

## گاردهای مسیریابی (Navigation Guards)

📌 **برای کنترل دسترسی به مسیرها از گاردها استفاده می‌کنیم:**

### گارد سراسری:

```javascript
router.beforeEach((to, from) => {
  // اگر کاربر لاگین نکرده باشد و مسیر نیاز به احراز هویت داشته باشد
  if (!isAuthenticated && to.meta.requiresAuth) {
    return { name: 'Login' }
  }
})
```

### گارد در مسیر:

```javascript
const routes = [
  {
    path: '/admin',
    component: Admin,
    beforeEnter: (to, from) => {
      // بررسی دسترسی ادمین
      if (!isAdmin) {
        return false
      }
    }
  }
]
```

### گارد در کامپوننت:

```vue
<script>
export default {
  beforeRouteEnter(to, from, next) {
    // این کامپوننت هنوز ایجاد نشده است
    next(vm => {
      // دسترسی به نمونه کامپوننت با `vm`
    })
  },
  
  beforeRouteUpdate(to, from) {
    // وقتی مسیر تغییر می‌کند اما کامپوننت دوباره استفاده می‌شود
  },
  
  beforeRouteLeave(to, from) {
    // قبل از خروج از مسیر
    const answer = window.confirm('آیا مطمئن هستید؟')
    if (!answer) return false
  }
}
</script>
```

## مدیریت اسکرول

📌 **می‌توانیم رفتار اسکرول را هنگام تغییر مسیر کنترل کنیم:**

```javascript
const router = createRouter({
  history: createWebHistory(),
  routes: [...],
  scrollBehavior(to, from, savedPosition) {
    if (savedPosition) {
      return savedPosition
    } else {
      return { top: 0 }
    }
  }
})
```

## لود تنبل (Lazy Loading)

📌 **برای بهبود عملکرد، می‌توانیم کامپوننت‌ها را به صورت تنبل لود کنیم:**

```javascript
const routes = [
  {
    path: '/about',
    name: 'About',
    component: () => import('./views/About.vue')
  }
]
```

## نکات مهم

✅ **بهترین شیوه‌ها:**
1. از نام‌گذاری مسیرها استفاده کنید
2. مسیرهای پربازدید را به صورت عادی و بقیه را به صورت تنبل لود کنید
3. از متا فیلدها برای اطلاعات اضافی مسیرها استفاده کنید
4. گاردهای مسیریابی را برای کنترل دسترسی استفاده کنید

## جمع‌بندی

✅ **Vue Router ابزار قدرتمندی برای مدیریت مسیرها در Vue.js است**
✅ **می‌توان مسیرهای پویا و تو در تو تعریف کرد**
✅ **گاردهای مسیریابی برای کنترل دسترسی استفاده می‌شوند**
✅ **لود تنبل به بهبود عملکرد برنامه کمک می‌کند**
