# 🎨 دليل التخصيص والتطوير

هذا الملف يحتوي على أفكار لتخصيص وتطوير نظام مشاعر الطلاب.

## 🎨 تخصيص الألوان

### تغيير الألوان الرئيسية

في كلا الملفين (`index.html` و `dashboard.html`)، ابحث عن قسم `:root` في CSS وغيّر الألوان:

```css
:root {
    --primary-bg: #e0f7fa;           /* لون الخلفية الرئيسي */
    --container-bg: #ffffff;          /* لون الحاويات */
    --primary-color: #00796b;         /* اللون الأساسي */
    --secondary-color: #004d40;       /* اللون الثانوي */
    --accent-color: #ffab40;          /* لون التمييز */
}
```

### مثال: ألوان زرقاء
```css
:root {
    --primary-bg: #e3f2fd;
    --primary-color: #1976d2;
    --secondary-color: #0d47a1;
    --accent-color: #ff9800;
}
```

### مثال: ألوان خضراء
```css
:root {
    --primary-bg: #e8f5e9;
    --primary-color: #388e3c;
    --secondary-color: #1b5e20;
    --accent-color: #ffc107;
}
```

## ➕ إضافة مشاعر جديدة

في ملف `index.html`، أضف في قسم `feelings-grid`:

```html
<div>
    <input type="radio" id="NEW_FEELING_ID" name="feeling" value="اسم_الشعور">
    <label for="NEW_FEELING_ID" class="feeling-option">
        <span class="emoji">😊</span>
        <span class="feeling-text">اسم الشعور</span>
    </label>
</div>
```

ثم في `dashboard.html`، أضف في قائمة `feelingsMap`:

```javascript
'اسم_الشعور': { emoji: '😊', category: 'positive' }
```

## 🏫 تخصيص الصفوف والشعب

### إضافة صفوف جديدة

في كلا الملفين، ابحث عن قائمة الصفوف وأضف:

```html
<option value="السابع">السابع</option>
<option value="الثامن">الثامن</option>
```

### إضافة شعب جديدة

```html
<option value="4">4</option>
<option value="5">5</option>
```

## 🔔 إضافة تنبيهات للمشاعر السلبية

في ملف `dashboard.html`، أضف هذا الكود داخل function `displayFeelings`:

```javascript
// تنبيه للمشاعر السلبية
if (feelingData.category === 'negative') {
    // إرسال إشعار للمعلم
    console.log(`تنبيه: الطالب ${feeling.studentName} يشعر بـ ${feeling.feeling}`);
    
    // يمكنك هنا إضافة كود لإرسال بريد إلكتروني أو رسالة
}
```

## 📊 إضافة رسوم بيانية

استخدم مكتبة Chart.js لإضافة رسوم بيانية:

### 1. أضف في `<head>` من `dashboard.html`:
```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
```

### 2. أضف canvas للرسم البياني:
```html
<canvas id="feelingsChart" width="400" height="200"></canvas>
```

### 3. أضف الكود JavaScript:
```javascript
function createChart(feelings) {
    const ctx = document.getElementById('feelingsChart').getContext('2d');
    
    // حساب عدد كل شعور
    const feelingCounts = {};
    feelings.forEach(f => {
        feelingCounts[f.feeling] = (feelingCounts[f.feeling] || 0) + 1;
    });
    
    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: Object.keys(feelingCounts),
            datasets: [{
                label: 'عدد الطلاب',
                data: Object.values(feelingCounts),
                backgroundColor: 'rgba(0, 121, 107, 0.6)'
            }]
        }
    });
}
```

## 🔐 إضافة نظام تسجيل دخول كامل

استخدم Firebase Authentication:

### 1. في Firebase Console:
- اذهب إلى Authentication
- فعّل Email/Password

### 2. أضف في `dashboard.html`:
```html
<script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-auth-compat.js"></script>
```

