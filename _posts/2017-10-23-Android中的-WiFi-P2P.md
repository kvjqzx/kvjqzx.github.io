---
layout: post
title: Android 中的 Wi-Fi P2P
categories : Android
tags : [Android, WiFi, P2P, Direct, WPS]
---
{% include JB/setup %}

## 概述

Wi-Fi P2P 是Wi-Fi联盟推出的，用于多个Wi-Fi设备在没有AP的情况下直接连接的技术，主要用于无线投屏，将手机上的图片或视频传给电视去显示。通过认证的设备会标有Wi-Fi Direct商标。

## 架构

### P2P 组件
* P2P Device GO和GC的统称
* P2P Group Owner
* P2P Client

### 拓扑结构
* 仅P2P模式    
有且仅有一个扮演GO，其它设备扮演GC，周围不支持P2P的STA(Legacy Client)也可以发现并关联上GO。
![P2P mode]({{ BASE_PATH }}/assets/pics/p2p_topology.png)

* P2P和STA并存模式    
并发操作需要设备支持多MAC 实体。P2P Group和WLAN BSS可以处于相同的信道，也可以不同。
![P2P concurrent mode]({{ BASE_PATH }}/assets/pics/p2p_topology_concurrent.png)

## P2P 特殊的功能和服务
* P2P Discovery 让P2P设备相互发现并建立连接。
* P2P Group Operation GO如何管理一个 Group
* P2P Power Management 提供一系列的功能来减少设备电量消耗
* Managed P2P Device Operation (optional) 在企业环境里IT部门如何管理P2P设备

### P2P Discovery
让P2P设备能够快速发现对方并建立连接。    
包含以下4个主要部分：
* Device Discovery 帮助两个设备处于一个公用信道并交换设备信息
* Service Discovery (optional) 允许一个P2P设备在建立连接前发现可用的上层服务
* Group Formation 用于决定哪个设备成为GO 并建立新的P2P Group
* P2P Invitation 邀请P2P设备加入一个已经存在的 Group

## 相关协议

* [Wi-Fi Peer-to-Peer Technical Specification v1.7]({{ BASE_PATH }}/assets/docs/Wi-Fi_P2P_Technical_Specification_v1.7.pdf)


