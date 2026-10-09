---
Episode: Additional-E1
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
-------
### Reference - مرجع ها
1. All Ports Numbers for Protocols
	1. لیست کاملی از پروتکل ها و شماره پورت آنها
### Network Attacks
1. ICMP Redirect
2. DHCP Spoofing
3. Port Stealing
4. DNS spoofing (یا DNS cache poisoning)
5. SSL Stripping
6. Wireless Network Attacks
	1. Deauthentication Attack
	2. Evil Twin Attack
	3. WPA/WPA2 Handshake Capturing
### Web Attacks
### Port Numbers - Full Version (100+)
| #   | پروتکل / سرویس           | پورت(های) پیش‌فرض      | TCP / UDP   | توضیح مختصر                    | سیستم‌عامل‌های معمول    |
| --- | ------------------------ | ---------------------- | ----------- | ------------------------------ | ----------------------- |
| 1   | HTTP                     | 80                     | TCP         | وب‌سرور (Apache, Nginx, IIS)   | Windows, Linux          |
| 2   | HTTPS                    | 443                    | TCP         | وب‌سرور امن (TLS/SSL)          | Windows, Linux          |
| 3   | FTP (کنترل)              | 21                     | TCP         | فایل‌سرور (کنترل)              | Windows, Linux          |
| 4   | FTP (داده)               | 20                     | TCP         | فایل‌سرور (داده)               | Windows, Linux          |
| 5   | SFTP / SSH               | 22                     | TCP         | دستورات امن، انتقال فایل       | Windows, Linux          |
| 6   | Telnet                   | 23                     | TCP         | دستورات (غیر امن)              | Windows, Linux          |
| 7   | SMTP (ارسال ایمیل)       | 25                     | TCP         | ارسال ایمیل                    | Windows, Linux          |
| 8   | SMTPS (SMTP+TLS)         | 465                    | TCP         | ارسال ایمیل امن                | Windows, Linux          |
| 9   | SMTP Submission          | 587                    | TCP         | ارسال ایمیل (TLS)              | Windows, Linux          |
| 10  | POP3                     | 110                    | TCP         | دریافت ایمیل                   | Windows, Linux          |
| 11  | POP3S                    | 995                    | TCP         | دریافت ایمیل امن               | Windows, Linux          |
| 12  | IMAP                     | 143                    | TCP         | دریافت ایمیل                   | Windows, Linux          |
| 13  | IMAPS                    | 993                    | TCP         | دریافت ایمیل امن               | Windows, Linux          |
| 14  | RDP                      | 3389                   | TCP         | دسکتاپ ریموت ویندوز            | Windows                 |
| 15  | VNC                      | 5900                   | TCP         | دسکتاپ ریموت (VNC)             | Windows, Linux          |
| 16  | SMB (CIFS)               | 445                    | TCP         | اشتراک فایل Windows            | Windows, Linux          |
| 17  | NetBIOS Name Service     | 137                    | UDP         | نام‌گذاری NetBIOS              | Windows, Linux          |
| 18  | NetBIOS Datagram Service | 138                    | UDP         | Datagram NetBIOS               | Windows, Linux          |
| 19  | NetBIOS Session Service  | 139                    | TCP         | نشست NetBIOS                   | Windows, Linux          |
| 20  | LDAP                     | 389                    | TCP/UDP     | جستجوی دایرکتوری               | Windows, Linux          |
| 21  | LDAPS                    | 636                    | TCP         | LDAP امن (TLS)                 | Windows, Linux          |
| 22  | DNS                      | 53                     | TCP/UDP     | نام‌گذاری دامنه                | Windows, Linux          |
| 23  | DHCP (Client)            | 68                     | UDP         | درخواست DHCP                   | Windows, Linux          |
| 24  | DHCP (Server)            | 67                     | UDP         | پاسخ DHCP                      | Windows, Linux          |
| 25  | NTP                      | 123                    | UDP         | همگام‌سازی ساعت                | Windows, Linux          |
| 26  | SNMP                     | 161                    | UDP         | مانیتورینگ شبکه                | Windows, Linux          |
| 27  | SNMPTRAP                 | 162                    | UDP         | اعلان خطا                      | Windows, Linux          |
| 28  | RADIUS                   | 1812                   | UDP         | احراز هویت RADIUS              | Windows, Linux          |
| 29  | TACACS+                  | 49                     | TCP         | احراز هویت TACACS+             | Windows, Linux          |
| 30  | MSSQL                    | 1433                   | TCP         | پایگاه‌داده SQLServer          | Windows, Linux          |
| 31  | MySQL                    | 3306                   | TCP         | پایگاه‌داده MySQL              | Windows, Linux          |
| 32  | PostgreSQL               | 5432                   | TCP         | پایگاه‌داده PostgreSQL         | Windows, Linux          |
| 33  | Oracle (Listener)        | 1521                   | TCP         | پایگاه‌داده Oracle             | Windows, Linux          |
| 34  | MongoDB                  | 27017                  | TCP         | پایگاه‌داده MongoDB            | Windows, Linux          |
| 35  | Redis                    | 6379                   | TCP         | حافظهٔ کی‑وی (Redis)           | Windows, Linux          |
| 36  | Memcached                | 11211                  | TCP         | حافظهٔ کی‑وی (Memcached)       | Windows, Linux          |
| 37  | MQTT                     | 1883                   | TCP         | پیام‌رسانی MQTT                | Windows, Linux          |
| 38  | MQTTs                    | 8883                   | TCP         | MQTT امن                       | Windows, Linux          |
| 39  | XMPP (Jabber)            | 5222                   | TCP         | پیام‌رسانی XMPP                | Windows, Linux          |
| 40  | SSH Tunneling            | 22                     | TCP         | همان SFTP/SSH                  | Windows, Linux          |
| 41  | NFS                      | 2049                   | TCP/UDP     | اشتراک فایل Unix               | Windows, Linux          |
| 42  | SMB (SMB1)               | 139                    | TCP         | همان SMB                       | Windows, Linux          |
| 43  | HTTP‑Alt                 | 8080                   | TCP         | وب‌سرور جایگزین                | Windows, Linux          |
| 44  | HTTPS‑Alt                | 8443                   | TCP         | وب‌سرور امن جایگزین            | Windows, Linux          |
| 45  | HTTPS‑Shadowsocks        | 8388                   | TCP         | Shadowsocks                    | Linux                   |
| 46  | OpenVPN (UDP)            | 1194                   | UDP         | VPN                            | Windows, Linux          |
| 47  | OpenVPN (TCP)            | 443                    | TCP         | VPN با TCP                     | Windows, Linux          |
| 48  | WireGuard                | 51820                  | UDP         | VPN WireGuard                  | Linux                   |
| 49  | L2TP                     | 1701                   | UDP         | VPN L2TP                       | Windows, Linux          |
| 50  | PPTP                     | 1723                   | TCP         | VPN PPTP                       | Windows, Linux          |
| 51  | RDP (TLS)                | 443                    | TCP         | RDP بر روی TLS                 | Windows                 |
| 52  | HTTP‑WebSocket           | 80/443                 | TCP         | WebSocket (موجب 80/443)        | Windows, Linux          |
| 53  | SIP                      | 5060                   | TCP/UDP     | VoIP (SIP)                     | Windows, Linux          |
| 54  | SIP TLS                  | 5061                   | TCP         | SIP امن                        | Windows, Linux          |
| 55  | STUN                     | 3478                   | UDP         | تعویض NAT (STUN)               | Windows, Linux          |
| 56  | TURN                     | 3478                   | UDP         | تعویض NAT (TURN)               | Windows, Linux          |
| 57  | RTP                      | 5004                   | UDP         | انتقال صوت/ویدیو               | Windows, Linux          |
| 58  | RTCP                     | 5005                   | UDP         | کنترل RTCP                     | Windows, Linux          |
| 59  | RTMP                     | 1935                   | TCP         | پخش زنده (RTMP)                | Windows, Linux          |
| 60  | H.323                    | 1720                   | TCP/UDP     | VoIP (H.323)                   | Windows, Linux          |
| 61  | IRC                      | 6667                   | TCP         | IRC (مخالف 6667)               | Windows, Linux          |
| 62  | IRC (SSL)                | 6697                   | TCP         | IRC امن                        | Windows, Linux          |
| 63  | SSH (alt)                | 2222                   | TCP         | SSH جایگزین                    | Windows, Linux          |
| 64  | DNSSEC (DNS over TLS)    | 853                    | TCP         | DNS امن                        | Windows, Linux          |
| 65  | DHCPv6 (Server)          | 547                    | UDP         | DHCPv6                         | Windows, Linux          |
| 66  | DHCPv6 (Client)          | 546                    | UDP         | DHCPv6                         | Windows, Linux          |
| 67  | mDNS                     | 5353                   | UDP         | سرویس شناسایی هم‌رده (Bonjour) | Windows, Linux          |
| 68  | SSDP                     | 1900                   | UDP         | سرویس کشف UPnP                 | Windows, Linux          |
| 69  | TFTP                     | 69                     | UDP         | انتقال فایل (TFTP)             | Windows, Linux          |
| 70  | NetBIOS (DNS)            | 137-139                | UDP/TCP     | همان NetBIOS                   | Windows, Linux          |
| 71  | UPnP                     | 1900                   | UDP         | کشف UPnP                       | Windows, Linux          |
| 72  | WS-Discovery             | 3702                   | UDP         | کشف سرویس (WS-Discovery)       | Windows, Linux          |
| 73  | KCP (RUDP)               | 4000                   | UDP         | پروتکل UDP ساده                | Windows, Linux          |
| 74  | NTRIP                    | 2101                   | TCP         | پخش GPS (NTRIP)                | Windows, Linux          |
| 75  | SFTP‑FTP                 | 990                    | TCP         | FTP امن (FTPS)                 | Windows, Linux          |
| 76  | CIFS                     | 445                    | TCP         | همان SMB                       | Windows, Linux          |
| 77  | X11                      | 6000                   | TCP         | سرور X11                       | Linux                   |
| 78  | SSH (SFTP)               | 2022                   | TCP         | SFTP جایگزین                   | Windows, Linux          |
| 79  | HTTPS‑HTTPS-ALPN         | 443                    | TCP         | TLS/SSL ALPN                   | Windows, Linux          |
| 80  | HTTP‑XMPP                | 5269                   | TCP         | XMPP (Jabber)                  | Windows, Linux          |
| 81  | MQTT‑TLS                 | 8883                   | TCP         | MQTT امن                       | Windows, Linux          |
| 82  | WebDAV                   | 8080                   | TCP         | WebDAV (HTTP)                  | Windows, Linux          |
| 83  | HTTPS‑WebDAV             | 8443                   | TCP         | WebDAV امن                     | Windows, Linux          |
| 84  | HTTPS‑Git                | 443                    | TCP         | گیت HTTPS                      | Windows, Linux          |
| 85  | SSH‑Git                  | 22                     | TCP         | گیت SSH                        | Windows, Linux          |
| 86  | Git‑HTTP                 | 9418                   | TCP         | گیت HTTP                       | Windows, Linux          |
| 87  | Zabbix                   | 10051                  | TCP         | Zabbix server                  | Windows, Linux          |
| 88  | Zabbix (agent)           | 10050                  | TCP         | Zabbix agent                   | Windows, Linux          |
| 89  | Plesk                    | 8443                   | TCP         | Plesk panel                    | Windows, Linux          |
| 90  | cPanel                   | 2082/2083/2086         | TCP         | cPanel panel                   | Linux                   |
| 91  | WHM                      | 2087                   | TCP         | WHM panel                      | Linux                   |
| 92  | Webmin                   | 10000                  | TCP         | Webmin panel                   | Linux                   |
| 93  | Docker                   | 2375 (TCP), 2376 (TLS) | TCP         | Docker daemon                  | Windows, Linux          |
| 94  | Kubernetes API           | 6443                   | TCP         | API سرور Kubernetes            | Linux                   |
| 95  | Etcd                     | 2379/2380              | TCP         | etcd cluster                   | Linux                   |
| 96  | Consul                   | 8500                   | TCP         | سرویس Consul                   | Windows, Linux          |
| 97  | Grafana                  | 3000                   | TCP         | داشبورد Grafana                | Windows, Linux          |
| 98  | Prometheus               | 9090                   | TCP         | Prometheus                     | Windows, Linux          |
| 99  | Kibana                   | 5601                   | TCP         | Kibana                         | Windows, Linux          |
| 100 | Elasticsearch            | 9200                   | TCP         | Elasticsearch                  | Windows, Linux          |
| #   | پروتکل / سرویس           | پورت(های) پیش‌فرض      | TCP / UDP   | توضیح مختصر                    | سیستم‌عامل‌های معمول    |
| 1   | HTTP                     | 80                     | TCP         | وب‌سرور (Apache, Nginx, IIS)   | Windows, Linux          |
| 2   | HTTPS                    | 443                    | TCP         | وب‌سرور امن (TLS/SSL)          | Windows, Linux          |
| 3   | FTP (کنترل)              | 21                     | TCP         | فایل‌سرور (کنترل)              | Windows, Linux          |
| 4   | FTP (داده)               | 20                     | TCP         | فایل‌سرور (داده)               | Windows, Linux          |
| 5   | SFTP / SSH               | 22                     | TCP         | دستورات امن، انتقال فایل       | Windows, Linux          |
| 6   | Telnet                   | 23                     | TCP         | دستورات (غیر امن)              | Windows, Linux          |
| 7   | SMTP (ارسال ایمیل)       | 25                     | TCP         | ارسال ایمیل                    | Windows, Linux          |
| 8   | SMTPS (SMTP+TLS)         | 465                    | TCP         | ارسال ایمیل امن                | Windows, Linux          |
| 9   | SMTP Submission          | 587                    | TCP         | ارسال ایمیل (TLS)              | Windows, Linux          |
| 10  | POP3                     | 110                    | TCP         | دریافت ایمیل                   | Windows, Linux          |
| 11  | POP3S                    | 995                    | TCP         | دریافت ایمیل امن               | Windows, Linux          |
| 12  | IMAP                     | 143                    | TCP         | دریافت ایمیل                   | Windows, Linux          |
| 13  | IMAPS                    | 993                    | TCP         | دریافت ایمیل امن               | Windows, Linux          |
| 14  | RDP                      | 3389                   | TCP         | دسکتاپ ریموت ویندوز            | Windows                 |
| 15  | VNC                      | 5900                   | TCP         | دسکتاپ ریموت (VNC)             | Windows, Linux          |
| 16  | SMB (CIFS)               | 445                    | TCP         | اشتراک فایل Windows            | Windows, Linux          |
| 17  | NetBIOS Name Service     | 137                    | UDP         | نام‌گذاری NetBIOS              | Windows, Linux          |
| 18  | NetBIOS Datagram Service | 138                    | UDP         | Datagram NetBIOS               | Windows, Linux          |
| 19  | NetBIOS Session Service  | 139                    | TCP         | نشست NetBIOS                   | Windows, Linux          |
| 20  | LDAP                     | 389                    | TCP/UDP     | جستجوی دایرکتوری               | Windows, Linux          |
| 21  | LDAPS                    | 636                    | TCP         | LDAP امن (TLS)                 | Windows, Linux          |
| 22  | DNS                      | 53                     | TCP/UDP     | نام‌گذاری دامنه                | Windows, Linux          |
| 23  | DHCP (Client)            | 68                     | UDP         | درخواست DHCP                   | Windows, Linux          |
| 24  | DHCP (Server)            | 67                     | UDP         | پاسخ DHCP                      | Windows, Linux          |
| 25  | NTP                      | 123                    | UDP         | همگام‌سازی ساعت                | Windows, Linux          |
| 26  | SNMP                     | 161                    | UDP         | مانیتورینگ شبکه                | Windows, Linux          |
| 27  | SNMPTRAP                 | 162                    | UDP         | اعلان خطا                      | Windows, Linux          |
| 28  | RADIUS                   | 1812                   | UDP         | احراز هویت RADIUS              | Windows, Linux          |
| 29  | TACACS+                  | 49                     | TCP         | احراز هویت TACACS+             | Windows, Linux          |
| 30  | MSSQL                    | 1433                   | TCP         | پایگاه‌داده SQLServer          | Windows, Linux          |
| 31  | MySQL                    | 3306                   | TCP         | پایگاه‌داده MySQL              | Windows, Linux          |
| 32  | PostgreSQL               | 5432                   | TCP         | پایگاه‌داده PostgreSQL         | Windows, Linux          |
| 33  | Oracle (Listener)        | 1521                   | TCP         | پایگاه‌داده Oracle             | Windows, Linux          |
| 34  | MongoDB                  | 27017                  | TCP         | پایگاه‌داده MongoDB            | Windows, Linux          |
| 35  | Redis                    | 6379                   | TCP         | حافظهٔ کی‑وی (Redis)           | Windows, Linux          |
| 36  | Memcached                | 11211                  | TCP         | حافظهٔ کی‑وی (Memcached)       | Windows, Linux          |
| 37  | MQTT                     | 1883                   | TCP         | پیام‌رسانی MQTT                | Windows, Linux          |
| 38  | MQTTs                    | 8883                   | TCP         | MQTT امن                       | Windows, Linux          |
| 39  | XMPP (Jabber)            | 5222                   | TCP         | پیام‌رسانی XMPP                | Windows, Linux          |
| 40  | SSH Tunneling            | 22                     | TCP         | همان SFTP/SSH                  | Windows, Linux          |
| 41  | NFS                      | 2049                   | TCP/UDP     | اشتراک فایل Unix               | Windows, Linux          |
| 42  | SMB (SMB1)               | 139                    | TCP         | همان SMB                       | Windows, Linux          |
| 43  | HTTP‑Alt                 | 8080                   | TCP         | وب‌سرور جایگزین                | Windows, Linux          |
| 44  | HTTPS‑Alt                | 8443                   | TCP         | وب‌سرور امن جایگزین            | Windows, Linux          |
| 45  | HTTPS‑Shadowsocks        | 8388                   | TCP         | Shadowsocks                    | Linux                   |
| 46  | OpenVPN (UDP)            | 1194                   | UDP         | VPN                            | Windows, Linux          |
| 47  | OpenVPN (TCP)            | 443                    | TCP         | VPN با TCP                     | Windows, Linux          |
| 48  | WireGuard                | 51820                  | UDP         | VPN WireGuard                  | Linux                   |
| 49  | L2TP                     | 1701                   | UDP         | VPN L2TP                       | Windows, Linux          |
| 50  | PPTP                     | 1723                   | TCP         | VPN PPTP                       | Windows, Linux          |
| 51  | RDP (TLS)                | 443                    | TCP         | RDP بر روی TLS                 | Windows                 |
| 52  | HTTP‑WebSocket           | 80/443                 | TCP         | WebSocket (موجب 80/443)        | Windows, Linux          |
| 53  | SIP                      | 5060                   | TCP/UDP     | VoIP (SIP)                     | Windows, Linux          |
| 54  | SIP TLS                  | 5061                   | TCP         | SIP امن                        | Windows, Linux          |
| 55  | STUN                     | 3478                   | UDP         | تعویض NAT (STUN)               | Windows, Linux          |
| 56  | TURN                     | 3478                   | UDP         | تعویض NAT (TURN)               | Windows, Linux          |
| 57  | RTP                      | 5004                   | UDP         | انتقال صوت/ویدیو               | Windows, Linux          |
| 58  | RTCP                     | 5005                   | UDP         | کنترل RTCP                     | Windows, Linux          |
| 59  | RTMP                     | 1935                   | TCP         | پخش زنده (RTMP)                | Windows, Linux          |
| 60  | H.323                    | 1720                   | TCP/UDP     | VoIP (H.323)                   | Windows, Linux          |
| 61  | IRC                      | 6667                   | TCP         | IRC (مخالف 6667)               | Windows, Linux          |
| 62  | IRC (SSL)                | 6697                   | TCP         | IRC امن                        | Windows, Linux          |
| 63  | SSH (alt)                | 2222                   | TCP         | SSH جایگزین                    | Windows, Linux          |
| 64  | DNSSEC (DNS over TLS)    | 853                    | TCP         | DNS امن                        | Windows, Linux          |
| 65  | DHCPv6 (Server)          | 547                    | UDP         | DHCPv6                         | Windows, Linux          |
| 66  | DHCPv6 (Client)          | 546                    | UDP         | DHCPv6                         | Windows, Linux          |
| 67  | mDNS                     | 5353                   | UDP         | سرویس شناسایی هم‌رده (Bonjour) | Windows, Linux          |
| 68  | SSDP                     | 1900                   | UDP         | سرویس کشف UPnP                 | Windows, Linux          |
| 69  | TFTP                     | 69                     | UDP         | انتقال فایل (TFTP)             | Windows, Linux          |
| 70  | NetBIOS (DNS)            | 137-139                | UDP/TCP     | همان NetBIOS                   | Windows, Linux          |
| 71  | UPnP                     | 1900                   | UDP         | کشف UPnP                       | Windows, Linux          |
| 72  | WS-Discovery             | 3702                   | UDP         | کشف سرویس (WS-Discovery)       | Windows, Linux          |
| 73  | KCP (RUDP)               | 4000                   | UDP         | پروتکل UDP ساده                | Windows, Linux          |
| 74  | NTRIP                    | 2101                   | TCP         | پخش GPS (NTRIP)                | Windows, Linux          |
| 75  | SFTP‑FTP                 | 990                    | TCP         | FTP امن (FTPS)                 | Windows, Linux          |
| 76  | CIFS                     | 445                    | TCP         | همان SMB                       | Windows, Linux          |
| 77  | X11                      | 6000                   | TCP         | سرور X11                       | Linux                   |
| 78  | SSH (SFTP)               | 2022                   | TCP         | SFTP جایگزین                   | Windows, Linux          |
| 79  | HTTPS‑HTTPS-ALPN         | 443                    | TCP         | TLS/SSL ALPN                   | Windows, Linux          |
| 80  | HTTP‑XMPP                | 5269                   | TCP         | XMPP (Jabber)                  | Windows, Linux          |
| 81  | MQTT‑TLS                 | 8883                   | TCP         | MQTT امن                       | Windows, Linux          |
| 82  | WebDAV                   | 8080                   | TCP         | WebDAV (HTTP)                  | Windows, Linux          |
| 83  | HTTPS‑WebDAV             | 8443                   | TCP         | WebDAV امن                     | Windows, Linux          |
| 84  | HTTPS‑Git                | 443                    | TCP         | گیت HTTPS                      | Windows, Linux          |
| 85  | SSH‑Git                  | 22                     | TCP         | گیت SSH                        | Windows, Linux          |
| 86  | Git‑HTTP                 | 9418                   | TCP         | گیت HTTP                       | Windows, Linux          |
| 87  | Zabbix                   | 10051                  | TCP         | Zabbix server                  | Windows, Linux          |
| 88  | Zabbix (agent)           | 10050                  | TCP         | Zabbix agent                   | Windows, Linux          |
| 89  | Plesk                    | 8443                   | TCP         | Plesk panel                    | Windows, Linux          |
| 90  | cPanel                   | 2082/2083/2086         | TCP         | cPanel panel                   | Linux                   |
| 91  | WHM                      | 2087                   | TCP         | WHM panel                      | Linux                   |
| 92  | Webmin                   | 10000                  | TCP         | Webmin panel                   | Linux                   |
| 93  | Docker                   | 2375 (TCP), 2376 (TLS) | TCP         | Docker daemon                  | Windows, Linux          |
| 94  | Kubernetes API           | 6443                   | TCP         | API سرور Kubernetes            | Linux                   |
| 95  | Etcd                     | 2379/2380              | TCP         | etcd cluster                   | Linux                   |
| 96  | Consul                   | 8500                   | TCP         | سرویس Consul                   | Windows, Linux          |
| 97  | Grafana                  | 3000                   | TCP         | داشبورد Grafana                | Windows, Linux          |
| 98  | Prometheus               | 9090                   | TCP         | Prometheus                     | Windows, Linux          |
| 99  | Kibana                   | 5601                   | TCP         | Kibana                         | Windows, Linux          |
| 100 | Elasticsearch            | 9200                   | TCP         | Elasticsearch                  | Windows, Linux          |
### Ports Number - Version 2 (70 Ports)

