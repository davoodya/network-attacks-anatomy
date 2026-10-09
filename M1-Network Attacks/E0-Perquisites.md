---
Episode: E1
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
aliases:
---
-------
## Perquisites
### Terminology
### Protocols
### Concepts
#### IP Forwarding
- **کارکرد IP Forwarding**:
ویژگی IP Forwarding (که در لینوکس با نام `ip_forward` شناخته می‌شود) قابلیتی در هسته سیستم‌عامل است که به آن اجازه می‌دهد بسته‌های شبکه‌ای که برای خودش مقصد نیستند را به **مسیر بعدی ارسال (Forward)** کند. به طور پیش‌فرض، یک میزبان عادی (Host) بسته‌هایی که آدرس IP مقصد آن‌ها با IP خودش متفاوت است را Drop می‌کند. در واقع، Kernel بسته را دور می‌اندازد چون میگوید "این بسته برای من نیست."

با فعال‌سازی IP Forwarding، ماشین ما از یک **Host** به یک **Router** تبدیل می‌شود. این دقیقاً همان چیزی است که در حملات MITM به آن نیاز است. در این حملات ما می‌خواهیم ترافیک بین هدف و Gateway را ببینیم و دستکاری کنیم، اما نمی‌خواهیم ارتباط آن‌ها قطع شود. با فعال بودن IP Forwarding، ترافیک پس از عبور از کارت شبکه ما، به مقصد اصلی خود هدایت می‌شود و کاربر متوجه بروز MITM نخواهد شد.


- **حملاتی که نیاز به IP Forwarding دارند**:
تمامی حملاتی که در آن‌ها نقش یک **مسیریاب میانی (Intermediary Router)** را ایفا می‌کنیم، نیازمند فعال‌سازی IP Forwarding هستند، تعدادی از این حملات عبارتند از:

| حمله                                   | دلیل نیاز به Forwarding                                                                |
| -------------------------------------- | -------------------------------------------------------------------------------------- |
| **ARP Spoofing / ARP Cache Poisoning** | برای عبور ترافیک از مهاجم به Gateway (حفظ اتصال اینترنت هدف)                           |
| **DHCP Spoofing**                      | برای آنکه مهاجم بتواند ترافیک دستگاه‌های دریافت‌کننده IP را به Gateway واقعی هدایت کند |
| **ICMP Redirect Attack**               | برای Forward کردن بسته‌هایی که از مسیر تغییر یافته می‌آیند                             |
| **DNS Spoofing (ترکیبی)**              | در صورتی که مهاجم فقط DNS را جعل کند و نخواهد ارتباط قطع شود                           |
| **SSLstrip / SSL Downgrade**           | برای انتقال ترافیک HTTP تنزل‌ یافته(Downgrade شده) به مقصد اصلی                        |
| **Rogue Access Point (Evil Twin)**     | برای اتصال کاربران Access Point(نقطه دسترسی) جعلی به اینترنت واقعی                     |

- **نحوه فعالسازی**:
```sh

#روش موقت (با اولین راه‌اندازی مجدد غیرفعال می‌شود)
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
#روش دائمی
sudo sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/' /etc/sysctl.conf
sudo sysctl -p
#بررسی وضعیت فعلی
cat /proc/sys/net/ipv4/ip_forward
#خروجی: 1 = فعال | 0 = غیرفعال
```

#### Key Logger
ابزار Keylogger یک نرم‌افزار یا سخت‌افزار است که به‌صورت مخفی تمامی کلیدهایی که روی صفحه‌ کلید(Keyboard) فشرده میشود را ثبت می‌کند؛ هدفش معمولاً جمع‌آوری رمز عبور، کارت اعتباری یا اطلاعات حساس است.

- **چگونه کار می‌کند؟**
	- نرم‌افزاری که در پس‌زمینه اجرا می‌شود و هر فشار کلید را ثبت می‌کند.
	- سخت‌افزاری، مثل یک پورت USB که کلیدها را مستقیماً می‌گیرد.

- **نشانگرهای وجود**
	- مصرف بالای CPU یا حافظه.
	- برنامه‌های ناشناخته یا مخفی در Task Manager.
	- تغییرات در فایل‌های سیستم که با آنتی ویروس ها شناسایی می‌شوند.

- **روش های دفاعی**
	- استفاده از آنتی‌ویروس معتبر.
	- بررسی دقیق برنامه‌های نصب‌شده و حذف هر چیزی که ناشناخته باشد.
	- تنظیمات فایروال و مانیتورینگ شبکه برای شناسایی ترافیک غیرمعمول.

