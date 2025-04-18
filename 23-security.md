### **📌 صفحه ۲۳ - امنیت در Vue.js**  

در این صفحه، به بررسی **نکات امنیتی در Vue.js** می‌پردازیم و یاد می‌گیریم که چگونه از **حملات رایج مانند XSS و CSRF جلوگیری کنیم**. همچنین، بهترین روش‌ها برای ایمن‌سازی پروژه‌های Vue را بررسی خواهیم کرد.  

---

## **۱. چرا امنیت در Vue.js مهم است؟ 🔒**  

📌 در یک برنامه **فرانت‌اند** مانند Vue.js، داده‌ها مستقیماً با کاربران تعامل دارند و در معرض خطر **حملات سایبری** قرار می‌گیرند.  

✅ **مهم‌ترین تهدیدات امنیتی در Vue.js:**  
✔ **حمله XSS (Cross-Site Scripting)** - تزریق اسکریپت مخرب در صفحات وب.  
✔ **حمله CSRF (Cross-Site Request Forgery)** - ارسال درخواست‌های ناخواسته از طرف کاربر.  
✔ **افشای داده‌ها** - نمایش اطلاعات حساس در کنسول یا شبکه.  
✔ **حملات تزریق (Injection Attacks)** - تزریق کدهای مخرب در API یا دیتابیس.  

---

## **۲. جلوگیری از حملات XSS (Cross-Site Scripting) 🚫**  

📌 **XSS** یکی از رایج‌ترین تهدیدات امنیتی است که مهاجمان می‌توانند کدهای مخرب جاوا اسکریپت را در برنامه تزریق کنند.  

✅ **چگونه XSS اتفاق می‌افتد؟**  
فرض کنید یک ورودی از کاربر دریافت می‌شود و بدون فیلتر شدن در صفحه نمایش داده می‌شود:  

```vue
<template>
  <div v-html="userInput"></div>
</template>

<script>
export default {
  data() {
    return {
      userInput: "<script>alert('Hacked!')</script>",
    };
  }
};
</script>
```

✅ **نتیجه:**  
🚨 این کد باعث می‌شود که اسکریپت مخرب در صفحه اجرا شود و امنیت برنامه را تهدید کند.  

✅ **راه‌حل:**  
❌ **هرگز از `v-html` برای نمایش محتوای ورودی کاربر استفاده نکنید.**  
✔ به جای آن از **binding ایمن (`{{ }}`)** استفاده کنید:  

```vue
<template>
  <div>{{ userInput }}</div>
</template>
```

✔ استفاده از **کتابخانه‌های ضد-XSS** مانند `DOMPurify`:  

```sh
npm install dompurify
```

```js
import DOMPurify from "dompurify";

export default {
  data() {
    return {
      userInput: ""
    };
  },
  computed: {
    sanitizedInput() {
      return DOMPurify.sanitize(this.userInput);
    }
  }
};
```

---

## **۳. جلوگیری از حملات CSRF (Cross-Site Request Forgery) 🛑**  

📌 **CSRF** زمانی رخ می‌دهد که یک مهاجم درخواست‌های مخربی را از طریق کاربر احراز هویت‌شده ارسال می‌کند.  

✅ **مثال یک حمله CSRF:**  
- کاربر به حساب بانکی خود وارد شده است.  
- مهاجم لینکی حاوی درخواست انتقال پول را برای کاربر ارسال می‌کند.  
- اگر درخواست بدون اعتبارسنجی ارسال شود، تراکنش انجام خواهد شد!  

✅ **راه‌حل:**  
✔ **استفاده از CSRF Token** در درخواست‌های POST:  

```js
axios.post("/api/transfer", { amount: 100 }, {
  headers: {
    "X-CSRF-Token": "token_ایمن"
  }
});
```

✔ **استفاده از HTTP `SameSite` Cookie** برای جلوگیری از ارسال کوکی در درخواست‌های خارجی.  

---

## **۴. جلوگیری از افشای داده‌های حساس**  

📌 بسیاری از توسعه‌دهندگان اطلاعات حساس مانند **توکن API یا کلیدهای خصوصی** را به اشتباه در فایل‌های قابل مشاهده قرار می‌دهند.  

✅ **راه‌حل:**  
✔ **عدم قرار دادن اطلاعات حساس در `client-side` و استفاده از `.env`**:  

```
VUE_APP_API_KEY=123456
```

✔ **هرگز اطلاعات حساس را در `localStorage` ذخیره نکنید** زیرا به راحتی توسط مهاجمان قابل دسترسی است.  

✔ استفاده از **HTTP Headers** مانند `Content Security Policy (CSP)` برای جلوگیری از اجرای کدهای مخرب:  

```sh
Content-Security-Policy: default-src 'self';
```

---

## **۵. جلوگیری از حملات تزریق (SQL Injection & Command Injection) 🔐**  

📌 اگر داده‌های ورودی مستقیماً به سرور ارسال شوند، ممکن است مهاجمان از طریق **SQL Injection** یا **Command Injection** به سیستم نفوذ کنند.  

✅ **راه‌حل:**  
✔ **هرگز داده‌های ورودی کاربر را مستقیماً پردازش نکنید.**  
✔ **از ORMهای ایمن مانند Prisma یا Sequelize برای ارتباط با دیتابیس استفاده کنید.**  

```js
const user = await db.users.findOne({
  where: { email: sanitizedInput }
});
```

✔ **فیلتر کردن و بررسی داده‌های ورودی** قبل از پردازش:  

```js
if (!/^[a-zA-Z0-9]+$/.test(userInput)) {
  throw new Error("Invalid input");
}
```

---

## **📌 جمع‌بندی این صفحه**  

✅ Vue.js به طور پیش‌فرض ایمن است، اما همچنان نیاز به **اقدامات امنیتی** برای جلوگیری از حملات دارد.  
✅ **XSS و CSRF** از رایج‌ترین حملات علیه برنامه‌های Vue هستند.  
✅ **نباید از `v-html` برای نمایش ورودی کاربر استفاده کرد.**  
✅ **اطلاعات حساس نباید در `localStorage` یا `public code` قرار بگیرند.**  
✅ **اعتبارسنجی داده‌های ورودی و استفاده از توکن‌های CSRF برای درخواست‌های مهم الزامی است.**  

