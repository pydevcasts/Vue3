ایجاد یک پروژه **Todo List** با استفاده از Vue.js 3 شامل مراحل مختلفی است. در اینجا یک راهنمای گام‌به‌گام برای راه‌اندازی این پروژه ارائه می‌دهیم.

### **مرحله ۱: راه‌اندازی پروژه**

1. **نصب Vue CLI** (اگر قبلاً نصب نشده است):
   ```sh
   npm install -g @vue/cli
   ```

2. **ایجاد پروژه جدید**:
   ```sh
   vue create todo-list
   ```

   در این مرحله، گزینه‌های پیش‌فرض را انتخاب کنید یا تنظیمات شخصی‌سازی شده را انتخاب کنید.

3. **وارد پوشه پروژه شوید**:
   ```sh
   cd todo-list
   ```

4. **نصب Vue Router** (برای مدیریت مسیرها):
   ```sh
   vue add router
   ```

5. **نصب Pinia** (برای مدیریت وضعیت):
   ```sh
   npm install pinia
   ```

### **مرحله ۲: تنظیم ساختار پروژه**

ساختار پروژه به صورت زیر خواهد بود:

```
src/
|-- assets/         // فایل‌های استاتیک (تصاویر، CSS)
|-- components/     // کامپوننت‌های عمومی
|-- views/          // صفحات اصلی
|-- store/          // مدیریت وضعیت با Pinia
|-- router/         // فایل‌های مربوط به مسیریابی
|-- App.vue         // کامپوننت اصلی
|-- main.js         // نقطه ورودی
```

### **مرحله ۳: ایجاد Store با Pinia**

1. **ایجاد یک فایل Store**:
   در پوشه `src/store` یک فایل جدید به نام `todoStore.js` ایجاد کنید.

   ```js
   // src/store/todoStore.js
   import { defineStore } from 'pinia';

   export const useTodoStore = defineStore('todos', {
     state: () => ({
       todos: [],
     }),
     actions: {
       addTodo(todo) {
         this.todos.push(todo);
       },
       removeTodo(index) {
         this.todos.splice(index, 1);
       },
       toggleTodo(index) {
         this.todos[index].completed = !this.todos[index].completed;
       },
     },
   });
   ```

### **مرحله ۴: ایجاد کامپوننت Todo**

1. **ایجاد کامپوننت Todo**:
   در پوشه `src/components` یک فایل جدید به نام `TodoItem.vue` ایجاد کنید.

   ```vue
   <template>
     <div class="todo-item">
       <input type="checkbox" v-model="todo.completed" @change="toggleTodo" />
       <span :class="{ completed: todo.completed }">{{ todo.text }}</span>
       <button @click="removeTodo">حذف</button>
     </div>
   </template>

   <script>
   import { defineProps } from 'vue';

   export default {
     props: {
       todo: Object,
       index: Number,
     },
     methods: {
       toggleTodo() {
         this.$emit('toggle', this.index);
       },
       removeTodo() {
         this.$emit('remove', this.index);
       },
     },
   };
   </script>

   <style scoped>
   .completed {
     text-decoration: line-through;
   }
   </style>
   ```

### **مرحله ۵: ایجاد صفحه TodoList**

1. **ایجاد صفحه TodoList**:
   در پوشه `src/views` یک فایل جدید به نام `TodoList.vue` ایجاد کنید.

   ```vue
   <template>
     <div>
       <h1>لیست کارها</h1>
       <input v-model="newTodo" @keyup.enter="addTodo" placeholder="کار جدید را وارد کنید" />
       <div v-for="(todo, index) in todos" :key="index">
         <TodoItem :todo="todo" :index="index" @toggle="toggleTodo" @remove="removeTodo" />
       </div>
     </div>
   </template>

   <script>
   import { ref } from 'vue';
   import { useTodoStore } from '../store/todoStore';
   import TodoItem from '../components/TodoItem.vue';

   export default {
     components: {
       TodoItem,
     },
     setup() {
       const store = useTodoStore();
       const newTodo = ref('');

       const addTodo = () => {
         if (newTodo.value.trim()) {
           store.addTodo({ text: newTodo.value, completed: false });
           newTodo.value = '';
         }
       };

       const toggleTodo = (index) => {
         store.toggleTodo(index);
       };

       const removeTodo = (index) => {
         store.removeTodo(index);
       };

       return {
         newTodo,
         todos: store.todos,
         addTodo,
         toggleTodo,
         removeTodo,
       };
     },
   };
   </script>
   ```

### **مرحله ۶: تنظیم Router**

1. **ویرایش فایل Router**:
   در `src/router/index.js` مسیرها را تنظیم کنید.

   ```js
   import { createRouter, createWebHistory } from 'vue-router';
   import TodoList from '../views/TodoList.vue';

   const routes = [
     {
       path: '/',
       name: 'Home',
       component: TodoList,
     },
   ];

   const router = createRouter({
     history: createWebHistory(),
     routes,
   });

   export default router;
   ```

### **مرحله ۷: ویرایش فایل اصلی App.vue**

1. **ویرایش `App.vue`**:
   در `src/App.vue` فایل را به صورت زیر ویرایش کنید:

   ```vue
   <template>
     <div id="app">
       <router-view />
     </div>
   </template>

   <script>
   export default {
     name: 'App',
   };
   </script>

   <style>
   #app {
     max-width: 600px;
     margin: 0 auto;
     padding: 20px;
     font-family: Arial, sans-serif;
   }
   </style>
   ```

### **مرحله ۸: اجرای پروژه**

1. **اجرای پروژه**:
   حالا می‌توانید پروژه را اجرا کنید:

   ```sh
   npm run serve
   ```

2. **مشاهده در مرورگر**:
   به آدرس `http://localhost:8080` بروید و اپلیکیشن Todo List خود را مشاهده کنید.
