
  
  <script setup>
  import { defineAsyncComponent } from 'vue'
  
  const AsyncComponent = defineAsyncComponent({
    loader: () => import('./components/ListView.vue'),
    loadingComponent: {
      template: '<p>⏳ در حال بارگیری...</p>'
    },
    errorComponent: {
      template: '<p>❌ خطایی در بارگیری رخ داد.</p>'
    },
    delay: 2000,
    timeout: 3000 // زمان انتظار ۳ ثانیه قبل از نمایش خطا
  })
  </script>
  
  <template>
    <div>
      <h1>صفحه اصلی</h1>
 <AsyncComponent></AsyncComponent>
    </div>
  </template>
  