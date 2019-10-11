---
title: VSCode设置同步插件-Setting Sync 2019配置教程
date: 2019-10-11 19:30:33
tags:
- VSCode插件
categories:
- 实用技巧
---
# 前言
VSCode是我个人认为最好用的编辑器，你可以使用插件将你的VSCode打造成属于你自己的一把利刃。但随着插件、配置项的增多，更换机器后重新配置一遍VSCode着实是一项大工程，本文来介绍一下2019版的VSCode配置同步插件-Setting Sync的配置及使用
<!-- more -->
# 步骤
1. 首先在VSCode中搜索Setting Sync并安装
2. 安装成功后直接摁`Shift+Alt+U`（网上说的什么去GitHub的设置里新建token已经不适用了！），然后点击右侧的`Login with Github`，
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011200258.png)
再点击`openlink`，登入你的GitHub账号，
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011200411.png)
待出现success字样后便可关闭浏览器
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011200502.png)
此时VSCode会弹出选择Github Gist的页面，如果你曾经备份过VSCode的配置，那么你会看到你曾经备份的Github Gist的id；如果没有，则可以新建一个Gist用来保存VsCode配置信息。（无需记住你的Gist ID，你所需要记住的只有你的Github账号）
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011201345%20(1).png"/>

3. 访问Github Gist：在你Github头像下拉列表中找到`Your gists`，但直接点开是进不去的，你需要在hosts文件末尾写上`192.30.253.118 gist.github.com`，再次访问即可看到你所有的Github Gist
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191011201423.png"/>
