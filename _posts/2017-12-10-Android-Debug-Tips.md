---
layout: post
title: Android Debug Tips
categories : Android
tags : [Android, WiFi, Debug]
---
{% include JB/setup %}

### 国家码
高通平台    
```
Updating regulatory domain eg SWEDEN
$iw reg set SE

Query current regulatory domain
$iw reg get
```

博通平台    
```
adb shell wl country
$ adb shell wl country DE   设置国家码
$ adb shell wl country      查看国家码
DE (DE/0) GERMANY
```


