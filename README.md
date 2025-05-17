# SQL-Injection-DVWA
# دليل عملي لاستغلال ثغرة SQL Injection في DVWA على ويندوز

مرحبًا! هذا المستودع يشرح خطوة بخطوة كيفية استغلال ثغرة **SQL Injection** في تطبيق **DVWA** (Damn Vulnerable Web Application) على نظام **Windows**.

---

## 📋 محتويات المستودع

1. [المتطلبات الأساسية](#المتطلبات-الأولية)  
2. [التثبيت والإعداد على ويندوز](#التثبيت-والإعداد-على-ويندوز)  
3. [تجربة ثغرة SQL Injection](#تجربة-ثغرة-sql-injection)  
4. [أمثلة على الـ Payloads](#أمثلة-على-الـ-payloads)  
5. [المراجع](#المراجع)  
6. [الرخصة](#الرخصة)  

---

## المتطلبات الأولية

- **نظام تشغيل** Windows (7/8/10/11)  
- **XAMPP** (Apache + PHP + MySQL) مُثبت ومُشغّل  
- نسخة **DVWA** موجودة في مجلد `C:\xampp\htdocs\dvwa`  
- متصفح ويب (Chrome أو Firefox)  

---

## التثبيت والإعداد على ويندوز

1. **تحميل XAMPP**  
   - نزّل من: https://www.apachefriends.org  
   - ثبّت واختر Apache و MySQL و PHP.

2. **تحميل DVWA**  
   - افتح PowerShell أو موجه الأوامر وانفّذ:
     ```bash
     git clone https://github.com/digininja/DVWA.git C:\xampp\htdocs\dvwa
     ```
   - أو فكّ الضغط عن ملف ZIP داخل `C:\xampp\htdocs\dvwa`.

3. **ضبط إعدادات PHP**  
   - افتح `C:\xampp\php\php.ini` وابحث عن الأسطر التالية وتأكد منها:
     ```ini
     magic_quotes_gpc = Off
     allow_url_include = On
     ```
   - أعد تشغيل خدمة Apache من لوحة تحكم XAMPP.

4. **تهيئة قاعدة البيانات**  
   - شغل MySQL من لوحة XAMPP.  
   - افتح المتصفح واذهب إلى:  
     ```
     http://localhost/phpmyadmin
     ```  
   - أنشئ قاعدة بيانات جديدة باسم:  
     ```
     dvwa
     ```
   - في مجلد `C:\xampp\htdocs\dvwa\config` انسخ `config.inc.php.dist` وأعد تسميته إلى `config.inc.php` ثم عدّل:
     ```php
     $_DVWA['db_user']     = 'root';
     $_DVWA['db_password'] = '';
     ```

5. **إعداد DVWA**  
   - في المتصفح افتح:
     ```
     http://localhost/dvwa/setup.php
     ```
   - اضغط على **Create / Reset Database**  
   - سجّل الدخول:
     - اسم المستخدم: `admin`
     - كلمة المرور: `password`

---

## تجربة ثغرة SQL Injection

1. من القائمة الجانبية اختر: **SQL Injection**  
2. تأكد من أن مستوى الأمان (Security) على **Low**  
3. في حقل **User ID** جرّب الإدخال التالي:
   ```sql
   1' OR '1'='1
   ```
4. اضغط **Submit**  
   سترى أن جميع سجلات المستخدمين عُرضت لأن الشرط أصبح دائمًا صحيحًا.

---

## أمثلة على الـ Payloads

| الهدف                          | الـ Payload                                          |
| ------------------------------ | ---------------------------------------------------- |
| عرض كل المستخدمين              | `1' OR '1'='1`                                       |
| تعليق باقي الاستعلام          | `1'; --`                                             |
| جلب اسم القاعدة وإصدار السيرفر | `0' UNION SELECT null, database(), version() --`     |

---

## المراجع

- مستودع DVWA الرسمي: https://github.com/digininja/DVWA  
- دليل PHP عن حقن SQL: https://www.php.net/manual/en/security.sql-injection.php  

---

## الرخصة

هذا المستند مرخص بموجب **MIT License**.  
© 2025 Bashar Naanah
