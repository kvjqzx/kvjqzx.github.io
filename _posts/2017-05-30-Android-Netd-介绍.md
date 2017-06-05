---
layout: post
categories : Android
tags : [Android, tutorial, netd]
---
{% include JB/setup %}

Netd 基本概念
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

Netd 如何接收UEvent消息
---
主要是通过**NetlinkManager**， 它接收并处理来自Kernel的UEvent消息。    
这些消息经NetlinkManager解析后，通过它的广播**CommandListener**发送给Framework层的NetworkManagementService。  

在NetlinkManager里面定义了以下几个socket用于接收不同的UEvent事件。

```
int mUeventSock; // NETLINK_KOBJECT_UEVENT  NETLINK_FORMAT_ASCII
int mRouteSock;  // NETLINK_ROUTE  NETLINK_FORMAT_BINARY
int mQuotaSock;  // NETLINK_NFLOG  NETLINK_FORMAT_BINARY
int mStrictSock; // NETLINK_NETFILTER  NETLINK_FORMAT_BINARY_UNICAST
```

* NETLINK_KOBJECT_UEVENT   
  代表kobject事件，一般用来通知内核中某个模块的加载或卸载
* NETLINK_ROUTE  
  代表routing或link改变时对应的消息
* NETLINK_NFLOG  
  带宽控制相关，网络数据超过一定字节后kernel发送相关警告
* NETLINK_NETFILTER  
  Offer to detect non-SSL/TLS network traffic

在创建socket的时候，将创建上面的4种类型。
```
if ((*sock = socket(PF_NETLINK, SOCK_DGRAM | SOCK_CLOEXEC, netlinkFamily)) < 0)
```

当socket创建好了之后，将传给NetlinkHandler，用于解析消息。
```
NetlinkHandler *handler = new NetlinkHandler(this, *sock, format);
if (handler->start()) {
    ALOGE("Unable to start NetlinkHandler: %s", strerror(errno));
    close(*sock);
    return NULL;
}
```
定义的几个Handler如下
```
NetlinkHandler *mUeventHandler;
NetlinkHandler *mRouteHandler;
NetlinkHandler *mQuotaHandler;
NetlinkHandler *mStrictHandler;
```
每个NetlinkHandler对象均会单独创建一个线程用于接收socket消息，当Kernel发送UEvent后，   
NetlinkHandler便从select返回，然后调用onDataAvailable函数，该函数内部会创建一个NetlinkEvent对象。   
NetlinkEvent对象根据socket创建时候指定的解析类型去解析来自Kernel的UEvent消息。    
最终NetlinkHandler的onEvent被调用，各种UEvent消息在该函数内分类处理。   
NetlinkHandler最终将处理结果通过NetworkManager中定义的广播发给NetworkManagementService。
```
SocketListener *mBroadcaster;
```

Netd 如何将消息发送给Framework层
---

Netd 如何接收Framework层的消息
---
