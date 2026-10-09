---
Episode: 0-Topics Should be Write in Book
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
### Reference - مرجع ها - Cheat Sheets
1. All Ports Numbers for Protocols - Cheat Sheets
	1. لیست کاملی از پروتکل ها و شماره پورت آنها
### Prerequisite 
1. All Protocols in Network
	1. IP (Internet Protocol)
	2. ARP (Address Resolution Protocol)
	3. DHCP (Dynamic Host Configuration Protocol)
	4. DNS (Domain Name Server)
	5. NDP(Neighbor Discover Protocol)
	6. STP (Spanning Tree Protocol)
	7. MSTP 
	8. Other STP Families
	9. RSTP (Rapid Spanning Tree Protocol)
	10. CDP (Cisco Discovery Protocol)
	11. DTP (Dynamic Trunking Protocol)
	12. VTP (VLAN Trunking Protocol)
	13. HSRP (Hot Standby Router Protocol)
	14. ISL (Inter Switch Link - VLAN Tagging)
	15. MPLS (Multi Protocol Label Switching)
	16. IEEE 802.1Q (VLAN Tagging)
	17. IEEE 802.1X (Port‑Based Network Access Control)
	18. TLS
	19. SSL
	20. Proxy Protocols(SOCKS, ....)
	21. NFS (Network File System)
2. Terminology
	1. Network
	2. Machine - Host
	3. Server
	4. MAC Address
	5. IP Address
	6. Router
	7. Switch
	8. Hub
	9. Access Points
	10. Firewall
	11. Protocol
	12. Port
	13. Topology
	14. Gateway, Subnet Mask, IP, DNS
	15. Penetration Tester -  Pentester(تست نفوذگر)
	16. Attacker
	17. Spoof
	18. Sniff
	19. Defensive Security
	20. Offensive Security
	21. IP Forwarding
	22. IDS
	23. IPS
	24. Passive Attacks
	25. Active Attacks
	26. VPS
	27. VPN
3. Concepts
	1. IP Forwarding
	2. Key Logger
	3. OSI
	4. Docker
	5. CA Certificates
	6. Handshakes(TLS Handshake, WPA Handshake, Three Way Handshake, Four Way Handshake)
	7. Proxy
4. Necessary Tools
	1. WSL
	2. winget
	3. scoop
	4. choco
	5. Docker

### Attack Tools
1. Add Installation Method for All Tools
2. Reminded Topics
	1. Ettercap
		1. Write All Ettercap Chapter based on
			1. All Settings: https://manpages.debian.org/testing/ettercap-common/ettercap.8.en.html
		2. Installation Methods
		3. `Text mode`, `Curses`, `GTK` Modes
		4. Reminded Plug-ins(gre_relay, krb5_downgrade, nbns_spoof, mdns_spoof, rand_flood, smurf_attack, stp_mangler,  ...)
	2. Bettercap
		1. Installation Methods
		2. Reminded Modules(ui, c2, dos_attack, ...)
		3. Bettercap GUI Mode
		4. Web Interfaces for Bettercap
	3. Yersinia
		1. Check Attacks Based on https://manpages.debian.org/testing/yersinia/yersinia.8.en.html
	4. 
3. MITM
	1. [x] mitmproxy ✅ 2026-05-12
	2. sslsniff
	3. [x] Dsniff ✅ 2026-05-12
		1. SSH MITM(in dsniff)
		2. arpspoof
	4. PETEP (`PEnetration TEsting Proxy`)
	5. HTTP Toolkit
	6. Xerosploit
	7. [x] Yersinia ✅ 2026-05-12
	8. Wireshark
	9. Burp Suite
	10. Kismet
	11. tcpdump
	12. BeEF (Post Exploitation after MITM Attacks)
		1. ابزار BeEF (Browser Exploitation Framework) در دسته‌بندی کلی حملات شبکه، در گروه Post-Exploitation (پس از نفوذ) و به طور تخصصی‌تر در زیرمجموعه Client-Side Attacks (حملات سمت کاربر) قرار می‌گیرد.
		2. اگرچه BeEF به خودی خود یک ابزار مستقیم برای اجرای حمله MITM نیست، اما یکی از قدرتمندترین ابزارها برای بهره‌برداری از نتیجه یک حمله MITM محسوب می‌شود. رابطه آن با MITM به این صورت است:
	13. Other Tools
		1. SSLstrip / SSLstrip+
		2. dnschef-ng (جعل DNS)
		3. proxy.py (پروکسی قدرتمند)
		4. Evilginx (فیشینگ پیشرفته)
		5. 
	14. WiFi Pineapple (Hardware)
