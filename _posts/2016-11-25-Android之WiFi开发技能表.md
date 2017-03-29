---
layout: post
category : lessons
tags : [Android, WiFi, Wireshark, tcpdump, Fiddler, Aircrack, HAL, supplicant]
---
{% include JB/setup %}

Android中WiFi相关代码位置
-----
* Settings
  * [platform/packages/apps/settings](http://androidxref.com/7.0.0_r1/xref/packages/apps/Settings/)
* CertInstaller
  * [platform/packages/apps/certinstaller](http://androidxref.com/7.0.0_r1/xref/packages/apps/CertInstaller/)
* Frameworks
  * [platform/frameworks/opt/net/wifi](http://androidxref.com/7.0.0_r1/xref/frameworks/opt/net/wifi/)
  * [platform/frameworks/base](http://androidxref.com/7.0.0_r1/xref/frameworks/base/wifi/)
* HAL
  * [platform/hardware/libhardware_legacy](http://androidxref.com/7.0.0_r1/xref/hardware/libhardware_legacy/wifi/)
* wpa_supplicant
  * [platform/external/wpa_supplicant_8](http://androidxref.com/7.0.0_r1/xref/external/wpa_supplicant_8/)

分析工具
-----
* [Wireshark](https://www.wireshark.org)
  网络协议分析工具
* [tcpdump](http://www.tcpdump.org)
  命令行形式的包分析工具，一般用该工具抓取报文，用Wireshark进行分析
* [Fiddler](http://www.telerik.com/fiddler)
  针对 HTTP/HTTPS 抓包以及协议分析工具
* [Aircrack-ng](http://www.aircrack-ng.org/)
  Aircrack-ng is a complete suite of tools to assess WiFi network security.

相关命令
-----
* [iw](http://linuxwireless.org/en/users/Documentation/iw/)
A new nl80211 based CLI configuration utility for wireless devices. It supports all new drivers that have been added to the kernel recently. The old tool iwconfing, which uses Wireless Extensions interface, is deprecated and it's strongly recommended to switch to iw and nl80211.

参考书
-----
* [802.11 Wireless Networks-The definitive Guide]({{ BASE_PATH }}/assets/docs/802.11_Wireless_Networks.pdf)
