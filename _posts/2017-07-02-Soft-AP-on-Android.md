---
layout: post
title: SoftAP on Android
categories : Android
tags : [Android, WiFi, SoftAP]
---
{% include JB/setup %}

SoftAP相关的APIs
---

提供给应用层的APIs都在WifiManager中，但是它们都是hidden的，仅提供给系统调用，详细信息如下:    
* public int getWifiApState()    

```
The lookup key for an int that indicates whether Wi-Fi AP is enabled,
disabled, enabling, disabling, or failed.

WIFI_AP_STATE_DISABLED
WIFI_AP_STATE_DISABLING
WIFI_AP_STATE_ENABLED
WIFI_AP_STATE_ENABLING
WIFI_AP_STATE_FAILED
```

* public boolean isWifiApEnabled()
* public boolean setWifiApEnabled(WifiConfiguration wifiConfig, boolean enabled)
* public WifiConfiguration getWifiApConfiguration()

对于Framework层来说，SoftAP配置的读取和写入操作都是对下面的文件。    
里面主要包含SSID、安全性、密码、频段和信道。    

```
/data/misc/wifi/softap.conf
```

WifiApConfigStore 在WifiStateMachine的InitialState中进行实例化，此时会从上面的文件中读取SoftAP配置进行初始化，
如果从文件中读出来的为空，则用默认配置进行初始化，同时将默认配置写入该文件。

SoftAP开启和关闭流程分析
---

WifiManager#setWifiApEnabled()    
WifiServiceImpl 发送消息`CMD_SET_AP`给WifiController    
WifiStateMachine#setHostApRunning()，在该方法里面会判断开启还是关闭SoftAP，开启发送`CMD_START_AP`，关闭发送`CMD_STOP_AP`消息。    
开启调用SoftApManager#start()，会发送`CMD_START`给SoftApStateMachine，然后调用startSoftAp()    
关闭调用SoftApManageri#stop()，会发送`CMD_STOP`给SoftApStateMachine，然后调用stopSoftAp()    
通过NetworkManagementService调用startAccessPoint()/stopAccessPoint()    

SoftAP 架构图
---

![SoftAP architecture]({{ BASE_PATH }}/assets/pics/softap_architecture.png)


