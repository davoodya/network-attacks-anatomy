---
Episode: Writing Archives - Questions & Prompts
Date: 2026-05-12T18:28:00
Title: Network Attacks - General Models
tags:
  - Networking
  - network_security
  - network_attacks
  - Network_Pentest
Categories:
  - Networking
  - Network Pentesting
  - Network Security
Is Finished?:
Practice Status:
Spend Time:
Tools:
banner:
banner_icon:
Home:
Previous Episode:
Next Episode:
---
-------
## فصل Burp Suite - - سرفصل ها - خلاصه
**فصل X: Burp Suite - استاندارد طلایی تست نفوذ وب**

1. **Burp Suite: Introduction & History**
    
    - معرفی ابزار، تاریخچه و نقش آن در تست نفوذ مدرن.
        
2. **Burp Suite: Editions and Installation Guide**
    
    - مقایسه نسخه‌های Community و Professional، راهنمای نصب گام‌به‌گام بر روی سیستم‌عامل‌های مختلف.
        
3. **Burp Suite: User Interface and Basic Configuration**
    
    - آشنایی با اجزای اصلی محیط کار، تنظیمات پروکسی و مرورگر.
        
4. **Burp Suite: Core Features & Dashboard**
    
    - بررسی داشبورد، Event Log و تنظیمات کلی ابزار.
        
5. **Burp Suite: Target Scope and Site map**
    
    - تعریف Scope، ساخت و تحلیل نقشه سایت برای درک ساختار برنامه هدف.
        
6. **Burp Suite: Most Important Modules**
    
    - معرفی و توضیح کامل ماژول‌های اصلی: Proxy, Repeater, Intruder, Decoder, Comparer, Sequencer.
        
7. **Burp Suite: Reconnaissance & Scanning – Part 1**
    
    - اسکن غیرفعال (Passive Scanning) و تحلیل خودکار آسیب‌پذیری‌ها.
        
8. **Burp Suite: Reconnaissance & Scanning – Part 2**
    
    - اسکن فعال (Active Scanning)، Crawling و کشف محتوای پنهان.
        
9. **Burp Suite: Attack & Exploitation Modules**
    
    - استفاده از Repeater برای تست دستی و Intruder برای حملات خودکار مانند Fuzzing و Brute-Forcing.
        
10. **Burp Suite: Advanced Interception Techniques**
    
    - تسلط بر قوانین رهگیری (Interception Rules)، فیلترها و دستکاری درخواست‌ها و پاسخ‌ها.
        
11. **Burp Suite: Session Handling and Macros**
    
    - خودکارسازی مدیریت نشست‌ها و قوانین پیچیده احراز هویت با استفاده از Macros.
        
12. **Burp Suite: BApp Store (Extensions)**
    
    - نصب و استفاده از افزونه‌های کاربردی برای افزایش قدرت ابزار (مانند Active Scan++, Autorize, Turbo Intruder).
        
13. **Burp Suite: Custom Extension Development**
    
    - مقدمه‌ای بر نوشتن افزونه‌های سفارشی با استفاده از APIهای Java، Python یا Ruby.
        
14. **Burp Suite: Configuration Library & Project Files**
    
    - مدیریت پیکربندی‌های مختلف و ذخیره‌سازی پروژه‌ها.
        
15. **Burp Suite: Implement Common Web Attacks**
    
    - پیاده‌سازی عملی حملات کلاسیک وب (SQLi، XSS، CSRF، IDOR و غیره) با استفاده از ماژول‌های Burp.
        
16. **mitmproxy vs Burp Suite – Complementary Tools**
    
    - مقایسه و بررسی چگونگی استفاده ترکیبی از این دو ابزار قدرتمند.
        
17. **Burp Suite: Conclusion**
    
    - جمع‌بندی نهایی و تأکید بر اصول اخلاقی.
## فصل Burp Suite - سرفصل ها - پارت 1

### بخش ۰: مبانی و آشنایی اولیه (Burp Suite: Foundations)

#### ۰.۱. Burp Suite: Introduction & History
- تاریخچه توسعه Burp Suite توسط **PortSwigger** و جایگاه آن به عنوان استاندارد صنعتی تست نفوذ وب
- فلسفه طراحی: پروکسی جامع برای دستکاری و تحلیل ترافیک HTTP/HTTPS
- تکامل ابزار از نسخه ۱.۰ تا نسخه ۲۰۲۶

#### ۰.۲. Burp Suite: Editions and Installation Guide
- مقایسه جامع نسخه‌ها: **Community vs Professional vs Enterprise**
    - محدودیت‌های Intruder در نسخه Community
    - ویژگی‌های انحصاری Professional: Scanner، Collaborator، REST API، و Burp AI Extension
- نصب و راه‌اندازی در سیستم‌های عامل مختلف (Windows, Linux, macOS)
- آشنایی با پیش‌نیازها: Java Runtime Environment و تنظیمات حافظه (Heap size)

#### ۰.۳. Burp Suite: User Interface and Basic Configuration
- ساختار کلی رابط کاربری (Dashboard, Tabs, Message Editor)
- تنظیمات اولیه پروکسی (Proxy Listener و پورت ۸۰۸۰)
- آشنایی با **Burp's Browser** (مرورگر داخلی با پروکسی از پیش تنظیم شده)
- نصب و اعتماد به **CA Certificate** Burp برای رمزگشایی ترافیک HTTPS

---

### بخش ۱: شروع کار و آشنایی با هسته ابزار (Getting Started & Core Features)

#### ۱.۱. شروع کار با Burp Suite
- راه‌اندازی اولین جلسه تست
- پیکربندی مرورگر خارجی برای استفاده از پروکسی Burp 
- مفاهیم پایه: Intercept (روشن/خاموش)، Forward، Drop

#### ۱.۲. Burp Suite: Core Features & Dashboard
- **Dashboard:** نمای کلی از فعالیت‌ها، رویدادها و هشدارها
- **Live Tasks:** مدیریت اسکن خودکار در حین مرور
- **Issue Activity:** تاریخچه و وضعیت آسیب‌پذیری‌های کشف شده

#### ۱.۳. Burp Suite: Target Scope and Site Map
- **Target Scope:** تعیین محدوده تست و اهمیت آن در کاهش نویز 
    - Normal Scope Control – تعریف URL prefixes
    - Advanced Scope Control – استفاده از Regex برای Host, Port, Path
- **Site Map:** درخت توپولوژی برنامه و محتوای کشف شده
- فیلتر کردن بر اساس in-scope items 
- وارد کردن/خروجی گرفتن از Site Map

---

### بخش ۲: روش‌های کانفیگ و شخصی‌سازی Burp Suite

- **Project Options:** تنظیمات مربوط به پروژه جاری (Connections, HTTP, SSL, Sessions, Misc)
- **User Options:** تنظیمات سراسری (Display, Hotkeys, Editor, Logger)
- **Configuration Files:** ذخیره و بارگذاری فایل‌های کانفیگ (JSON/XML)
- **Tabs Refactoring:** مرتب‌سازی و شخصی‌سازی زبانه‌های ابزار
- **Look and Feel:** تغییر تم بصری (Light/Dark)

---

### بخش ۳: توضیح تمام قسمت‌ها، منوها، گزینه‌ها و ماژول‌های موجود

**هدف:** پوشش دادن هر چه هست و نیست در Burp Suite – تمام قسمت‌های مهم، زیرابزارها، کاربرد هر یک، و نحوه عملکرد.

#### ۳.۱. Proxy
- **Intercept:** رهگیری و تغییر لحظه‌ای درخواست‌ها 
- **HTTP History & WebSockets History:** تاریخچه ترافیک
- **Options:** تنظیمات پیشرفته پروکسی
- **Match and Replace:** قوانین جایگزینی خودکار در هدرها/بدنه
- **SSL Pass Through:** استثنا کردن هاست‌های خاص از رمزگشایی

#### ۳.۲. Target
- **Site Map:** مدیریت نقشه سایت و محتوا
- **Scope:** تعیین محدوده با قوانین ساده و پیشرفته 

#### ۳.۳. Intruder
- **Attack Types:** Sniper, Battering ram, Pitchfork, Cluster bomb
- **Positions:** تعیین نقاط تزریق
- **Payloads Settings:** انواع Payload (Simple list, Runtime file, Numbers, Dates, Brute forcer, Null payloads, Character frobber, Bit flipper, Username generator, Grep-Extract)
- **Attack Results:** تحلیل نتایج و شناسایی پاسخ‌های غیرعادی
- **محدودیت‌های نسخه Community**

#### ۳.۴. Repeater
- ارسال مجدد و ویرایش دستی درخواست‌ها 
- **Comparison:** ابزار مقایسه بصری پاسخ‌ها
- **Render:** نمایش پاسخ HTML در مرورگر داخلی
- نام‌گذاری تب‌ها برای پیگیری تست‌های مختلف

#### ۳.۵. Scanner (نسخه Professional)
- **Active vs Passive Scanning**
- **Scan Queue و Scan Results**
- **Authenticated Scanning**
- **Live Scanning:** اسکن خودکار در حین مرور

#### ۳.۶. Collaborator (نسخه Professional)
- تشخیص آسیب‌پذیری‌های خارج از باند (Out-of-Band)
- **Blind SSRF, Blind XSS, Blind SQLi**

#### ۳.۷. ابزارهای کمکی
- **Decoder:** رمزگشایی/رمزگذاری (Base64, URL, HTML, ASCII Hex, Hex, Octal, Binary, Gzip)
- **Comparer:** مقایسه بصری دو داده
- **Sequencer:** تحلیل کیفیت تصادفی بودن توکن‌ها
- **Logger:** ثبت و جستجوی پیشرفته ترافیک
- **Organizer:** جمع‌آوری و annotation درخواست‌ها

#### ۳.۸. Extender
- مدیریت افزونه‌های نصب شده
- بارگذاری افزونه‌های Java/Python (Jython)/Ruby (JRuby)

---

### بخش ۴: پیاده‌سازی انواع اسکن‌های مختلف

