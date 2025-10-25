# 📚 بوابة شاملة احترافية - Professional Multi-Purpose Portal

## 🎯 نظرة عامة على المشروع

تم تطوير بوابة ويب احترافية شاملة (Full-Stack) تجمع بين:
- **التجارة الإلكترونية:** دبلومات مهنية، كتب إلكترونية، أعمال فنية تشكيلية
- **منصة محتوى:** مقالات، صحة وجمال، تسويق إلكتروني
- **الأخبار والطقس:** أخبار دولية وعربية، حالة الطقس العالمية
- **التواصل:** نموذج تواصل، روابط وسائل التواصل الاجتماعي

---

## 🏗️ البنية التحتية للمشروع

### **التقنيات المستخدمة:**

| الطبقة | التقنية | الإصدار |
|:---|:---|:---|
| **Frontend** | React 19 + Tailwind CSS 4 | Latest |
| **Backend** | Express 4 + tRPC 11 | Latest |
| **Database** | MySQL/TiDB + Drizzle ORM | Latest |
| **Authentication** | Manus OAuth | Integrated |
| **Payment** | Stripe | Ready |
| **Storage** | S3 | Integrated |
| **APIs** | NewsAPI, Open-Meteo | Integrated |

---

## 📁 هيكل المشروع

```
professional_portal/
├── client/                          # الواجهة الأمامية (Frontend)
│   ├── src/
│   │   ├── pages/                   # صفحات التطبيق
│   │   │   ├── Home.tsx             # الصفحة الرئيسية
│   │   │   ├── Products.tsx         # صفحة المنتجات
│   │   │   ├── Cart.tsx             # سلة المشتريات
│   │   │   ├── Checkout.tsx         # صفحة الدفع
│   │   │   ├── NewsWeather.tsx      # الأخبار والطقس
│   │   │   ├── DigitalMarketing.tsx # التسويق الإلكتروني
│   │   │   ├── HealthBeauty.tsx     # الصحة والجمال
│   │   │   ├── Blog.tsx             # المدونة
│   │   │   └── Contact.tsx          # اتصل بنا
│   │   ├── components/              # مكونات قابلة لإعادة الاستخدام
│   │   ├── contexts/                # React Contexts
│   │   ├── hooks/                   # Custom Hooks
│   │   ├── lib/                     # Utilities
│   │   ├── App.tsx                  # التوجيه الرئيسي
│   │   └── main.tsx                 # نقطة الدخول
│   └── public/                      # الملفات الثابتة
│
├── server/                          # الخادم الخلفي (Backend)
│   ├── routers.ts                   # واجهات tRPC API
│   ├── db.ts                        # دوال قاعدة البيانات
│   ├── stripe.ts                    # خدمة الدفع
│   ├── news-weather.ts              # خدمة الأخبار والطقس
│   ├── storage.ts                   # خدمة التخزين
│   └── _core/                       # الملفات الأساسية
│       ├── index.ts                 # نقطة دخول الخادم
│       ├── context.ts               # سياق tRPC
│       ├── oauth.ts                 # المصادقة
│       ├── llm.ts                   # تكامل LLM
│       └── ...
│
├── drizzle/                         # قاعدة البيانات
│   ├── schema.ts                    # تعريف الجداول
│   └── relations.ts                 # العلاقات بين الجداول
│
├── shared/                          # الملفات المشتركة
│   ├── const.ts                     # الثوابت
│   └── types.ts                     # الأنواع المشتركة
│
└── package.json                     # المتطلبات والإعدادات
```

---

## 🗄️ قاعدة البيانات

### **الجداول الرئيسية:**

#### 1. **جدول المستخدمين (users)**
```sql
- id (Primary Key)
- openId (Unique - من Manus OAuth)
- name
- email
- loginMethod
- role (admin | user)
- createdAt
- updatedAt
- lastSignedIn
```

#### 2. **جدول الفئات (categories)**
```sql
- id (Primary Key)
- name
- description
- icon
- createdAt
```

#### 3. **جدول المنتجات (products)**
```sql
- id (Primary Key)
- categoryId (Foreign Key)
- name
- description
- price (بالدولار)
- image
- stock
- isActive
- createdAt
- updatedAt
```

#### 4. **جدول سلة المشتريات (cartItems)**
```sql
- id (Primary Key)
- userId (Foreign Key)
- productId (Foreign Key)
- quantity
- addedAt
```

#### 5. **جدول الطلبات (orders)**
```sql
- id (Primary Key)
- userId (Foreign Key)
- totalAmount
- status (pending | completed | cancelled)
- customerName
- customerEmail
- customerPhone
- shippingAddress
- createdAt
- updatedAt
```

#### 6. **جدول تفاصيل الطلبات (orderItems)**
```sql
- id (Primary Key)
- orderId (Foreign Key)
- productId (Foreign Key)
- quantity
- price
```

---

## 🔌 واجهات API (tRPC)

### **1. واجهات المنتجات**
```typescript
// الحصول على جميع الفئات
trpc.products.getCategories.useQuery()

// الحصول على جميع المنتجات
trpc.products.getAll.useQuery()

// الحصول على منتجات فئة معينة
trpc.products.getByCategory.useQuery({ categoryId })

// الحصول على تفاصيل منتج واحد
trpc.products.getById.useQuery({ id })
```

