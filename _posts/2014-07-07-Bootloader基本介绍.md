---
layout: post
category : lessons
tagline: "Supporting tagline"
tags : [Bootloader]
---
{% include JB/setup %}

##Bootloader是什么?
系统上电后，需要一段程序来进行初始化，如果它能将操作系统内核复制到内存中运行，无论从本地还是从远端，就称这段程序为Bootloader。

Bootloader就是这么一段程序，它在系统上电时开始执行，初始化硬件设备、建立内存空间的映射图，从而将系统的软硬件带到一个合适的状态，以便为最终调用操作系统内核准备好环境。

在嵌入式系统中，CPU上电后，会从某个地址开始执行，比如MIPS架构的CPU会从**0xBFC0000**取第一条指令，而ARM结构的CPU则从地址**0x000000**开始。嵌入式开发板中，需要把存储器件ROM或Flash等映射到这个地址，Bootloader就存放在这个地址开始处，这样一上电就可以执行。

通常，Bootloader是严重依赖于硬件而实现的，特别是在嵌入式世界。因此，在嵌入式世界里建立一个通用的Bootloader几乎是不可能的。

##嵌入式Linux系统的软件层次：

* 引导加载程序
包括固化在固件中的boot代码和Bootloader两大部分。
有些CPU在运行Bootloader之前先运行一段固化程序，比如x86结构的CPU就是先运行BIOS中的固件，然后才运行硬盘第一个分区（MBR）中的Bootloader。在大多数嵌入式系统中并没有固件，Bootloader是上电后执行的第一个程序。
* Linux内核
* 文件系统 包括根文件系统和建立于Flash内存设备之上的文件系统。
* 用户应用程序 特定于用户的应用程序，它们也存储在文件系统中。

##Bootloader的操作模式
大多数Bootloader都包含两种不同的操作模式：启动加载模式和下载模式。这种区别仅对开发人员才有意义。从最终用户的角度看，bootloader就是用来加载系统的，不存在这两种模式。

* 启动加载模式（Boot loading）
Bootloader的正常工作模式，直接从本机的某个固态存储设备上将系统加载到RAM中运行。嵌入式产品发布的时候都是以该种模式。

* 下载模式（downloading）
目标机器上的bootloader将通过串口或网络连接等通讯手段从主机下载文件。通常是下载内核镜像，先下载到RAM中，然后写入flash类固态存储设备。这种模式通常在第一次安装内核和根文件系统时或系统升级时使用。
工作在这种模式下的bootloader通常会向它的终端用户提供一个简单的命令行接口。

像Blob和U-Boot等这种功能强大的Bootloader，通常会同时支持这两种模式。比如U-Boot在正常的启动加载模式启动时，都会延时数秒钟时间，等待终端用户按下任意键切换到下载模式，如果用户没有按，则继续启动Linux内核。

##Bootloader的启动过程
Bootloader的启动过程可分为单阶段（Single Stage）、多阶段（Multi Stage）两种。

通常多阶段的Bootloader能提供更为复杂的功能以及更好的可移植性。从固态设备上启动的Bootloader大多都是两阶段的启动过程。

第一阶段用汇编实现，完成一些依赖CPU体系结构的初始化，并调用第二阶段的代码。
第二阶段通常用C语言实现，这样可以实现更复杂的功能。

Bootloader第一阶段功能：

* 硬件设备初始化
* 为加载Bootloader的第二阶段代码准备RAM空间
* 复制Bootloader的第二阶段的代码到RAM中
* 设置好栈
* 跳转到第二阶段代码的C入口点

Bootloader第二阶段

* 初始化本阶段要用到的硬件设备
* 检测系统内存映射
* 将内核映像和根文件系统映像从Flash上读到RAM空间
* 为内核设置启动参数
* 调用内核

###硬件设备初始化
* 屏蔽所有中断
> 为中断提供服务通常是操作系统设备驱动的责任，因此在Bootloader的执行过程中不必响应任何中断。中断屏蔽可以通过写CPU中断屏蔽寄存器或状态寄存器来完成。

* 设置CPU的速率和时钟频率
* RAM初始化
> 正确设置系统内存控制器的功能寄存器以及各内存库控制寄存器等。
* 初始化LED
> 典型地，通过GPIO来驱动LED，来表明系统的状态；如果板子上没有LED，可以通过初始化UART向串口打印信息来完成这一点。

### 为加载stage2准备RAM空间
为了获得更快的执行速度，通常把stage2加载到RAM空间中来执行，因此必须为stage2准备好一段可用的RAM空间。

##常见的Bootloader
* [U-Boot](ftp://ftp.denx.de/pub/u-boot/)
> 开源、支持多个处理器系列，在嵌入式开发比较常用。
* Redboot
> 开源、是Redhat公司随eCos发布的一个BOOT方案。
* vivi
> 韩国mizi公司开发的bootloader, 适用于ARM9处理器。