- **پیکربندی اسکنر:** تنظیمات Scan Complexity (Fast/Normal/Deep)، انتخاب issue types
- **اسکن دستی:** ارسال درخواست به Scanner به صورت موردی
- **اسکن خودکار:** استفاده از Live Tasks
- **اسکن احراز هویت شده (Authenticated Scanning):** ترکیب با Session Handling Rules
- **اسکن APIها (REST & GraphQL)**
- **اسکن Gray-box / Black-box**
- بهینه‌سازی اسکن برای کارایی

---

### بخش ۵: پیاده‌سازی انواع Attack & Exploitation

- **Authentication Attacks:** فریب احراز هویت، بازنشانی نشست
- **Authorization Attacks:** IDOR، Privilege Escalation، Directory Traversal
- **Input Validation Attacks:** SQLi، XSS، Command Injection، XXE، SSRF
- **Business Logic Attacks:** نقض جریان کاری، سوءاستفاده از محدودیت نرخ
- **Client-Side Attacks:** Clickjacking، DOM-based XSS، CORS misconfiguration
- **File Upload Attacks:** دور زدن پسوند، polyglot files

---

### بخش ۶: پیاده‌سازی اصولی تست نفوذ وب و Bug Hunting

#### ۶.۱. متدولوژی تست نفوذ با Burp Suite
- **OWASP Testing Guide v4/v5** پیاده‌سازی گام‌به‌گام
- نقشه راه Bug Hunter: از شناسایی سطح حمله تا بهره‌برداری

#### ۶.۲. Implement Common Web Attacks

##### SQL Injection
- شناسایی نقاط تزریق با Intruder
- استخراج داده با UNION و Error-based payloads 
- استفاده از Collaborator برای Blind SQLi

##### Cross-Site Scripting (XSS)
- تست Reflected و Stored XSS با Repeater
- DOM Invader: کشف خودکار DOM-based XSS
- تزریق payloadهای سفارشی

##### CSRF و SSRF
- شناسایی و بهره‌برداری از CSRF
- استفاده از Collaborator برای SSRF

##### XXE (XML External Entity)
- دستکاری XML payloadها با Repeater
- خواندن فایل‌های داخلی سرور

##### JWT Attacks
- تست نقص‌های پیاده‌سازی JWT (alg: none، brute-force کلید)

#### ۶.۳. تکنیک‌های پیشرفته Bug Hunting
- **Param Miner:** کشف پارامترهای پنهان و سرآیندهای سفارشی
- **Autorize:** تست سطح دسترسی و IDOR
- **Turbo Intruder:** حملات موازی با سرعت بالا

---

### بخش ۷: تکنیک‌های پیشرفته رهگیری (Advanced Interception Techniques)

- **WebSocket Interception:** رهگیری و دستکاری پیام‌های WebSocket
- **HTTP/2 Interception:** تحلیل درخواست‌های باینری HTTP/2
- **Custom Intercept Rules:** نوشتن قوانین پیچیده برای رهگیری بر اساس محتوا
- **Response Modification:** تغییر پویا پاسخ‌های سرور قبل از رسیدن به مرورگر
- **TLS 1.3 Workaround:** استفاده از **mitmproxy** به عنوان لایه میانی برای رمزگشایی ترافیک TLS 1.3 

---

### بخش ۸: مدیریت نشست و Macros

#### ۸.۱. Session Handling Rules
- مفهوم مدیریت نشست خودکار
- انواع قوانین: Run a macro، Validate session، Enforce session، Prompt for credentials
- تنظیم Scope برای تعیین محل اعمال قاعده

#### ۸.۲. Macros
- **Macro چیست؟** دنباله خودکار درخواست‌های HTTP برای شبیه‌سازی تعاملات 
- **کاربردها:**
    - لاگین خودکار برای اسکن نواحی ممیزی شده 
    - استخراج خودکار CSRF/JWT tokens‌های پویا
    - بازسازی نشست پس از انقضا
    - انجام workflowهای چند مرحله‌ای (ورود → خرید محصول → خروج)
- **روش ایجاد Macro:**
    - انتخاب درخواست‌ها از Proxy History
    - تنظیم **Custom parameter locations** برای استخراج tokens از پاسخ‌ها 
    - تنظیم درخواست‌ها برای به‌روزرسانی خودکار هدرها و پارامترها
- **Cookie Jar:** مدیریت کوکی‌ها در طول اجرای Macro
- **تست Macro:** تأیید عملکرد صحیح قبل از استفاده در اتوماسیون
- **مقایسه قابلیت‌های Macro در نسخه‌های مختلف:** 

| قابلیت | Community | Professional |
|:---|:---:|:---:|
| Macro creation | ✅ | ✅ |
| Session Handling Rules | ❌ | ✅ |
| Intruder + Macro | ❌ | ✅ |
| Scanner + Macro | ❌ | ✅ |

---

### بخش ۹: mitmproxy vs Burp Suite – Complementary Tools

در این بخش به بررسی این موضوع می‌پردازیم که **mitmproxy** و **Burp Suite** هرگز رقیب یکدیگر نبوده‌اند، بلکه ابزارهایی **مکمل** هستند. 

| ویژگی | Burp Suite | mitmproxy |
|:---|:---|:---|
| رابط کاربری | GUI کامل | CLI/TUI (خط فرمان) |
| قابلیت اصلی | تست نفوذ وب حرفه‌ای | پروکسی سبک برای دیباگ و ضبط |
| پشتیبانی از افزونه | Montoya API, BApp Store | Python scripting (addons) |
| رمزگشایی TLS 1.3 | محدودیت (نیازمند JDK patch) | پشتیبانی کامل  |
| کاربرد ترکیبی | تحلیل نهایی و بهره‌برداری | لایه میانی برای downgrade TLS 1.3 به 1.2 |

**سناریوی ترکیبی (Meta-style):** 
```
فلو: دستگاه موبایل → mitmproxy(:8645) → Burp Proxy(:8646)
```
دستور راه‌اندازی mitmproxy:
```bash
mitmproxy --mode upstream:http://127.0.0.1:8646 --set tls_version_server_min=TLS1_2 --set tls_version_server_max=TLS1_2 --ssl-insecure -p 8645
```

این تنظیمات به محققان امنیتی اجازه می‌دهد تا ترافیک برنامه‌های موبایلی که از TLS 1.3 استفاده می‌کنند را با Burp Suite تحلیل کنند. 

---

### بخش ۱۰: خودکارسازی و Extension Development (سطح پیشرفته)

#### ۱۰.۱. BChecks – Scanner Rules سفارشی
- BCheck چیست؟ (زبان دامنه‌خاص برای قوانین اسکنر)
- ساختار فایل BCheck (metadata، وابستگی‌ها، قوانین تشخیص)
- نوشتن قانون سفارشی برای تشخیص آسیب‌پذیری خاص

#### ۱۰.۲. Bambdaها – شخصی‌سازی بدون کامپایل
- Bambda چیست؟ (Lambda + Burp – قطعه‌کد جاوا اجرا در لحظه)
- انواع: **Request Bambda**، **Response Bambda**، **WebSocket Bambda**
- سناریوهای کاربردی: فیلتر درخواست‌ها، افزودن هدرها، استخراج داده

#### ۱۰.۳. Montoya API (Extension Development)
- Montoya API در مقابل API قدیمی
- راه‌اندازی محیط توسعه (IntelliJ IDEA / Eclipse)
- پیاده‌سازی `BurpExtension` و متد `initialize()`
- افزودن Tab سفارشی و Context Menu
- **HttpHandler:** رهگیری و تغییر ترافیک
- **WebSocketHandler:** مدیریت WebSocket
- **Persistance:** ذخیره‌سازی داده بین جلسات
- **AI-Powered Extensions:** یکپارچه‌سازی با LLM برای تولید payload هوشمند

#### ۱۰.۴. REST API (نسخه Professional)
- فعال‌سازی API و احراز هویت
- نقاط پایانی اصلی: شروع توقف اسکن، دریافت نتایج، مدیریت پروژه

---

### بخش ۱۱: Burp Suite: Conclusion

#### جمع‌بندی نهایی و تأکید بر اصول اخلاقی

در این فصل، مجموعه ابزارهای **Burp Suite** را به عنوان یکی از قدرتمندترین و کامل‌ترین ابزارهای تست نفوذ وب بررسی کردیم. از مبانی پروکسی و رهگیری ترافیک گرفته تا تکنیک‌های پیشرفته مانند **Macros** برای خودکارسازی مدیریت نشست‌های پویا ، **Session Handling Rules** برای انجام workflowهای پیچیده ، و حتی **Extension Development** با Montoya API برای توسعه افزونه‌های سفارشی.

همان‌طور که در طول فصل مشاهده کردیم، قدرت اصلی Burp Suite در **اکوسیستم گسترده افزونه‌ها (BApp Store)** و **قابلیت شخصی‌سازی بی‌نظیر** آن نهفته است – از **BChecks** ساده و **Bambdaهای** بدون نیاز به کامپایل گرفته تا افزونه‌های کامل جاوا با دسترسی به تمام جنبه‌های ترافیک.

**نکته اخلاقی و قانونی:** Burp Suite ابزاری بسیار قدرتمند است که استفاده از آن مسئولیت‌های بزرگی را به همراه دارد. PortSwigger، توسعه‌دهنده این ابزار، همواره بر **آزمایش فقط در محیط‌های دارای مجوز و شبکه‌های شخصی** تأکید کرده است. کلیه محتوای این فصل صرفاً جنبه **آموزشی** داشته و برای استفاده در **محیط‌های آزمایشگاهی و تحت نظارت مربیان متخصص** طراحی شده است. هرگونه استفاده از Burp Suite برای تست نفوذ در شبکه‌های بدون اخذ مجوز کتبی، غیرقانونی بوده و شامل پیگرد قانونی است.

**مسیر پیش رو:** با تسلط بر Burp Suite، اکنون قادر خواهید بود:
- ترافیک HTTP/HTTPS/WebSocket را به صورت کامل رهگیری و دستکاری کنید
- اسکن‌های خودکار آسیب‌پذیری را پیکربندی و اجرا کنید
- حملات پیچیده وب (SQLi، XSS، SSRF، CSRF و ...) را با ابزارهای تخصصی Burp پیاده‌سازی کنید
- مدیریت نشست‌های پویا و CSRF tokens را با Macros به صورت خودکار انجام دهید
- افزونه‌های سفارشی برای نیازهای خاص بنویسید

