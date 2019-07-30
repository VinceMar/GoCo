---
title: Hexo博客操作手册
date: 2019-07-16 01:32:06
tags:
- Hexo
categories:
- 实用技巧
---
# 前言
&emsp;&emsp;Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。本贴用于记录各种关于Hexo博客的操作，欢迎收藏。
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190730153403.png"/>
<!-- more -->
# 给Hexo博客绑定域名
原文链接：[给Hexo搭建的博客绑定域名](https://juejin.im/post/5c0380a86fb9a049ff4ddd74)
## 第一步
首先你得你得搭建 XXX.github.io 这样的博客。
## 第二步
你的博客可以访问后，去阿里云、腾讯云等网站去注册个域名。

## 第三步
获取博客的ip地址

Tips: 这一步主要为了解析域名，不获取IP的话，step4解析A类型就行

第一： clone 你创建的 仓库 xxx.github.io

第二： git的输入 ping www.xxx.github.io 得到IP 如下图：
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716013540.png)

## 第四步
等域名审核完后，和我相关的就来了。以下用阿里云注册的域名为例。 进入阿里云的管理控制台-域名与网站-云解析DNS，进入域名的解析设置，点击新手指导，将得到的 IP 地址填到记录值一栏，点击确定就 OK 了。填完以后的解析列表会出现：
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716013610.png)  
记录值就是自己 github 的二级域名的 IP地址。
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716013716.png)
Tips: 一般解析都需要点时间，等个20分钟左右就好了
## 第五步
在hexo项目下 source 文件夹下创建CNAME 文件（没有后缀名的），在里面写上购买的域名，例如：
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716013756.png)
## 第六步
最后到你 xxx.github.io 的Settings里，填上你的域名
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716013820.png)
这样新域名就ok啦，可以访问了。