4. **All Tools Comparing**
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
7. Port Stealing (Stolen Switch Ports)
8. Neighbor Discovery Attack
9. MAC Attacks:
	1. MAC Flooding
	2. MAC Spoofing
10. Layer 2 Switching Attacks:
	1. BPDU - STP Attacks
		1. Configuration BPDU Sending (DoS)
		2. tcn BPDU Sending (DoS)
11. DoS:
	1. Slow Read DoS
### Pictures
1. Yersinia - Yersinia GUI(Graphic User Interface) Mode
2. Bettercap - Work with Bettercap GUI
3. MITMf
4. mitmproxy
	1. https://virgool.io/@NoneAuthor/%D8%A7%D8%A8%D8%B2%D8%A7%D8%B1-mitmproxy-%DA%86%DB%8C%D8%B3%D8%AA-qcudlv4eb4bj
5. 
### Extensions
1. Bettercap
	1. Caplets https://github.com/bettercap/caplets
	2. Automatic Sniffing Script https://github.com/90N45-d3v/BetterSniff
	3. Bettercap UI - https://github.com/bettercap/ui
	4. Better Vision - UI for Bettercap: Test before USE: https://github.com/Vallsp/bettercap-vision
2. MITMf Configuration Files
	1. https://github.com/byt3bl33d3r/MITMf/tree/master/config/app_cache_poison_templates
	2. https://github.com/byt3bl33d3r/MITMf/tree/master/config/captive
	3. https://github.com/byt3bl33d3r/MITMf/tree/master/config/hta_driveby
	4. https://github.com/byt3bl33d3r/MITMf/tree/master/config/responder
	5. https://github.com/byt3bl33d3r/MITMf/blob/master/config/mitmf.conf
### Sources - References I Used for this Book
1. GitHub Repository of Tools
2. Docker Docs
3. https://manpages.debian.org/
4. Medium Articles:
	1. https://medium.com/@sagapatil/man-in-the-middle-mitm-attacks-a-practical-guide-with-ettercap-bettercap-more-082f0fd69ff4
5. GitHub User Contents:
	1. https://raw.githubusercontent.com/mitmproxy
6. https://deepwiki.com/
7. https://docs.mitmproxy.org/
8. https://security.stackexchange.com/
9. https://www.programmersought.com/
10. http://git.panaceas.org/
11. https://openkoji.iscas.ac.cn/koji/
12. https://developer.aliyun.com/
13. https://www.linuxlinks.com
14. https://packages.trisquel.info/
15. https://zhuanlan.zhihu.com
16. https://vcdx200.uw.cz
17. https://ipfs.io/
18. https://unixlinux.tmit.bme.hu
19. https://man.cx/
20. https://debian-facile.org/
21. https://man.archlinux.org/
22. https://www.yisu.com/
23. https://linuxcommandlibrary.com/
24. https://libraries.io/
25. https://app4.secure.forcepoint.com/
26. https://lists.freedesktop.org/
27. http://archives.archlinux32.org/
28. https://pypi.org/
29. https://www.monkey.org/~dugsong/dsniff/
30. https://www.nsrc.org
31. https://unixlinux.tmit.bme.hu
32. https://blog.csdn.net
33. https://netadmin.com.tw
34. https://wikipedia.org
### Web Attacks