در فصل‌های بعدی با سایر ابزارهای مکمل مانند **PETEP** برای پروتکل‌های غیر HTTP و **BeEF** برای بهره‌برداری از مرورگر آشنا خواهیم شد. 


### جدول نهایی – ساختار کامل فصل Burp Suite

| شماره | عنوان بخش | توضیح |
|:---:|:---|:---|
| ۰ | مبانی و آشنایی اولیه | تاریخچه، نصب، تنظیمات اولیه |
| ۱ | شروع کار و هسته ابزار | Dashboard، Scope، Site Map |
| ۲ | کانفیگ و شخصی‌سازی | Project/User Options، فایل‌های کانفیگ |
| ۳ | تمام ماژول‌ها و ابزارها | پوشش کامل Proxy، Intruder، Repeater، Scanner، Collaborator و ابزارهای کمکی |
| ۴ | پیاده‌سازی اسکن‌ها | تنظیمات و اجرای اسکن‌های مختلف |
| ۵ | پیاده‌سازی حملات | Authentication، Authorization، Input Validation، Business Logic |
| ۶ | تست نفوذ وب و Bug Hunting | متدولوژی OWASP، پیاده‌سازی SQLi، XSS، CSRF، SSRF، XXE، JWT |
| ۷ | تکنیک‌های پیشرفته رهگیری | WebSocket، HTTP/2، Custom Rules، TLS 1.3 + mitmproxy |
| ۸ | مدیریت نشست و Macros | Session Handling Rules + Macros  |
| ۹ | mitmproxy vs Burp Suite | مقایسه و مکمل بودن، سناریوی ترکیبی  |
| ۱۰ | خودکارسازی و توسعه | BChecks، Bambdaها، Montoya API، REST API |
| ۱۱ | جمع‌بندی | مرور کلی و تأکید بر اصول اخلاقی |

## فصل Burp Suite - سرفصل ها - پارت 2
### بخش ۱: مبانی و معماری (Burp Suite: Foundations & Architecture)