- **نکتهٔ امنیتی**
- مراقب باشید که نرم‌افزارهای نظارتی را فقط از منابع معتبر نصب کنید.
- برای کاربرهای خانواده یا شرکت، روش‌های نظارت قانونی و با رضایت کاربران را انتخاب کنید.

### Necessary Tools
#### Python
یکی از پیش نیاز های اصلی وپرکاربرد این کتاب که برای اجرای بسیاری از ابزارها نیاز است، Python میباشد. در اینجا آموزش نصب پایتون 3 نسخه 14 که در تاریخ نگارش این کتاب جدیدترین نسخه موجود پایتون است را در انواع سیستم عامل های مختلف بایکدیگر بررسی میکنیم.

1. **نصب در لینوکس های دبیانی (Debian / Ubuntu / Kali / Linux Mint):**
- **1.1: با Package Manager:** 
	- ساده ترین روش نصب پایتون استفاده از Package Manager است.
```sh
sudo apt update
sudo apt install python3 python3-venv python3-pip
```

- **1.2: با کامپایل سورس (حرفه ای)** 
	- روش حرفه ای نصب پایتون کامپایل کدهای پایتون از سورس آن است که در این روش میتوانیم ورژن پایتون که میخواهیم بر روی سیستم نصب شود را دقیق مشخص کنیم. برای تغییر ورژن کافیست بجای `3.14.14` ورژن مورد نظر وارد شود.
```sh
sudo apt update
sudo apt install build-essential libssl-dev zlib1g-dev \
  libncurses5-dev libgdbm-dev libnss3-dev libreadline-dev \
  libffi-dev libsqlite3-dev wget tk-dev liblzma-dev

wget https://www.python.org/ftp/python/3.14.4/Python-3.14.4.tgz
tar -xf Python-3.14.4.tgz
cd Python-3.14.4

./configure --prefix=/usr/local --enable-optimizations
make -j$(nproc)
sudo make altinstall   
```
- نکته: پارامتر altinstall از overwrite شدن python3 جلوگیری می‌کند.

----------

2. **نصب در لینوکس های ردهتی (RedHat/CentOS/Fedora/AlmaLinux/Rocky):**
- **2.1: با Package Manager:** 
	- نکته: در لینوکس های Red Hat قدیمی بجای `dnf` از `yum` استفاده میشود.
```sh
sudo dnf install python3 python3-pip python3-virtualenv
```

- **2.2: با کامپایل سورس (حرفه ای)** 
```sh
#Install requirements 
sudo dnf groupinstall "Development Tools"
sudo dnf install openssl-devel bzip2-devel libffi-devel \
    zlib-devel ncurses-devel readline-devel sqlite-devel \
    tk-devel xz-devel lzma

#install Python
wget https://www.python.org/ftp/python/3.14.4/Python-3.14.4.tgz
tar -xf Python-3.14.4.tgz
cd Python-3.14.4

./configure --enable-optimizations --prefix=/usr/local
make -j$(nproc)
sudo make altinstall

```

---------

3. **نصب در لینوکس های آرچ (Arch Linux/Manjaro/Black Arch) با Package Manager** 
	- در Arch معمولاً آخرین نسخه Python در مخازن موجود است.
```sh
sudo pacman -Syu python python-pip python-virtualenv
```

- اگر نسخه جدیدتری نیاز دارید (AUR):
```sh
yay -S python-git
```

----

4. **نصب در ویندوز:**
- **4.1: نصب از طریق Microsoft Store (روش توصیه‌شده)**
	- ساده‌ترین و auto-update:
	1. وارد Microsoft Store شوید.
	2. عبارت Python را جستجو کنید.
	3. یکی از نسخه‌های رسمی Python Software Foundation را نصب کنید.

- **4.2: نصب از طریق Installer رسمی**
	- دانلود آخرین نسخه از:
	- https://www.python.org/downloads/windows/
	- تنظیمات مهم هنگام نصب:
	- فعال‌کردن گزینه: “Add Python to PATH”
	- انتخاب “Customize installation” برای نصب ‌pip و IDLE و tcl/tk
	- فعال‌کردن “Install for all users” و تنظیم مسیر در:`C:\Program Files\Python312\`

- **4.3: نصب با winget (سریع‌تر)**
	- ابزار winget یک ابزار برای مدیریت بسته ها در ویندوز است. 
```sh
winget install Python.Python.3.12
```

#### !