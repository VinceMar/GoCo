---
title: 电信校园网NetKeeper错误118解决办法
date: 2019-07-30 00:57:32
tags:
- NetKeeper
categories:
- 实用技巧
---

# 前言
&emsp;&emsp;前几天从某东上买好了SSD和内存条，打算把我的主力战机升级下硬件，顺便重装个系统再重新分个区，涅槃重生一下。在老师的帮助下顺利安装上了Win10 2019年May的专业版，并且通过魔法将其永久激活了(请支持正版)。OK，在宿舍连上电信的NetKeeper,装完了常用的软件和开发环境,一重启突然发现右下角的网络连接不见了，登录NetKeeper提示118错误，这是为什么？刚才还好好的怎么突然就没网了？
<!-- more -->
# 解决方法
&emsp;&emsp;通过检查网络选项，发现虽然不显示网络连接，但是也是能正常连接WiFi的,说明无线网卡的驱动和硬件都没问题，难道是操作系统的问题？  
&emsp;&emsp;在重装了系统之后，一切正常，但是重启后仍然是没有右下角的网络连接，而且NetKeep依旧报错118。没办法，用手机查了一下，发现原来是NetKeeper有bug，不兼容Windows 1809版本及以上系统,具体来说可能是由于重启后会导致eventlog注册表丢失，使得网络服务无法正常启动。下面是CSDN博主`绿荫下的陪伴`提供的解决方案，感谢。(原文链接：[创翼错误118 pppoe拨号模块损坏](https://blog.csdn.net/qq_43251098/article/details/89419261))  

1. 新建一个txt文件
2. 复制下面代码到.txt文件里并保存
```
Windows Registry Editor Version 5.00


[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\EventLog]
"ServiceDll"=hex(2):25,00,53,00,79,00,73,00,74,00,65,00,6d,00,52,00,6f,00,6f,\
00,74,00,25,00,5c,00,53,00,79,00,73,00,74,00,65,00,6d,00,33,00,32,00,5c,00,\
77,00,65,00,76,00,74,00,73,00,76,00,63,00,2e,00,64,00,6c,00,6c,00,00,00
```
3. 将txt文件后缀改为reg
4. 双击运行该reg文件
5. 重启

OK，问题解决

# 总结
&emsp;&emsp;电信的校园网是我近几年来用过的最恶心的产品了，连接不稳定总掉线、网速时常龟速等等就先不提了，它最厉害的是只提供了最核心的两机一号的拨号上网服务，你的所有设备(树莓派、NAS等)都必须通过走一遍拨号的流程才能够正常上网(也有方法破解，但是我还没找到)。现在又有了这么一个恶性Bug，电信真的是在不遗余力的恶心学生，感觉电信的校园网服务除了便宜以外毫无优点。