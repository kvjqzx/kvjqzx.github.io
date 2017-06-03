---
layout: post
categories : Android
tags : [Android, tutorial, netd]
---
{% include JB/setup %}

Netd基本概念
---
Netd 是Network Deamon的缩写，即Network守护进程。专门负责网络管理和控制的后台程序。
Netd位于Framework和Kernel之间，是Android系统中网络相关消息和命令转发及处理的中枢模块。  
* Netd接收并处理来自Framework层中NetworkManagementService或NsdService的命令。这些命令最终由Netd中对应的Command对象去处理。
* Netd接收并解析来自Kernel的UEvent消息，然后再转发给Framework层中对应的Service去处理。

Netd 启动
---
Netd 进程由init进程根据init.rc的对应配置项进行启动。
/system/netd/server/netd.rc

```
service netd /system/bin/netd
    class main
    socket netd stream 0660 root system
    socket dnsproxyd stream 0660 root inet
    socket mdns stream 0660 root system
    socket fwmarkd stream 0660 root inet
```