- تاریخچه Burp Suite و جایگاه آن به عنوان **استاندارد صنعتی** تست نفوذ وب[](https://cyberleveling.com/blog/web-app-security-tools-compared)
    
- مقایسه جامع نسخه‌ها: **Community vs Professional vs Enterprise**[](https://cyberleveling.com/blog/web-app-security-tools-compared)
    
    - محدودیت‌های Intruder در نسخه Community
        
    - ویژگی‌های انحصاری Professional: Scanner، Collaborator، و Burp AI Extension[](https://github.com/iosec-shekhar/awesome-ai-security)
        
- معماری کلی: Proxy-Server Architecture و نحوه تعامل ماژول‌ها
    
- نصب و راه‌اندازی اولیه در سیستم‌های عامل مختلف
    
- مدیریت پروژه و ذخیره‌سازی وضعیت (Project Files)
    

> **مقایسه با رقبا:** توضیح تفاوت Burp با ZAP (رایگان و متن‌باز) و Caido (ابزار مدرن با تمرکز بر عملکرد) برای شفاف‌سازی انتخاب Burp[](https://news.ycombinator.com/item?id=46334485)[](https://cyberleveling.com/blog/web-app-security-tools-compared).

---

### بخش ۲: درک ترافیک وب (HTTP/HTTPS Fundamentals in Burp)

- ساختار درخواست و پاسخ HTTP (Method, Headers, Body)
    
- پروکسی شفاف (Transparent Proxy) و پروکسی صریح (Explicit Proxy)
    
- آشنایی با **Proxy Listenerها** و تنظیمات پورت
    
- مدیریت گواهی‌های SSL برای رمزگشایی ترافیک HTTPS
    
- کار با **WebSocket**ها (Proxy, Repeater, Scanner)[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    

---

### بخش ۳: ابزارهای هسته (Core Tools)

#### ۳.۱. Dashboard

- نمای کلی فعالیت و رویدادها
    
- مدیریت **Live Tasks** (اسکن خودکار در حین مرور)[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    

#### ۳.۲. Target

- **Site Map:** نقشه درختی از محتوای برنامه
    
- **Scope Definition:** تعیین محدوده تست برای کاهش نویز
    
- **Content Discovery:** کشف محتوای پنهان (فقط در نسخه Professional)[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    

#### ۳.۳. Proxy

- **Intercept:** رهگیری و تغییر لحظه‌ای درخواست‌ها
    
- **HTTP History:** تاریخچه ترافیک عبوری
    
- **WebSockets History:** تاریخچه ترافیک WebSocket
    
- **Options:** تنظیمات پیشرفته پروکسی (Match and Replace، SSL Pass Through، Response Modification)
    
- **Match and Replace:** قوانین جایگزینی خودکار در هدرها یا بدنه
    

#### ۳.۴. Intruder

- **اتوماسیون حملات:** Fuzzing، Brute Force، Enumeration
    
- **انواع Attack:** Sniper، Battering ram، Pitchfork، Cluster bomb
    
- **Positionها و Payloadها:** تعیین نقاط تزریق
    
- **محدودیت‌های نسخه Community** در مقایسه با Professional[](https://cyberleveling.com/blog/web-app-security-tools-compared)
    

#### ۳.۵. Repeater

- ارسال مجدد و ویرایش دستی درخواست‌ها
    
- **Comparison:** مقایسه پاسخ‌ها برای تحلیل تفاوت‌ها
    
- **Render:** نمایش پاسخ HTML در مرورگر داخلی
    

#### ۳.۶. Scanner (ویژه Professional)

- **Active Scanning:** اسکن خودکار آسیب‌پذیری‌ها (SQLi، XSS، SSRF)
    
- **Passive Scanning:** تحلیل منفعل ترافیک
    
- **Scan Queue و Scan Results**
    
- **Authenticated Scanning:** اسکن نواحی ممیزی شده[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    

#### ۳.۷. Collaborator (ویژه Professional)

- تشخیص آسیب‌پذیری‌های خارج از باند (Out-of-Band)
    
- Blind SSRF، Blind XSS، و Blind SQLi
    

#### ۳.۸. ابزارهای کمکی

- **Decoder:** رمزگشایی و رمزگذاری (Base64، URL، HTML و...)
    
- **Comparer:** مقایسه بصری دو داده
    
- **Sequencer:** تحلیل کیفیت تصادفی بودن توکن‌ها
    
- **Logger:** ثبت و جستجوی پیشرفته ترافیک[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **Organizer:** جمع‌آوری و annotation درخواست‌ها
    

---

### بخش ۴: اسکنر سفارشی با BChecks (سطح متوسط)

مطابق با مستندات PortSwigger، **BChecks** یک زبان دامنه‌خاص (DSL) برای **نوشتن قوانین اسکنر سفارشی** بدون نیاز به توسعه افزونه کامل است[](https://portswigger.net/burp/documentation/desktop/extend-burp)[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating).

**سرفصل‌های این بخش:**

- BCheck چیست و چه تفاوتی با Extension دارد؟
    
- ساختار یک فایل BCheck (metadata، وابستگی‌ها، و قوانین تشخیص)
    
- نوشتن قانون برای تشخیص آسیب‌پذیری خاص (مثال: شناسایی هدرهای امنیتی缺失)
    
- تست و اشکال‌زدایی BCheckها
    
- اشتراک‌گذاری و وارد کردن BCheck از جامعه
    

---

### بخش ۵: شخصی‌سازی با Bambdaها (سطح متوسط)

طبق مستندات PortSwigger، **Bambdaها** قطعه‌کدهای کوچک جاوا هستند که مستقیماً در رابط Burp اجرا می‌شوند و نیازی به راه‌اندازی پروژه ندارند[](https://portswigger.net/burp/documentation/desktop/extend-burp).

**سرفصل‌های این بخش:**

- Bambda چیست؟ (Bambda = Lambda + Burp)
    
- انواع Bambda: **Request** (پیش از ارسال)، **Response** (پس از دریافت)، **WebSocket**
    
- سناریوهای کاربردی:
    
    - فیلتر کردن خودکار درخواست‌های حاوی توکن‌های خاص
        
    - افزودن هدرهای امنیتی به تمام درخواست‌ها
        
    - استخراج داده‌های خاص از پاسخ‌ها
        
- ذخیره و اشتراک‌گذاری Bambdaها
    

---

### بخش ۶: توسعه افزونه با Montoya API (سطح پیشرفته)

طبق مستندات رسمی PortSwigger، **Montoya API** جدیدترین و اصلی‌ترین رابط برنامه‌نویسی برای توسعه افزونه‌های Burp است و جایگزین API قدیمی شده است[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating/first-extension)[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating).

#### ۶.۱. مبانی توسعه

- Montoya API در مقابل API قدیمی
    
- راه‌اندازی محیط توسعه (IDE: IntelliJ IDEA / Eclipse)
    
- ایجاد پروژه Maven/Gradle با **Starter Project** رسمی[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating/first-extension)[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating)
    
- پیاده‌سازی اینترفیس `BurpExtension` و متد `initialize()`
    

#### ۶.۲. افزودن قابلیت‌های UI

- ثبت آیتم در **Context Menu** (منوی کلیک راست)[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating/first-extension)
    
- اضافه کردن **Tab سفارشی** به Burp Suite[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating/first-extension)
    
- کار با **Tableهای سفارشی** و نمایش داده
    
- استفاده از **Swing Components** در افزونه
    

#### ۶.۳. تعامل با ترافیک

- **HttpHandler:** رهگیری و تغییر درخواست‌ها و پاسخ‌ها پیش از ارسال
    
- **WebSocketHandler:** مدیریت ارتباطات WebSocket
    
- **Proxy.*** و **Intruder.*** APIها
    

#### ۶.۴. مدیریت داده و وضعیت

- **Persistance** (ذخیره‌سازی داده بین جلسات)
    
- **Session Handling:** مدیریت نشست‌های کاربر
    
- **Scope:** تعیین محدوده‌های سفارشی
    

#### ۶.۵. افزونه‌های هوش مصنوعی (AI-Powered Extensions)

طبق مستندات PortSwigger، Montoya API از **ادغام قابلیت‌های AI** پشتیبانی می‌کند[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating). این بخش می‌تواند شامل:

- ارسال پرامپت به LLM از داخل افزونه
    
- تحلیل پاسخ‌های دریافتی و تولید payload هوشمند
    
- مثال عملی: افزونه‌ای که خودکار XSS را تشخیص می‌دهد و payload تولید می‌کند
    

#### ۶.۶. تست افزونه و اشکال‌زدایی

- بارگذاری فایل JAR در Burp (Extensions > Installed)
    
- مشاهده خروجی در Output/Error tabها
    
- استفاده از **Logging API** برای دیباگ
    

---

### بخش ۷: BApp Store و اکوسیستم افزونه‌ها

- مرور **BApp Store** و نصب افزونه‌های آماده:
    
    - **Turbo Intruder:** حملات موازی با سرعت بالا
        
    - **Param Miner:** کشف پارامترهای پنهان
        
    - **Autorize:** تست سطح دسترسی
        
    - **Active Scan++:** اسکنر مکمل
        
- به‌روزرسانی و مدیریت افزونه‌های نصب شده
    
- ارزیابی امنیتی افزونه‌های شخص ثالث
    

---

### بخش ۸: پیاده‌سازی حملات (Implement Common Attacks)

- **Authentication Testing:** Bypass، Rate Limiting، Session Fixation[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **File Upload Vulnerabilities:** Bypass پسوند و محتوا با Proxy و Repeater[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **Input Validation (SQLi, XSS, Command Injection):** استفاده از Intruder با payloadهای هدفمند[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **HTTP Methods Exploitation:** تست روش‌های غیرمجاز (PUT, DELETE, TRACE) با Repeater[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **Testing WebSockets:** رهگیری و تغییر پیام‌های WebSocket[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **Authenticated Scanning:** پیکربندی Session Handling Rules برای اسکن نواحی لاگین شده[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **API Testing (REST & GraphQL):** اسکنر و دستکاری درخواست‌های API
    

---

### بخش ۹: خودکارسازی و اسکریپت‌نویسی (Automation)

- استفاده از **REST API** Burp (نسخه Professional)
    
- **خط فرمان (Command Line):** اجرای Burp بدون UI با آرگومان‌های `--project-file` و `--config-file`
    
- **Live Tasks:** تنظیم اسکن‌های خودکار در حین مرور[](https://www.coursera.org/programs/st-vincent-pallotti-college-of-engineering-and-te-on-coursera-opkag/learn/packt-burp-suite-unfiltered-go-from-a-beginner-to-advanced-9muoz?authMode=login)
    
- **Session Handling Rules:** تعریف قوانین پیچیده برای مدیریت احراز هویت
    

---

### بخش ۱۰: گزارش‌گیری و مستندسازی (Reporting)

- تولید گزارش خودکار از Scanner
    
- سفارشی‌سازی گزارش (قالب HTML، XML، JSON)
    
- **Export Issues:** خروجی گرفتن از آسیب‌پذیری‌های یافت شده
    
- **Issue Activity Log:** تاریخچه فعالیت‌ها
    
- بهترین روش‌ها برای گزارش‌دهی حرفه‌ای
    

---

### بخش ۱۱: تعمیر و عیب‌یابی (Troubleshooting & Optimization)

- **کارایی (Performance):** مدیریت حافظه، محدودیت‌های Scope
    
- مشکلات رایج SSL و راه‌حل‌ها
    
- رفع تداخل افزونه‌ها
    
- **Logging پیشرفته:** تنظیم سطح لاگ برای دیباگ عمیق
    
- مسائل رایج در شبکه‌های شرکتی (Proxy chaining)
    

---

### بخش ۱۲: Burp Suite vs The World (مقایسه نهایی)

|ابزار|قوت اصلی|ضعف اصلی|بهترین کاربرد|
|---|---|---|---|
|**Burp Suite Pro**|جامع‌ترین + اکوسیستم افزونه|هزینه ($399/سال) [](https://cyberleveling.com/blog/web-app-security-tools-compared)|تست نفوذ حرفه‌ای|
|**Burp Suite Community**|رایگان + یکپارچگی با بقیهBurp|بدون Scanner و Intruder محدود[](https://cyberleveling.com/blog/web-app-security-tools-compared)|یادگیری و تست دستی|
|**OWASP ZAP**|رایگان + اسکنر خودکار|کندتر، UI کمتر شهودی[](https://news.ycombinator.com/item?id=46334485)[](https://cyberleveling.com/blog/web-app-security-tools-compared)|DevSecOps و CI/CD|
|**Caido**|سریع + معماری مدرن|هنوز در حال بلوغ[](https://cyberleveling.com/blog/web-app-security-tools-compared)|تست خودکار و محیط‌های بزرگ|
|**Charles Proxy**|ساده و کم‌اصطکاک|تمرکز بر مشاهده نه امنیت[](https://news.ycombinator.com/item?id=46334485)|دیباگ سریع برنامه‌ها|

---

### بخش ۱۳: جمع‌بندی و مسیر پیش رو (Conclusion)

- جمع‌بندی قابلیت‌های کلیدی Burp Suite
    
- تأکید بر **قابلیت توسعه‌پذیری (Extensibility)** به عنوان مزیت رقابتی اصلی[](https://portswigger.net/burp/documentation/desktop/extend-burp)[](https://portswigger.net/burp/documentation/desktop/extend-burp/extensions/creating)
    
- منابع پیشنهادی برای یادگیری بیشتر (PortSwigger Web Security Academy، labs، و مستندات)
    
- **تأکید اصول اخلاقی:** Burp Suite ابزاری قدرتمند است و باید صرفاً در محیط‌های دارای مجوز استفاده شود. PortSwigger دستورالعمل Responsible Disclosure را توصیه می‌کند.
    
- **What's Next؟** پیش‌نمایش از ابزارهای بعدی کتاب (مانند PETEP برای پروتکل‌های غیر HTTP)
    

### جدول نهایی زمان‌بندی پیشنهادی سرفصل‌ها

|بخش|سرفصل|زمان تقریبی تدریس|
|---|---|---|
|۱|مبانی و معماری|۱ جلسه|
|۲|درک ترافیک وب|۱ جلسه|
|۳|ابزارهای هسته (۱۳ زیربخش)|۴-۵ جلسه|
|۴|BChecks|۱ جلسه|
|۵|Bambdaها|۱ جلسه|
|۶|توسعه افزونه (۶ زیربخش)|۳-۴ جلسه|
|۷|BApp Store|۱ جلسه|
|۸|پیاده‌سازی حملات|۲-۳ جلسه|
|۹|خودکارسازی|۱ جلسه|
|۱۰|گزارش‌گیری|۱ جلسه|
|۱۱|عیب‌یابی|۱ جلسه|
|۱۲|مقایسه با سایر ابزارها|۱ جلسه|
|۱۳|جمع‌بندی|۰.۵ جلسه|
|**مجموع**||**~۲۰-۲۲ جلسه**|

---

## فصل Burp Suite - سرفصل ها - پارت 3 - تکمیلی

#### بخش جدید: خودکارسازی پیشرفته و مدیریت پروژه (Advanced Automation & Project Management)

این بخش به دلیل اهمیت بالای **یکپارچگی و اتوماسیون در تست نفوذ حرفه‌ای** و همچنین قابلیت‌های جدید Montoya API در نسخه‌های اخیر (۲۰۲۶) اضافه شده است .

**سرفصل‌های پیشنهادی:**

- **مدیریت حرفه‌ای پروژه‌ها (Project Management in Depth):**
    - کار با فایل‌های پروژه (`.burp`)؛ مزایا و معایب ذخیره‌سازی پروژه در دیسک در مقابل حافظه.
    - **مفهوم `Project Options` در مقابل `User Options`** و اولویت‌بندی آن‌ها (آموزش نحوه بازنویسی تنظیمات کاربر توسط پروژه).
    - اشتراک‌گذاری تنظیمات در تیم: **خروجی و ورودی گرفتن از تنظیمات به صورت JSON** (با استفاده از `exportProjectOptionsAsJson` و `importProjectOptionsFromJson`)، روشی استاندارد برای داشتن کانفیگ یکسان در تیم‌های حرفه‌ای تست نفوذ .

- **مدیریت پیشرفته‌ی تسک‌ها (Advanced Live Tasks & Resource Pools):**
    - **پیکربندی `Live Tasks`** برای اسکن خودکار و غیرفعال در حین کار با Proxy, Repeater و Intruder. نکته مهم: اسکن خودکار می‌تواند بر اساس `In-scope` یا یک `Custom Scope` خاص محدود شود تا نویز آزمایش کاهش یابد .
    - کار با `Resource Pools`: مدیریت حافظه و ترد برای اجرای همزمان چندین اسکن یا حمله بدون کاهش کارایی سیستم.
    - **مدیریت آیتم‌های تکراری (`Ignore duplicate items`)** برای جلوگیری از اتلاف منابع .

- **مدیریت خطا و استراتژی‌های `Session Handling`:**
    - **تکنیک‌های گسترش‌یافته‌ی Macros:** مدیریت سناریوهای پیچیده مانند ورود به سیستم (Login) و استخراج خودکار `CSRF Tokens` یا `Nonce`ها جهت استفاده در حملات Brute Force یا Fuzzing.
    - **مفهوم `Cookie Jar`:** مدیریت خودکار کوکی‌ها در Macros و درک نحوه عملکرد `Session Handling Rules`.

#### بخش جدید: اکوسیستم توسعه و یکپارچگی (The Burp Ecosystem & Integration)

این بخش برای پوشش دادن قابلیت‌های منحصربه‌فرد Burp در **توسعه سفارشی و تعامل با دنیای خارج** طراحی شده است که رقیب‌پذیری آن را تضمین می‌کند .

**سرفصل‌های پیشنهادی:**

- **انتخاب مسیر درست برای شخصی‌سازی (Choosing the Right Extensibility Option):**
    - مقایسه و راهنمای انتخاب بین سه سطح شخصی‌سازی:
        - **Bambdas:** برای فیلتر کردن ساده یا تغییرات کوچک و سریع (بدون نیاز به محیط توسعه) .
        - **BChecks:** برای تعریف قوانین اسکنر سفارشی و دقیق.
        - **Full Extensions (Montoya API):** برای قابلیت‌های پیچیده مانند اضافه کردن تب‌ها، دکمه‌ها، یا ادغام با ابزارهای دیگر (مانند Nuclei, ffuf) .

- **Extension Development in Practice (Montoya API):**
    - **مدیریت چرخه حیات افزونه:** نحوه مدیریت حافظه و بررسی `Extension Editors`ها برای جلوگیری از `Loop`های بینهایت که باعث کرش Burp می‌شود .
    - **توسعه افزونه با زبان‌های دیگر (Kotlin, Python, Ruby):** محدودیت‌ها و کاربردهای `Jython` و `JRuby` در مقابل `Java` و `Kotlin` .
    - **مثال کاربردی:** نوشتن یک افزونه ساده برای لاگ کردن تمام درخواست‌ها (Request) و پاسخ‌ها (Response) در یک فایل خارجی .

- **Burp Suite و هوش مصنوعی (AI-Powered Extensions):**
    - معرفی قابلیت‌های **Creating AI extensions** در Montoya API.
    - بررسی مفهوم **Vibe coding** با LLMها برای توسعه افزونه: نحوه استفاده از فایل‌های `CLAUDE.md` در پروژه‌های استارتر برای آموزش مدل‌های زبانی جهت تولید کد افزونه .

#### به‌روزرسانی بخش: مدیریت نشست و Macros (با نگاه به سناریوهای واقعی)

این بخش برای تکمیل بخش ۸ سرفصل‌های قبلی با جزئیات عملی‌تر و عیب‌یابی آن اضافه می‌شود.

**سرفصل‌های پیشنهادی:**

- **ماکروها در عمل (Macros in Action):**
    - **مرحله به مرحله ایجاد Macro برای Bypassing CSRF Tokens:** شامل انتخاب درخواست در Macro Recorder، پیکربندی `Custom parameter locations` در پاسخ (Response)، و تنظیم Regex برای استخراج خودکار Token .
    - **Step-by-step ایجاد `Session Handling Rule`:** نحوه اتصال Macro به Rule، تنظیم Scope برای اجرای Rule تنها روی Host خاص و ابزارهای مشخص (مانند Intruder) و تست نهایی در Repeater برای اطمینان از به‌روزرسانی Token .

#### به‌روزرسانی بخش: عیب یابی و نکات حرفه‌ای (Troubleshooting & Best Practices)

شامل نکات ظریف و تجربی که یک کاربر حرفه‌ای باید بداند.

**سرفصل‌های پیشنهادی:**

- **عیب‌یابی (Troubleshooting):**
    - **مشکل HTTP/2 در افزونه‌ها:** توضیح این نکته که افزونه‌ها نمی‌توانند درخواست‌های `Kettled` (HTTP/2 که توسط Burp به HTTP/1.1 تبدیل شده) را مستقیماً تغییر دهند و باید از فرمت استاندارد استفاده کرد .
    - **مشکل Import خودکار تنظیمات با API:** برخی مشکلات شناخته شده در `importUserOptionsFromJson` که ممکن است باعث بازگشت به تب Dashboard شود؛ راهکار استفاده از GUI برای Import حساس .

---

### ساختار نهایی و جامع سرفصل‌های فصل Burp Suite (نسخه به‌روزرسانی شده)

در اینجا ساختار نهایی را مشاهده می‌کنید که ترکیبی از سرفصل‌های قبلی و موارد جدید پیشنهادی است:

- **بخش ۰:** مبانی و آشنایی اولیه (بدون تغییر)
- **بخش ۱:** شروع کار و هسته ابزار (بدون تغییر)
- **بخش ۲:** روش‌های کانفیگ و شخصی‌سازی
- **بخش ۳:** توضیح تمام قسمت‌ها، منوها، گزینه‌ها و ماژول‌ها
- **بخش ۴:** پیاده‌سازی انواع اسکن‌های مختلف
- **بخش ۵:** پیاده‌سازی انواع Attack & Exploitation
- **بخش ۶:** تست نفوذ وب و Bug Hunting
- **بخش ۷:** تکنیک‌های پیشرفته رهگیری
- **بخش ۸:** مدیریت نشست و **Macros (تکمیل شده با مراحل عملی Bypass CSRF Token)**
- **بخش جدید ۹:** **خودکارسازی پیشرفته و مدیریت پروژه (شامل: مدیریت فایل پروژه، JSON Configs، Live Tasks، و Resource Pools)**
- **بخش جدید ۱۰:** **اکوسیستم توسعه و یکپارچگی (شامل: راهنمای انتخاب ابزار شخصی‌سازی، Extension Development با Montoya API، و AI Extensions)**
- **بخش ۱۱:** mitmproxy vs Burp Suite (بدون تغییر)
- **بخش ۱۲:** **عیب یابی و بهترین روش‌های حرفه‌ای (شامل: مشکلات HTTP/2, و Import/Export تنظیمات)**
- **بخش ۱۳:** جمع‌بندی نهایی و تأکید بر اصول اخلاقی



## فصل Burp Suite - سرفصل ها - ترکیب پارت ها - ورژن Mistral

## فصل Burp Suite - سرفصل ها - ترکیب تمام پارت ها(1 و 2) - Final

### بخش ۰: مبانی و آشنایی اولیه (Foundations)

| کد  | سرفصل                                              | زیربخش‌ها                                                                                         |   زمان   |
| :-: | :------------------------------------------------- | :------------------------------------------------------------------------------------------------ | :------: |
| ۰.۱ | Burp Suite: Introduction & History                 | تاریخچه، فلسفه طراحی، تکامل ابزار تا نسخه ۲۰۲۶                                                    | ۰.۲ جلسه |
| ۰.۲ | Burp Suite: Editions and Installation Guide        | مقایسه Community/Professional/Enterprise، نصب در Windows/Linux/macOS، پیش‌نیازها (JVM, Heap size) | ۰.۴ جلسه |
| ۰.۳ | Burp Suite: User Interface and Basic Configuration | ساختار UI، تنظیمات اولیه پروکسی (پورت ۸۰۸۰)، Burp's Browser، نصب CA Certificate                   | ۰.۴ جلسه |

**مجموع بخش ۰: ۱ جلسه**

---

### بخش ۱: معماری و درک ترافیک وب (Architecture & Web Traffic Fundamentals)

| کد  | سرفصل                                                         | زیربخش‌ها                                                                                                                                                                            |   زمان   |     |
| :-: | :------------------------------------------------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------: | --- |
| ۱.۱ | شروع کار با Burp Suite                                        | راه‌اندازی اولین جلسه تست، پیکربندی مرورگر خارجی، مفاهیم Intercept/Forward/Drop                                                                                                      | ۰.۳ جلسه |     |
| ۱.۲ | Burp Suite: Core Features & Dashboard                         | Dashboard، Live Tasks، Issue Activity                                                                                                                                                | ۰.۳ جلسه |     |
| ۱.۳ | Burp Suite: Target Scope and Site Map                         | Target Scope (Normal/Advanced)، Site Map، فیلتر in-scope، import/export                                                                                                              | ۰.۴ جلسه |     |
| ۱.۴ | درک ترافیک وب و مبانی پروتکل های وب (HTTP/HTTPS Fundamentals) | ساختار درخواست/پاسخ، پروکسی شفاف/صریح، Proxy Listener، مدیریت گواهی SSL، WebSocket<br><br>ساختار **HTTP/1.1, HTTP/2, HTTP/3**، تفاوت **HTTPS vs HTTP**، مدیریت **گواهی‌های SSL/TLS** | ۰.۵ جلسه |     |
| ۱.۵ | Burp Suite: Core Architecture                                 | معماری کلی، نحوه تعامل ماژول‌ها، Project vs User Options                                                                                                                             | ۰.۵ جلسه |     |

**مجموع بخش ۱: ۲ جلسه**

---

### بخش ۲: ابزارهای هسته Burpsuite (Core Tools) – کامل‌ترین مرجع

| کد  | سرفصل                            | زیربخش‌ها                                                                                                                                   |   زمان   |
| :-: | :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ | :------: |
| ۲.۱ | Proxy                            | Intercept، HTTP History، WebSockets History، Options، Match and Replace، SSL Pass Through                                                   |  ۱ جلسه  |
| ۲.۲ | Target                           | Site Map، Scope (Normal & Advanced)، Content Discovery (نسخه Professional)                                                                  | ۰.۵ جلسه |
| ۲.۳ | Intruder                         | Attack Types (Sniper, Battering ram, Pitchfork, Cluster bomb)، Positions، Payloads Settings (۱۳ نوع)، Attack Results، محدودیت‌های Community | ۱.۵ جلسه |
| ۲.۴ | Repeater                         | ارسال مجدد، Comparison، Render، نام‌گذاری تب‌ها                                                                                             | ۰.۵ جلسه |
| ۲.۵ | Scanner (نسخه Professional)      | Active vs Passive Scanning، Scan Queue، Authenticated Scanning، Live Scanning                                                               | ۰.۵ جلسه |
| ۲.۶ | Collaborator (نسخه Professional) | تشخیص آسیب‌پذیری‌های خارج از باند (Blind SSRF, Blind XSS, Blind SQLi)                                                                       | ۰.۳ جلسه |
| ۲.۷ | ابزارهای کمکی                    | Decoder، Comparer، Sequencer، Logger، Organizer                                                                                             | ۰.۵ جلسه |
| ۲.۸ | Extender                         | مدیریت افزونه‌ها، بارگذاری افزونه‌های Java/Python/Ruby                                                                                      | ۰.۲ جلسه |
| ۲.۹ | BApp Store                       | مرور، نصب، به‌روزرسانی، ارزیابی امنیتی افزونه‌ها                                                                                            | ۰.۵ جلسه |

**مجموع بخش ۲: ۵.۵ جلسه**

---

### بخش ۳: کانفیگ، شخصی‌سازی و روش‌های پیشرفته پیکربندی

| کد  | سرفصل                         | زیربخش‌ها                                                                                          |   زمان   |
| :-: | :---------------------------- | :------------------------------------------------------------------------------------------------- | :------: |
| ۳.۱ | روش‌های کانفیگ و شخصی‌سازی    | Project Options، User Options، Configuration Files (JSON/XML)، Tabs Refactoring، Look and Feel     | ۰.۵ جلسه |
| ۳.۲ | مدیریت حرفه‌ای پروژه‌ها       | فایل‌های پروژه (.burp)، Project Options vs User Options، اولویت‌بندی، اشتراک‌گذاری تنظیمات با JSON | ۰.۵ جلسه |
| ۳.۳ | مدیریت پیشرفته تسک‌ها و منابع | Live Tasks، Resource Pools، Ignore duplicate items                                                 | ۰.۵ جلسه |
| 3.4 | پیکربندی شبکه و پروکسی        | **Proxy Listeners** (Multi-Interface), **Upstream Proxy**, **PAC Files**, **Transparent Proxy**    | ۰.۵ جلسه |

**مجموع بخش ۳: ۱.۵ جلسه**

---

### بخش ۴: BChecks و Bambdaها (شخصی‌سازی سریع)

| کد | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| ۴.۱ | BChecks – Scanner Rules سفارشی | BCheck چیست؟ ساختار فایل (metadata، وابستگی‌ها، قوانین)، نوشتن قانون سفارشی، تست و اشکال‌زدایی، اشتراک‌گذاری | ۱ جلسه |
| ۴.۲ | Bambdaها – شخصی‌سازی بدون کامپایل | Bambda چیست؟ انواع (Request, Response, WebSocket)، سناریوهای کاربردی (فیلتر، افزودن هدر، استخراج داده) | ۱ جلسه |
| ۴.۳ | انتخاب مسیر درست برای شخصی‌سازی | مقایسه Bambdas vs BChecks vs Full Extensions + راهنمای انتخاب | ۰.۵ جلسه |

**مجموع بخش ۴: ۲.۵ جلسه**

---

### بخش ۵: مدیریت نشست (Session Handling) و Macros 

| کد  | سرفصل                                    | زیربخش‌ها                                                                                                                                |   زمان   |
| :-: | :--------------------------------------- | :--------------------------------------------------------------------------------------------------------------------------------------- | :------: |
| ۵.۱ | Session Handling Rules                   | مفهوم مدیریت نشست خودکار، انواع قوانین (Run macro, Validate session, Enforce session, Prompt for credentials)، تنظیم Scope               | ۰.۵ جلسه |
| ۵.۲ | Macros در عمل                            | Macro چیست؟ کاربردها (لاگین خودکار، استخراج Token، بازسازی نشست)، روش ایجاد Macro مرحله به مرحله، Custom parameter locations، Cookie Jar |  ۱ جلسه  |
| ۵.۳ | سناریوی عملی: Bypass CSRF Token با Macro | مراحل کامل: Macro Recorder → Custom locations → Regex extraction → Session Handling Rule → تست در Repeater                               | ۰.۵ جلسه |
| ۵.۴ | مدیریت خطا و استراتژی‌های پیشرفته        | عیب‌یابی Macros، مدیریت سناریوهای پیچیده ورود به سیستم - مدیریت **Multi-Step Logins**, **Error Handling**, **Debugging Macros**          | ۰.۵ جلسه |

**مجموع بخش ۵: ۲.۵ جلسه**

---

### بخش ۶: پیاده‌سازی حملات و تست نفوذ (Attacks & Exploitation)

| کد | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| ۶.۱ | پیاده‌سازی انواع اسکن‌ها | پیکربندی اسکنر (Fast/Normal/Deep)، اسکن دستی، اسکن خودکار، اسکن احراز هویت شده، اسکن APIها، بهینه‌سازی | ۱ جلسه |
| ۶.۲ | پیاده‌سازی انواع Attack & Exploitation | Authentication Attacks، Authorization Attacks (IDOR)، Input Validation (SQLi, XSS, Command Injection, XXE, SSRF)، Business Logic Attacks، Client-Side Attacks (Clickjacking, CORS)، File Upload Attacks | ۱.۵ جلسه |
| ۶.۳ | تست نفوذ وب و Bug Hunting – متدولوژی | OWASP Testing Guide، نقشه راه Bug Hunter، Param Miner، Autorize، Turbo Intruder | ۱ جلسه |
| ۶.۴ | Implement Common Web Attacks (جزئیات) | SQLi (UNION, Error-based, Blind)، XSS (Reflected, Stored, DOM-based با DOM Invader)، CSRF، SSRF (با Collaborator)، XXE، JWT Attacks | ۱.۵ جلسه |
- جدول فرمت Mistral سرفصل (Attack Implementation & Penetration Testing)

|کد|سرفصل|زیربخش‌ها|زمان|
|---|---|---|---|
|**۶.۱**|**پیکربندی اسکنر**|**Fast/Normal/Deep Scanning**, اسکن دستی، اسکن **Authenticated**, اسکن **APIها**, بهینه‌سازی **Scan Speed**|۱ جلسه|
|**۶.۲**|**پیاده‌سازی حملات رایج**|**Authentication Attacks** (Brute Force, Credential Stuffing), **Authorization Attacks** (IDOR, Broken Access Control)|۱ جلسه|
|**۶.۳**|**Input Validation Attacks**|**SQLi** (UNION, Error-Based, Blind), **XSS** (Reflected, Stored, DOM-Based با **DOM Invader**), **Command Injection**, **XXE**, **SSRF** (با Collaborator)|۱.۵ جلسه|
|**۶.۴**|**Client-Side & Business Logic Attacks**|**Clickjacking**, **CORS Misconfigurations**, **File Upload Attacks**, **JWT Attacks** (None Algorithm, Weak Secrets)|۱ جلسه|
|**۶.۵**|**متدولوژی تست نفوذ**|**OWASP Testing Guide**, نقشه راه **Bug Hunter**, ابزارهای مکمل (**Param Miner, Autorize, Turbo Intruder**)|۰.۵ جلسه|

**مجموع بخش ۶: ۵ جلسه**

---

### بخش ۷: تکنیک‌های پیشرفته رهگیری (Advanced Interception)

| کد | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| ۷.۱ | تکنیک‌های پیشرفته رهگیری | WebSocket Interception، HTTP/2 Interception، Custom Intercept Rules، Response Modification | ۰.۵ جلسه |
| ۷.۲ | TLS 1.3 Workaround | محدودیت Burp در TLS 1.3، معرفی mitmproxy به عنوان لایه میانی، سناریوی ترکیبی (Burp + mitmproxy) | ۰.۵ جلسه |

**مجموع بخش ۷: ۱ جلسه**

---

### بخش ۸: خودکارسازی و توسعه پیشرفته (Automation & Extension Development)

| کد  | سرفصل                                       | زیربخش‌ها                                                                                                              |   زمان   |
| :-: | :------------------------------------------ | :--------------------------------------------------------------------------------------------------------------------- | :------: |
| ۸.۱ | خودکارسازی با REST API (Professional)       | فعال‌سازی API و احراز هویت، نقاط پایانی اصلی (شروع/توقف اسکن، دریافت نتایج، مدیریت پروژه)                              | ۰.۵ جلسه |
| ۸.۲ | Montoya API (Extension Development) – مبانی | مقایسه Montoya API vs API Legacy، راه‌اندازی محیط توسعه (IDE)، پیاده‌سازی BurpExtension، متد initialize()              | ۰.۵ جلسه |
| ۸.۳ | Montoya API – افزودن UI                     | افزودن Tab سفارشی، Context Menu، Table سفارشی، Swing Components                                                        | ۰.۵ جلسه |
| ۸.۴ | Montoya API – تعامل با ترافیک               | HttpHandler (رهگیری و تغییر)، WebSocketHandler، Proxy.* API، Intruder.* API                                            | ۰.۵ جلسه |
| ۸.۵ | Montoya API – مدیریت داده و وضعیت           | Persistence (ذخیره‌سازی بین جلسات)، Session Handling، Scope                                                            | ۰.۵ جلسه |
| ۸.۶ | AI-Powered Extensions & Vibe Coding         | یکپارچه‌سازی با LLM، فایل‌های CLAUDE.md برای آموزش مدل، تولید کد افزونه با هوش مصنوعی (Vibe Coding)                    | ۰.۵ جلسه |
| ۸.۷ | Extension Development in Practice           | مدیریت چرخه حیات افزونه، توسعه با Kotlin/Python/Ruby (Jython/JRuby)، مثال کاربردی: لاگ کردن درخواست/پاسخ به فایل خارجی |  ۱ جلسه  |

**مجموع بخش ۸: ۴ جلسه**

---

### بخش ۹: گزارش‌گیری و عیب‌یابی (Reporting & Troubleshooting)

| کد | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| ۹.۱ | گزارش‌گیری | تولید گزارش خودکار از Scanner، سفارشی‌سازی (HTML, XML, JSON)، Export Issues، Issue Activity Log، بهترین روش‌ها | ۱ جلسه |
| ۹.۲ | عیب‌یابی (Troubleshooting) | مشکلات HTTP/2 در افزونه‌ها، مشکل Import خودکار تنظیمات با API، کارایی (حافظه، Scope)، مشکلات SSL، تداخل افزونه‌ها، Logging پیشرفته | ۱ جلسه |

**مجموع بخش ۹: ۲ جلسه**

---

### بخش ۱۰: مقایسه و مکمل بودن با سایر ابزارها (Comparison & Complementary Tools)

|  کد  | سرفصل                            | زیربخش‌ها                                                                           |   زمان   |
| :--: | :------------------------------- | :---------------------------------------------------------------------------------- | :------: |
| ۱۰.۱ | Burp Suite vs mitmproxy          | مقایسه ویژگی‌ها، رابط کاربری، قابلیت رمزگشایی TLS 1.3، سناریوی ترکیبی با دستور دقیق | ۰.۵ جلسه |
| ۱۰.۲ | Burp Suite vs OWASP ZAP vs Caido | مقایسه جامع با جدول (قوت‌ها، ضعف‌ها، بهترین کاربرد)                                 | ۰.۵ جلسه |
| ۱۰.۳ | Burp Suite vs Charles Proxy      | تمرکز بر دیباگ در مقابل امنیت                                                       | ۰.۳ جلسه |
| 10.4 | **یکپارچگی با ابزارهای CI/CD**   | **Burp Scanner در Pipelineها**, **Automated Security Testing** (GitLab CI, Jenkins) |   0.5    |

**مجموع بخش ۱۰: ۱.۳ جلسه (تقریباً ۱.۵ جلسه)**

---

### بخش ۱۱: جمع‌بندی نهایی (Conclusion)

| کد | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| ۱۱.۱ | جمع‌بندی و تأکید بر اصول اخلاقی | مرور کلی قابلیت‌های کلیدی، تأکید بر قانونی بودن استفاده، نقل قول از PortSwigger، توصیه Responsible Disclosure | ۰.۵ جلسه |
| ۱۱.۲ | مسیر پیش رو و منابع اضافی | PortSwigger Web Security Academy، مستندات رسمی، لابراتوارهای عملی، پیش‌نمایش ابزارهای بعدی (PETEP, BeEF) | ۰.۳ جلسه |

**مجموع بخش ۱۱: ۰.۸ جلسه (تقریباً ۱ جلسه)**

---

### جدول نهایی – خلاصه زمان‌بندی کل فصل

| شماره بخش | عنوان بخش | زمان (جلسه) |
|:---:|:---|:---:|
| ۰ | مبانی و آشنایی اولیه | ۱ |
| ۱ | معماری و درک ترافیک وب | ۲ |
| ۲ | ابزارهای هسته (Core Tools) | ۵.۵ |
| ۳ | کانفیگ، شخصی‌سازی و پیکربندی پیشرفته | ۱.۵ |
| ۴ | BChecks و Bambdaها (شخصی‌سازی سریع) | ۲.۵ |
| ۵ | مدیریت نشست و Macros (Session Handling) | ۲.۵ |
| ۶ | پیاده‌سازی حملات و تست نفوذ | ۵ |
| ۷ | تکنیک‌های پیشرفته رهگیری | ۱ |
| ۸ | خودکارسازی و توسعه پیشرفته | ۴ |
| ۹ | گزارش‌گیری و عیب‌یابی | ۲ |
| ۱۰ | مقایسه و مکمل بودن با سایر ابزارها | ۱.۵ |
| ۱۱ | جمع‌بندی نهایی | ۱ |

| **مجموع** | | **~۲۹-۳۰ جلسه** |

---

### نکات نهایی
1. **هم‌پوشانی‌های حذف شده:**
   - بخش «خودکارسازی» از جدول ۱ در بخش ۸ (خودکارسازی و توسعه پیشرفته) ادغام شد.
   - بخش «گزارش‌گیری» از جدول ۱ در بخش ۹ قرار گرفت.
   - بخش «BChecks» و «Bambdaها» از جدول ۱ در بخش ۴ ترکیب شدند.
   - بخش «مقایسه با سایر ابزارها» از جدول ۱ در بخش ۱۰ با عنوان «مقایسه و مکمل بودن» گنجانده شد.

2. **موارد اضافه شده از جدول ۲ که در جدول ۱ نبود:**
   - Session Handling Rules و Macros (بخش ۵)
   - TLS 1.3 + mitmproxy (بخش ۷)
   - مدیریت حرفه‌ای پروژه و Resource Pools (بخش ۳)
   - AI-Powered Extensions (بخش ۸)

3. **ساختار نهایی** از ساده به پیچیده و از مبانی به توسعه پیشرفته طراحی شده است و تمام جنبه‌های Burp Suite را پوشش می‌دهد.
## فصل Burp Suite - سرفصل ها - تمام پارت ها - Mistral Version
شما یک ساختار بسیار جامع و منسجم برای فصل **Burp Suite** تدوین کرده‌اید، اما می‌توان با بازنویسی و بهینه‌سازی سرفصل‌ها، آن را **حرفه‌ای‌تر، منسجم‌تر و کامل‌تر** کرد. در این بازنویسی، سعی کردم:
- **ترتیب منطقی** را بهبود بخشم (از مبانی تا پیشرفته).
- **عنوان‌ها و زیربخش‌ها** را شفاف‌تر و جذاب‌تر کنم.
- **موارد مهمی** که ممکن است جا افتاده باشند (مثل **CI/CD Integration**, **Cloud Burp**, **Team Collaboration**) را اضافه کنم.
- **زمان‌بندی** را با توجه به اهمیت هر بخش تنظیم کنم.
- **هم‌پوشانی‌ها** را حذف و **ارتباط بین بخش‌ها** را تقویت کنم.



### **فصل Burp Suite: آناتومی کامل – از مبانی تا توسعه پیشرفته**
*(Anatomy of Burp Suite: From Fundamentals to Advanced Development)*

---

### **بخش ۰: مبانی و آشنایی اولیه**
*(Foundations & Initial Setup)*


سرفصل‌های بخش ۰


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۰.۱** | **مقدمه‌ای بر Burp Suite** | تاریخچه، فلسفه طراحی، تکامل ابزار (از نسخه ۱ تا ۲۰۲۶)، نقش Burp در صنعت امنیت | ۰.۳ جلسه |
| **۰.۲** | **نسخه‌ها و تفاوت‌ها** | مقایسه **Community vs Professional vs Enterprise** (ویژگی‌ها، محدودیت‌ها، قیمت‌گذاری) | ۰.۳ جلسه |
| **۰.۳** | **نصب و پیکربندی اولیه** | نصب در **Windows/Linux/macOS/Docker**، پیش‌نیازها (JVM، Heap Size)، حل مشکلات نصب | ۰.۴ جلسه |

**مجموع بخش ۰:** **۱ جلسه**

---

---

### **بخش ۱: معماری و درک ترافیک وب**
*(Architecture & Web Traffic Fundamentals)*


سرفصل‌های بخش ۱


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۱.۱** | **راه‌اندازی و پیکربندی اولیه** | اولین جلسه تست، پیکربندی مرورگر خارجی (Firefox/Chrome/Burp Browser)، مفاهیم **Intercept/Forward/Drop** | ۰.۳ جلسه |
| **۱.۲** | **داشبورد و ویژگی‌های اصلی** | **Dashboard** (Live Tasks, Issue Activity, Event Logs)، مدیریت **Workspaces** | ۰.۳ جلسه |
| **۱.۳** | **Target Scope و Site Map** | **Target Scope** (Normal/Advanced)، فیلترهای **in-scope/out-of-scope**، Import/Export، **Content Discovery** (Professional) | ۰.۵ جلسه |
| **۱.۴** | **مبانی پروتکل‌های وب** | ساختار **HTTP/1.1, HTTP/2, HTTP/3**، تفاوت **HTTPS vs HTTP**، مدیریت **گواهی‌های SSL/TLS** | ۰.۵ جلسه |
| **۱.۵** | **معماری داخلی Burp Suite** | نحوه تعامل ماژول‌ها، **Project Options vs User Options**， مدیریت **Memory & Performance** | ۰.۴ جلسه |

**مجموع بخش ۱:** **۲ جلسه**

---

---

### **بخش ۲: ابزارهای هسته (Core Tools) – مرجع کامل**
*(Core Tools – Comprehensive Guide)*


سرفصل‌های بخش ۲


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۲.۱** | **Proxy: رهگیری و تحلیل ترافیک** | **Intercept** (HTTP/WebSocket)، **HTTP History**, **WebSockets History**, **Match and Replace**, **SSL Pass Through** | ۱ جلسه |
| **۲.۲** | **Target: مدیریت اهداف** | **Site Map** (Automatic/Manual Crawling)، **Scope Management**, **Content Discovery** (Professional) | ۰.۵ جلسه |
| **۲.۳** | **Intruder: حملات خودکار** | **Attack Types** (Sniper, Battering Ram, Pitchfork, Cluster Bomb)، **Payloads** (13 نوع)، **Grep & Filtering**, محدودیت‌های **Community** | ۱.۵ جلسه |
| **۲.۴** | **Repeater: تست دستی درخواست‌ها** | ارسال مجدد، **Comparison**, **Render**, مدیریت **Tabs** | ۰.۵ جلسه |
| **۲.۵** | **Scanner: اسکن خودکار آسیب‌پذیری‌ها** | **Active vs Passive Scanning**, **Scan Queue**, **Authenticated Scanning**, **Live Scanning** (Professional) | ۰.۵ جلسه |
| **۲.۶** | **Collaborator: تشخیص آسیب‌پذیری‌های Blind** | **Blind SSRF**, **Blind XSS**, **Blind SQLi**, **DNS/HTTP/SMTP Exfiltration** | ۰.۳ جلسه |
| **۲.۷** | **ابزارهای کمکی (Utilities)** | **Decoder** (Encode/Decode), **Comparer**, **Sequencer** (Token Analysis), **Logger**, **Organizer** | ۰.۵ جلسه |
| **۲.۸** | **Extender: مدیریت افزونه‌ها** | نصب **Java/Python/Ruby** افزونه‌ها، **BApp Store** (مرور، نصب، به‌روزرسانی) | ۰.۵ جلسه |

**مجموع بخش ۲:** **۵.۸ جلسه (~۶ جلسه)**

---

---
---
### **بخش ۳: پیکربندی پیشرفته و شخصی‌سازی**
*(Advanced Configuration & Customization)*


سرفصل‌های بخش ۳


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۳.۱** | **پیکربندی حرفه‌ای** | **Project Options vs User Options**, **Configuration Files** (JSON/XML), **Tabs Refactoring**, **Look and Feel** | ۰.۵ جلسه |
| **۳.۲** | **مدیریت پروژه‌ها** | فایل‌های **`.burp`**, اشتراک‌گذاری تنظیمات، **Backup & Restore** | ۰.۵ جلسه |
| **۳.۳** | **مدیریت منابع و تسک‌ها** | **Live Tasks**, **Resource Pools**, **Ignore Duplicate Items**, **Throttling** | ۰.۵ جلسه |
| **۳.۴** | **پیکربندی شبکه و پروکسی** | **Proxy Listeners** (Multi-Interface), **Upstream Proxy**, **PAC Files**, **Transparent Proxy** | ۰.۵ جلسه |

**مجموع بخش ۳:** **۲ جلسه**

---
---
### **بخش ۴: شخصی‌سازی سریع با BChecks و Bambdaها**
*(Rapid Customization with BChecks & Bambda)*


سرفصل‌های بخش ۴


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۴.۱** | **BChecks: قوانین سفارشی اسکنر** | ساختار فایل **`bcheck`** (Metadata, Dependencies, Rules), نوشتن **Custom Scanner Rules**, تست و اشکال‌زدایی | ۱ جلسه |
| **۴.۲** | **Bambdaها: شخصی‌سازی بدون کامپایل** | **Request/Response/WebSocket Bambdas**, سناریوهای کاربردی (فیلتر کردن، افزودن هدر، استخراج داده) | ۱ جلسه |
| **۴.۳** | **مقایسه روش‌های شخصی‌سازی** | **Bambdas vs BChecks vs Full Extensions**, راهنمای انتخاب بهترین روش | ۰.۵ جلسه |

**مجموع بخش ۴:** **۲.۵ جلسه**

---
---
### **بخش ۵: مدیریت نشست و Macros**
*(Session Handling & Macros)*


سرفصل‌های بخش ۵


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۵.۱** | **Session Handling Rules** | **Automatic Session Management**, انواع قوانین (**Run Macro, Validate Session, Enforce Session, Prompt for Credentials**) | ۰.۵ جلسه |
| **۵.۲** | **Macros: خودکارسازی فرآیندها** | **Macro Recorder**, کاربردها (لاگین خودکار، استخراج **CSRF Token**, بازسازی نشست), **Custom Parameter Locations**, **Cookie Jar** | ۱ جلسه |
| **۵.۳** | **سناریوی عملی: Bypass CSRF Token** | مراحل کامل: **Macro Recorder → Regex Extraction → Session Handling Rule → تست در Repeater** | ۰.۵ جلسه |
| **۵.۴** | **عیب‌یابی و استراتژی‌های پیشرفته** | مدیریت **Multi-Step Logins**, **Error Handling**, **Debugging Macros** | ۰.۵ جلسه |

**مجموع بخش ۵:** **۲.۵ جلسه**

---
---
### **بخش ۶: پیاده‌سازی حملات و تست نفوذ**
*(Attack Implementation & Penetration Testing)*


سرفصل‌های بخش ۶


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۶.۱** | **پیکربندی اسکنر** | **Fast/Normal/Deep Scanning**, اسکن دستی، اسکن **Authenticated**, اسکن **APIها**, بهینه‌سازی **Scan Speed** | ۱ جلسه |
| **۶.۲** | **پیاده‌سازی حملات رایج** | **Authentication Attacks** (Brute Force, Credential Stuffing), **Authorization Attacks** (IDOR, Broken Access Control) | ۱ جلسه |
| **۶.۳** | **Input Validation Attacks** | **SQLi** (UNION, Error-Based, Blind), **XSS** (Reflected, Stored, DOM-Based با **DOM Invader**), **Command Injection**, **XXE**, **SSRF** (با Collaborator) | ۱.۵ جلسه |
| **۶.۴** | **Client-Side & Business Logic Attacks** | **Clickjacking**, **CORS Misconfigurations**, **File Upload Attacks**, **JWT Attacks** (None Algorithm, Weak Secrets) | ۱ جلسه |
| **۶.۵** | **متدولوژی تست نفوذ** | **OWASP Testing Guide**, نقشه راه **Bug Hunter**, ابزارهای مکمل (**Param Miner, Autorize, Turbo Intruder**) | ۰.۵ جلسه |

**مجموع بخش ۶:** **۵ جلسه**

---
---
### **بخش ۷: تکنیک‌های پیشرفته رهگیری**
*(Advanced Interception Techniques)*


سرفصل‌های بخش ۷


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۷.۱** | **رهگیری پیشرفته** | **WebSocket Interception**, **HTTP/2 Interception**, **Custom Intercept Rules**, **Response Modification** | ۰.۵ جلسه |
| **۷.۲** | **کار با TLS 1.3** | محدودیت‌های Burp در **TLS 1.3**, معرفی **mitmproxy** به عنوان لایه میانی، سناریوی ترکیبی (**Burp + mitmproxy**) | ۰.۵ جلسه |

**مجموع بخش ۷:** **۱ جلسه**

---
---
### **بخش ۸: خودکارسازی و توسعه افزونه**
*(Automation & Extension Development)*


سرفصل‌های بخش ۸


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۸.۱** | **REST API (Professional)** | فعال‌سازی **API**, احراز هویت، نقاط پایانی اصلی (**Start/Stop Scan, Get Results, Project Management**) | ۰.۵ جلسه |
| **۸.۲** | **Montoya API: مبانی** | **Montoya vs Legacy API**, راه‌اندازی محیط توسعه (**IDE: IntelliJ, VS Code**), پیاده‌سازی **BurpExtension** | ۰.۵ جلسه |
| **۸.۳** | **Montoya API: افزودن UI** | **Custom Tabs**, **Context Menu**, **Custom Tables**, **Swing Components** | ۰.۵ جلسه |
| **۸.۴** | **Montoya API: تعامل با ترافیک** | **HttpHandler** (Intercept & Modify), **WebSocketHandler**, **Proxy.* API**, **Intruder.* API** | ۰.۵ جلسه |
| **۸.۵** | **Montoya API: مدیریت داده** | **Persistence** (ذخیره‌سازی بین جلسات), **Session Handling**, **Scope Management** | ۰.۵ جلسه |
| **۸.۶** | **توسعه افزونه با هوش مصنوعی** | یکپارچه‌سازی با **LLM**, فایل‌های **CLAUDE.md**, تولید کد افزونه با **AI** (Vibe Coding) | ۰.۵ جلسه |
| **۸.۷** | **مثال‌های عملی توسعه افزونه** | توسعه با **Kotlin/Python/Ruby** (Jython/JRuby), مثال: **لاگ کردن درخواست/پاسخ به فایل خارجی** | ۱ جلسه |

**مجموع بخش ۸:** **۴ جلسه**

---
---
### **بخش ۹: گزارش‌گیری و عیب‌یابی**
*(Reporting & Troubleshooting)*


سرفصل‌های بخش ۹


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۹.۱** | **گزارش‌گیری حرفه‌ای** | تولید گزارش **HTML/XML/JSON**, سفارشی‌سازی **Templates**, **Export Issues**, **Issue Activity Log** | ۱ جلسه |
| **۹.۲** | **عیب‌یابی و بهینه‌سازی** | مشکلات **HTTP/2 در افزونه‌ها**, **Import خودکار تنظیمات با API**, **Memory Management**, **SSL Issues**, **Plugin Conflicts**, **Advanced Logging** | ۱ جلسه |

**مجموع بخش ۹:** **۲ جلسه**

---
---
### **بخش ۱۰: مقایسه و یکپارچگی با سایر ابزارها**
*(Comparison & Integration with Other Tools)*


سرفصل‌های بخش ۱۰


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۱۰.۱** | **Burp Suite vs mitmproxy** | مقایسه ویژگی‌ها، **TLS 1.3 Support**, سناریوی ترکیبی با دستور دقیق | ۰.۵ جلسه |
| **۱۰.۲** | **Burp Suite vs OWASP ZAP vs Caido** | مقایسه جامع (**Strengths, Weaknesses, Best Use Cases**) | ۰.۵ جلسه |
| **۱۰.۳** | **Burp Suite vs Charles Proxy** | تمرکز بر **Debugging vs Security Testing** | ۰.۳ جلسه |
| **۱۰.۴** | **یکپارچگی با ابزارهای CI/CD** | **Burp Scanner در Pipelineها**, **Automated Security Testing** (GitLab CI, Jenkins) | ۰.۵ جلسه |

**مجموع بخش ۱۰:** **۱.۸ جلسه (~۲ جلسه)**

---
---
### **بخش ۱۱: جمع‌بندی و منابع اضافی**
*(Conclusion & Additional Resources)*


سرفصل‌های بخش ۱۱


| کد  | سرفصل | زیربخش‌ها | زمان |
|:---:|:---|:---|:---:|
| **۱۱.۱** | **جمع‌بندی و اصول اخلاقی** | مرور **قابلیت‌های کلیدی**, تأکید بر **Legal & Ethical Use**, نقل قول از **PortSwigger**, **Responsible Disclosure** | ۰.۵ جلسه |
| **۱۱.۲** | **مسیر پیش رو** | **PortSwigger Web Security Academy**, مستندات رسمی، لابراتوارهای عملی، پیش‌نمایش ابزارهای بعدی (**PETEP, BeEF, Nuclei**) | ۰.۵ جلسه |

**مجموع بخش ۱۱:** **۱ جلسه**

---
---
---
### **جدول خلاصه زمان‌بندی کل فصل**
خلاه زمان‌بندی


| شماره بخش | عنوان بخش | زمان (جلسه) |
|:---:|:---|:---:|
| ۰ | مبانی و آشنایی اولیه | ۱ |
| ۱ | معماری و درک ترافیک وب | ۲ |
| ۲ | ابزارهای هسته (Core Tools) | ۶ |
| ۳ | پیکربندی پیشرفته و شخصی‌سازی | ۲ |
| ۴ | BChecks و Bambdaها | ۲.۵ |
| ۵ | مدیریت نشست و Macros | ۲.۵ |
| ۶ | پیاده‌سازی حملات و تست نفوذ | ۵ |
| ۷ | تکنیک‌های پیشرفته رهگیری | ۱ |
| ۸ | خودکارسازی و توسعه افزونه | ۴ |
| ۹ | گزارش‌گیری و عیب‌یابی | ۲ |
| ۱۰ | مقایسه و یکپارچگی با سایر ابزارها | ۲ |
| ۱۱ | جمع‌بندی و منابع اضافی | ۱ |
| **مجموع** | | **~۳۱ جلسه** |

---
### **نکات کلیدی بازنویسی**
✅ **افزودن موارد جدید:**
- **CI/CD Integration** (بخش ۱۰.۴)
- **TLS 1.3 Workaround** (بخش ۷.۲)
- **AI-Powered Extensions** (بخش ۸.۶)
- **Montoya API** (بخش ۸.۲ تا ۸.۵)
- **Debugging Macros** (بخش ۵.۴)
- **HTTP/3 Support** (بخش ۱.۴)

✅ **بهبود ساختار:**
- **ترتیب منطقی** از مبانی تا پیشرفته.
- **عنوان‌ها و زیربخش‌ها** شفاف‌تر و جذاب‌تر.
- **زمان‌بندی** با توجه به اهمیت هر بخش تنظیم شد.

✅ **حذف هم‌پوشانی‌ها:**
- ادغام **خودکارسازی** در بخش ۸.
- ادغام **گزارش‌گیری** در بخش ۹.
- ادغام **مقایسه ابزارها** در بخش ۱۰.
# !

