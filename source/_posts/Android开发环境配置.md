---
title: Android开发环境配置
date: 2019-08-09 23:02:20
tags:
- Android
categories:
- 移动开发
---
# 前言
&emsp;&emsp;本文为Android开发环境的配置全记录，亲测可用，参考文章链接：[Android Studio 和 SDK 下载、安装和环境变量配置](https://blog.csdn.net/siwuxie095/article/details/53431818),作者：[siwuxie095](https://blog.csdn.net/siwuxie095/article/details/53431818).
<!-- more -->

# 配置过程
环境：Win10 x64

配置Android环境需要这些工具：
1. JDK (Java SE)
2. Android Studio IDE
3. Android SDK packages

## JDK的安装与配置
安装JDK和配置环境变量本文就不赘述了，请自行Google

## Android Studio的安装
前往[Android studio官网](https://developer.android.com/studio)下载最新的安装包，本文所用的环境为`3.4.2 for Windows 64-bit`，下载完毕后双击安装![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190809234713.png)
Android Virtual Device是运行Android虚拟机用的，如果你有Android设备的话这个可选可不选（据说速度不太行，但我用的还可以），但是没有Android设备的话那就必须安装了。
p.s:使用虚拟机可能需要去主板里开启VirtualMachine的设置。
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190809235325.png)
选择安装路径，条件允许的话默认路径就可以，接下来一路next后install即可（现在的安装程序流程已经极为简化了）

## Android SDK的安装
安装好Android Studio后点击运行
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810000455.png)
此处因为Android studio本身是不带SDK的，所以在没有安装SDK的情况下它是检测不到的，此次点击cancel即可，稍后会下载。
接下来进入配置界面，一路next
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810000949.png)
点击finish，开始安装Android SDK
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810002035.png)
安装完毕后点击Finish，此处可以进入SDK Manager管理安装的SDK和下载其他内容。

## 配置Android SDK环境变量
右键`我的电脑`,点击属性，选择`高级系统设置`-`环境变量`,然后在下方的环境变量框中点击`新建`
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810002624.png)
如图，给变量起个名字，然后找到sdk安装的路径，默认安装到`C:\Users\你的用户名\AppData\Local\Android\Sdk`（记得打开显示隐藏的项目，因为AppData是默认隐藏的）,完毕后点击确定
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810002840.png)
点击`path`-`编辑`-`新建`，新建两条路径：
+ %ANDROID_HOME%\tools
+ %ANDROID_HOME%\platform-tools
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810002938.png)
完毕后打开cmd，分别输入adb和android
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810003108.png)
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190810003130.png)
配置成功！