### **2. واجهات السلة**
```typescript
// الحصول على سلة المستخدم
trpc.cart.getCart.useQuery()

// إضافة منتج للسلة
trpc.cart.addItem.useMutation({ productId, quantity })

// حذف منتج من السلة
trpc.cart.removeItem.useMutation({ cartItemId })
```

### **3. واجهات الطلبات**
```typescript
// الحصول على طلبات المستخدم
trpc.orders.getMyOrders.useQuery()

// إنشاء طلب جديد
trpc.orders.createOrder.useMutation({ items, customerInfo })
```

### **4. واجهات الأخبار**
```typescript
// الأخبار الدولية
trpc.news.getInternational.useQuery({ limit })

// الأخبار العربية
trpc.news.getArabic.useQuery({ limit })
```

### **5. واجهات الطقس**
```typescript
// الحصول على حالة الطقس
trpc.weather.getWeather.useQuery({ latitude, longitude })
```

---

## 🎨 الصفحات الرئيسية

### **1. الصفحة الرئيسية (Home)**
- عرض شامل لجميع الأقسام
- 6 بطاقات رئيسية مع رموز ملونة
- شريط تنقل محسّن
- أزرار للعمل (تصفح، تسجيل دخول)

### **2. صفحة المنتجات (Products)**
- عرض جميع المنتجات
- تصفية حسب الفئة
- عرض الأسعار بالدولار
- إضافة للسلة

### **3. سلة المشتريات (Cart)**
- عرض المنتجات المختارة
- تعديل الكمية
- حساب الإجمالي
- الانتقال للدفع

### **4. صفحة الدفع (Checkout)**
- ملخص الطلب
- بيانات الفاتورة
- نظام الدفع الآمن (Stripe)

### **5. الأخبار والطقس (NewsWeather)**
- عرض حالة الطقس الحالية
- أخبار دولية وعربية
- كشف الموقع التلقائي

### **6. التسويق الإلكتروني (DigitalMarketing)**
- 6 مقالات عن التسويق الرقمي
- 6 أدوات موصى بها
- تصميم احترافي

### **7. الصحة والجمال (HealthBeauty)**
- 6 نصائح صحية
- مقالات مميزة
- موارد قابلة للتحميل

### **8. المدونة (Blog)**
- أرشيف شامل للمقالات
- نظام بحث متقدم
- تصفية حسب الفئات
- معلومات المؤلف والتاريخ

### **9. اتصل بنا (Contact)**
- نموذج تواصل كامل
- معلومات التواصل
- روابط وسائل التواصل الاجتماعي:
  - Facebook
  - Twitter
  - Instagram
  - LinkedIn

---

## 🔐 نظام المصادقة

**نوع المصادقة:** Manus OAuth

**المميزات:**
- تسجيل دخول آمن
- إدارة الجلسات (Sessions)
- حماية المسارات المحمية
- دعم الأدوار (Admin/User)

---

## 💳 نظام الدفع

**المنصة:** Stripe

**الميزات:**
- الدفع بالدولار الأمريكي
- معاملات آمنة
- دعم بطاقات الائتمان
- تأكيد الدفع الفوري

**الملفات:**
- `server/stripe.ts` - خدمة Stripe
- `client/src/pages/Checkout.tsx` - واجهة الدفع

---

## 📰 الأخبار والطقس

**المصادر:**
- **الأخبار:** NewsAPI
- **الطقس:** Open-Meteo API

**الملفات:**
- `server/news-weather.ts` - خدمة الأخبار والطقس
- `client/src/pages/NewsWeather.tsx` - واجهة العرض

---

## 🎨 التصميم والأسلوب

**نظام التصميم:**
- Tailwind CSS 4
- shadcn/ui Components
- Responsive Design
- Dark/Light Theme Support

**الألوان:**
- الأزرق: الرئيسي
- الأخضر: الكتب
- الأرجواني: الفن
- البرتقالي: التسويق
- الوردي: الصحة والجمال
- الأرجواني: المدونة

---

## 🚀 كيفية البدء

### **1. التثبيت**
```bash
cd professional_portal
pnpm install
```

### **2. إعداد المتغيرات البيئية**
```bash
# أضف في Settings → Secrets:
STRIPE_SECRET_KEY=your_key
VITE_STRIPE_PUBLISHABLE_KEY=your_key
```

### **3. تشغيل الخادم**
```bash
pnpm dev
```

### **4. الوصول للموقع**
```
http://localhost:3000
```

---

## 📊 استهلاك الرصيد

| المرحلة | الوصف | الرصيد التقريبي |
|:---|:---|:---|
| 1-2 | التهيئة والتجارة الإلكترونية | ~400 |
| 3-4 | الدفع والأخبار | ~350 |
| 5-6 | المحتوى والتصميم | ~400 |
| **المجموع** | | **~1,150** |

---

## 🔄 المتطلبات المستقبلية

- [ ] إضافة نظام التقييمات والتعليقات
- [ ] نظام الإشعارات البريدية
- [ ] لوحة تحكم الإدارة (Admin Dashboard)
- [ ] نظام الخصومات والعروض
- [ ] تحسين محركات البحث (SEO)
- [ ] تحليلات متقدمة
- [ ] نظام التوصيات الذكية

---

## 📞 الدعم والمساعدة

**للمزيد من المعلومات:**
- اطلع على ملفات الكود المرفقة
- راجع التعليقات في الملفات
- تحقق من README.md في المشروع

---

**تم إنشاء هذا المشروع بواسطة Manus AI**
**آخر تحديث: 2025-10-23**

