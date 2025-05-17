## شرح استغلال **SQL Injection** باستخدام DVWA

DVWA (Damn Vulnerable Web Application) هي بيئة اختبار آمنة تهدف لتعليم واستعراض ثغرات الـ SQL Injection.

### متطلبات التشغيل:
- نظام التشغيل: Windows (7 / 8 / 10 / 11)
- برنامج **XAMPP** يحتوي على:
  - Apache
  - PHP
  - MySQL
- DVWA: يوضع في المسار التالي:
  `C:\xampp\htdocs\dvwa`
- متصفح: Firefox أو Chrome

---

### خطوات التثبيت:

1. **تنزيل XAMPP**  
   من الرابط:  
   [https://www.apachefriends.org](https://www.apachefriends.org)  
   يحتوي على MySQL وPHP وApache

2. **تنزيل DVWA**  
   افتح CMD أو PowerShell ثم اكتب:
   ```bash
   git clone https://github.com/digininja/DVWA.git C:\xampp\htdocs\dvwa
   ```
   أو قم بتحميله كملف ZIP وفك الضغط إلى نفس المسار.

3. **تعديل إعدادات PHP**  
   (يجب تعديل بعض الإعدادات لتعمل DVWA بشكل صحيح، مثل تفعيل `allow_url_include` أو تعطيل بعض قيود الأمان حسب الحاجة)

---

### أدوات مساعدة:

- محرك البحث Edge أو Chrome
- أدوات اختبار الثغرات مثل:
  - **SQLmap**
  - **Burp Suite**
  - **Postman**
  - وغيرها من أدوات تحليل الحزم وحقن الأوامر
