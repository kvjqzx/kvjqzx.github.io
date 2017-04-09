---
layout: post
category : Tool
tags : [Android, APK, decompile, apktool, jd-gui, dex2jar]
---
{% include JB/setup %}

APK反编译工具
-----
* [Apktool](https://ibotpeaches.github.io/Apktool/) A tool for reverse engineering 3rd party, closed, binary Android apps. It can decode resources to nearly original form and rebuild them after making some modifications.
* [dex2jar](https://github.com/pxb1988/dex2jar) Tools to work with android .dex and java .class files
* [JD-GUI](http://jd.benow.ca) is a standalone graphical utility that displays Java source codes of “.class” files. You can browse the reconstructed source code with the JD-GUI for instant access to methods and fields.

macOS工具安装
-----
* Apktool
下载脚本和jar文件，分别重命名为apktool和apktool.jar，放到`/usr/local/bin`目录下，并增加可执行权限。

使用：
`apktool d name.apk`  反编译
`apktool b name` 将改完后的文件重现打包，生成的apk在name文件夹下的dist目录
* dex2jar
需要将d2j_invoke.sh和d2j-dex2jar.sh增加可执行权限
`d2j-dexjar.sh name.apk`  将apk转化为jar文件，将该jar文件拖到JD-GUI里面，即可看到源码，通过源码对比smali文件，可以修改smali文件重新打包。

macOS上工具下载链接
-----
* [apktool script]({{ BASE_PATH }}/assets/tools/apktool)
* [apktool_2.2.2.jar]({{ BASE_PATH }}/assets/tools/apktool_2.2.2.jar)
* [dex2jar-2.0]({{ BASE_PATH }}/assets/tools/apktool/dex2jar-2.0.zip)
* [jd-gui-osx-1.4.0.tar]({{ BASE_PATH }}/assets/tools/apktool/jd-gui-osx-1.4.0.tar)