| #   | پروتکل / سرویس           | پورت پیش‌فرض | نوع پورت (TCP/UDP) | توضیح مختصر                   | سیستم‌عامل‌های رایج |
| --- | ------------------------ | ------------ | ------------------ | ----------------------------- | ------------------- |
| 1   | HTTP                     | 80           | TCP                | وب‌سرور (Apache, Nginx, IIS)  | Windows, Linux      |
| 2   | HTTPS                    | 443          | TCP                | وب‌سرور امن (TLS/SSL)         | Windows, Linux      |
| 3   | FTP                      | 21           | TCP                | فایل‌سرور (کنترل)             | Windows, Linux      |
| 4   | FTP                      | 20           | TCP                | فایل‌سرور (داده)              | Windows, Linux      |
| 5   | SFTP / SSH               | 22           | TCP                | دستورات امن، انتقال فایل      | Windows, Linux      |
| 6   | Telnet                   | 23           | TCP                | دستورات امن (نه)              | Windows, Linux      |
| 7   | SMTP                     | 25           | TCP                | ارسال ایمیل                   | Windows, Linux      |
| 8   | SMTPS                    | 465          | TCP                | ارسال ایمیل امن (SMTPS)       | Windows, Linux      |
| 9   | POP3                     | 110          | TCP                | دریافت ایمیل                  | Windows, Linux      |
| 10  | POP3S                    | 995          | TCP                | دریافت ایمیل امن              | Windows, Linux      |
| 11  | IMAP                     | 143          | TCP                | دریافت ایمیل (IMAP)           | Windows, Linux      |
| 12  | IMAPS                    | 993          | TCP                | دریافت ایمیل امن              | Windows, Linux      |
| 13  | RDP                      | 3389         | TCP                | دسکتاپ ریموت (Remote Desktop) | Windows             |
| 14  | SMB (CIFS)               | 445          | TCP                | اشتراک فایل Windows           | Windows, Linux      |
| 15  | NetBIOS Name Service     | 137          | UDP                | نام‌گذاری NetBIOS             | Windows, Linux      |
| 16  | NetBIOS Datagram Service | 138          | UDP                | پیکسل‌های NetBIOS             | Windows, Linux      |
| 17  | NetBIOS Session Service  | 139          | TCP                | نشست‌های NetBIOS              | Windows, Linux      |
| 18  | LDAP                     | 389          | TCP/UDP            | سرویس جست‌وجوی دایرکتوری      | Windows, Linux      |
| 19  | LDAPS                    | 636          | TCP                | LDAP امن (TLS)                | Windows, Linux      |
| 20  | DNS                      | 53           | TCP/UDP            | سرویس نام‌گذاری دامنه         | Windows, Linux      |
| 21  | DHCP (Client)            | 68           | UDP                | درخواست DHCP                  | Windows, Linux      |
| 22  | DHCP (Server)            | 67           | UDP                | پاسخ DHCP                     | Windows, Linux      |
| 23  | NTP                      | 123          | UDP                | همگام‌سازی ساعت               | Windows, Linux      |
| 24  | SNMP                     | 161          | UDP                | مانیتورینگ شبکه               | Windows, Linux      |
| 25  | SNMPTRAP                 | 162          | UDP                | اعلان خطا                     | Windows, Linux      |
| 26  | RADIUS                   | 1812         | UDP                | احراز هویت RADIUS             | Windows, Linux      |
| 27  | TACACS+                  | 49           | TCP                | احراز هویت TACACS+            | Windows, Linux      |
| 28  | MSSQL                    | 1433         | TCP                | پایگاه‌داده Microsoft SQL     | Windows, Linux      |
| 29  | MySQL                    | 3306         | TCP                | پایگاه‌داده MySQL             | Windows, Linux      |
| 30  | PostgreSQL               | 5432         | TCP                | پایگاه‌داده PostgreSQL        | Windows, Linux      |
| 31  | Oracle (Listener)        | 1521         | TCP                | پایگاه‌داده Oracle            | Windows, Linux      |
| 32  | MongoDB                  | 27017        | TCP                | پایگاه‌داده MongoDB           | Windows, Linux      |
| 33  | Redis                    | 6379         | TCP                | حافظهٔ کی-وی (Redis)          | Windows, Linux      |
| 34  | Memcached                | 11211        | TCP                | حافظهٔ کی-وی (Memcached)      | Windows, Linux      |
| 35  | MQTT                     | 1883         | TCP                | پروتکل پیام‌گذاری (MQTT)      | Windows, Linux      |
| 36  | MQTTs                    | 8883         | TCP                | MQTT امن                      | Windows, Linux      |
| 37  | XMPP                     | 5222         | TCP                | پیام‌رسانی (Jabber)           | Windows, Linux      |
| 38  | JMX (Java)               | 1099         | TCP                | مدیریت جاوا                   | Windows, Linux      |
| 39  | WinRM                    | 5985         | TCP                | مدیریت Windows Remote         | Windows             |
| 40  | WinRM (HTTPS)            | 5986         | TCP                | مدیریت امن                    | Windows             |
| 41  | SSH Tunneling            | 22           | TCP                | همان SFTP/SSH                 | Windows, Linux      |
| 42  | NFS                      | 2049         | TCP/UDP            | اشتراک فایل UNIX              | Windows, Linux      |
| 43  | SMB (SMB1)               | 139          | TCP                | همان SMB                      | Windows, Linux      |
| 44  | HTTP‑Alt                 | 8080         | TCP                | وب‌سرور‌های جایگزین           | Windows, Linux      |
| 45  | HTTPS‑Alt                | 8443         | TCP                | وب‌سرور امن جایگزین           | Windows, Linux      |
| 46  | HTTPS‑Caddy              | 2015         | TCP                | وب‌سرور Caddy                 | Linux               |
| 47  | HTTPS‑Nginx              | 8443         | TCP                | جایگزین Nginx                 | Linux               |
| 48  | HTTPS‑Tomcat             | 8443         | TCP                | جایگزین Tomcat                | Windows, Linux      |
| 49  | HTTPS‑Shadowsocks        | 8388         | TCP                | Shadowsocks                   | Linux               |
| 50  | HTTPS‑OpenVPN            | 1194         | UDP                | VPN                           | Windows, Linux      |
| 51  | HTTPS‑OpenVPN (TCP)      | 443          | TCP                | VPN با TCP                    | Windows, Linux      |
| 52  | HTTPS‑WireGuard          | 51820        | UDP                | VPN WireGuard                 | Linux               |
| 53  | HTTPS‑WireGuard (TCP)    | 51820        | TCP                | WireGuard با TCP              | Linux               |
| 54  | HTTPS‑IIS (HTTP/2)       | 80           | TCP                | IIS HTTP/2                    | Windows             |
| 55  | HTTPS‑IIS (HTTPS/2)      | 443          | TCP                | IIS HTTPS/2                   | Windows             |
| 56  | HTTPS‑Apache (HTTP/2)    | 80           | TCP                | Apache HTTP/2                 | Linux               |
| 57  | HTTPS‑Apache (HTTPS/2)   | 443          | TCP                | Apache HTTPS/2                | Linux               |
| 58  | HTTPS‑Caddy (HTTP/3)     | 443          | UDP                | Caddy HTTP/3                  | Linux               |
| 59  | HTTPS‑Nginx (HTTP/3)     | 443          | UDP                | Nginx HTTP/3                  | Linux               |
| 60  | HTTPS‑Shadowsocks (TLS)  | 8388         | TCP                | Shadowsocks TLS               | Linux               |
| 61  | HTTPS‑Zimbra             | 443          | TCP                | Zimbra Collaboration          | Linux               |
| 62  | HTTPS‑Zimbra             | 80           | TCP                | Zimbra HTTP                   | Linux               |
| 63  | HTTPS‑Exchange           | 443          | TCP                | Microsoft Exchange            | Windows             |
| 64  | HTTPS‑Exchange           | 80           | TCP                | Microsoft Exchange HTTP       | Windows             |
| 65  | HTTPS‑SAP                | 8000         | TCP                | SAP GUI (HTTP)                | Windows, Linux      |
| 66  | HTTPS‑SAP                | 443          | TCP                | SAP GUI (HTTPS)               | Windows, Linux      |
| 67  | HTTPS‑Samba              | 139          | TCP                | اشتراک فایل (Samba)           | Linux               |
| 68  | HTTPS‑Samba              | 445          | TCP                | اشتراک فایل (SMB)             | Linux               |
| 69  | HTTPS‑Kerberos           | 88           | TCP/UDP            | Kerberos                      | Windows, Linux      |
| 70  | HTTPS‑Kerberos           | 464          | TCP/UDP            | Kerberos تغییر رمز            | Windows, Linux      |