### 3. أضف كود تسجيل الدخول:
```javascript
const auth = firebase.auth();

function login() {
    const email = prompt("البريد الإلكتروني:");
    const password = prompt("كلمة المرور:");
    
    auth.signInWithEmailAndPassword(email, password)
        .then(() => {
            console.log("تم تسجيل الدخول بنجاح");
        })
        .catch((error) => {
            alert("خطأ في تسجيل الدخول");
        });
}

// التحقق من تسجيل الدخول
auth.onAuthStateChanged((user) => {
    if (!user) {
        login();
    }
});
```

## 📧 إرسال تقارير بالبريد الإلكتروني

استخدم EmailJS أو خدمة مشابهة:

```javascript
function sendEmailReport() {
    // استخدم EmailJS أو Firebase Functions لإرسال البريد
    emailjs.send("service_id", "template_id", {
        to_email: "principal@school.com",
        message: "تقرير مشاعر الطلاب اليومي"
    });
}
```

## 🌙 وضع الليل (Dark Mode)

أضف زر لتبديل الوضع:

```html
<button onclick="toggleDarkMode()">🌙 الوضع الليلي</button>
```

```javascript
function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
}
```

```css
body.dark-mode {
    --primary-bg: #1a1a1a;
    --container-bg: #2d2d2d;
    --text-dark: #ffffff;
}
```

## 📱 تحويل إلى تطبيق جوال

استخدم PWA (Progressive Web App):

### أضف في `<head>`:
```html
<link rel="manifest" href="manifest.json">
<meta name="theme-color" content="#00796b">
```

### أنشئ ملف `manifest.json`:
```json
{
  "name": "نظام مشاعر الطلاب",
  "short_name": "مشاعر الطلاب",
  "start_url": "index.html",
  "display": "standalone",
  "background_color": "#e0f7fa",
  "theme_color": "#00796b",
  "icons": [
    {
      "src": "icon-192.png",
      "sizes": "192x192",
      "type": "image/png"
    }
  ]
}
```

## 🔊 إضافة أصوات

```javascript
function playSound() {
    const audio = new Audio('success.mp3');
    audio.play();
}

// استخدمه بعد إرسال النموذج
form.addEventListener('submit', async function(e) {
    // ... الكود الموجود
    playSound(); // إضافة الصوت
});
```

## 📊 تقارير متقدمة

### تقرير شهري:
```javascript
function generateMonthlyReport() {
    const thisMonth = new Date().getMonth();
    const monthlyFeelings = allFeelings.filter(f => {
        const feelingDate = f.timestamp?.toDate();
        return feelingDate && feelingDate.getMonth() === thisMonth;
    });
    
    console.log(`تقرير الشهر: ${monthlyFeelings.length} مشاعر`);
}
```

### تقرير حسب الصف:
```javascript
function generateClassReport(className) {
    const classFeelings = allFeelings.filter(f => f.classLevel === className);
    // إنشاء تقرير مفصل
}
```

## 💾 نسخ احتياطي تلقائي

```javascript
function autoBackup() {
    setInterval(() => {
        exportToCSV();
        console.log("تم إنشاء نسخة احتياطية");
    }, 86400000); // كل 24 ساعة
}
```

## 🎯 أفكار إضافية

1. **نظام النقاط**: امنح الطلاب نقاطاً عند مشاركة مشاعرهم الإيجابية
2. **تذكيرات يومية**: أرسل تذكيراً للطلاب لتسجيل مشاعرهم
3. **مقارنات زمنية**: قارن مشاعر الطلاب عبر الأسابيع والأشهر
4. **تكامل مع Google Classroom**: ربط مع Google Classroom
5. **إحصائيات متقدمة**: متوسطات، نسب مئوية، اتجاهات
6. **ردود تلقائية**: رسائل دعم تلقائية للمشاعر السلبية
7. **لوحة معلومات للأهل**: صفحة خاصة لأولياء الأمور
8. **تقييم المعلمين**: إضافة قسم لتقييم المعلمين

---

💡 **نصيحة**: ابدأ بالتخصيصات البسيطة أولاً، ثم انتقل تدريجياً للميزات المتقدمة!
