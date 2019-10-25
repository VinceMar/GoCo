---
title: Web端游戏技术初探
date: 2019-10-24 22:53:09
tags:
- H5游戏
- 微信小游戏
categories:
- Web开发
---

# 前言 
&emsp;&emsp;近期需要准备一个大赛，作品涉及到H5游戏开发相关知识，本篇文章用来记录我所梳理的资料。
<!-- more -->
# 简介
## 名词辨析
先来列举几个概念：  
+ **传统网页游戏（Flash游戏）：** 客户端使用Flash的ActionScript编程语言进行开发并与后台进行数据交互的游戏。
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191025014431.png"/>
+ **H5游戏：** H5游戏即HTML5游戏（Html5 Game），可用于移动端的web页面，让玩家无需下载就可以自在体验游戏，它还能在移动端做出Flash无法做出的很多动画效果出来，亦可与Flash游戏相媲美的，在将来的H5游戏也将朝着移动化与社交化趋势而发展的。  
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191025014514.png"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191025014608.png"/>

&emsp;&emsp;以上两种游戏统称为网页游戏，即无需客户端，只在浏览器便可运行的游戏。其中，H5游戏又包括近年来很火爆的**微信小游戏**，它相较于原生的H5游戏有一些变化，但本质上依旧是H5技术。

## 区别

+ **H5游戏与传统网页游戏：** H5游戏使用的是Javascript开发，而传统网页游戏使用的是Flash。随着技术的发展，Flash已处于灭绝的边缘，所以现代网页游戏（即网页游戏）基本等同于H5游戏。
+ **H5游戏与微信小游戏：** 微信小游戏是H5游戏的一个子分支，属于H5游戏但并不等于H5游戏，具体区别如下图
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191025011029.png"/>

## 优缺点
H5游戏的优点：
+ 多设备跨平台
+ 即时更新
+ 天然的营销推广利器
+ 无需下载和安装

缺点：
+ 受浏览器和手机机型支持所限
+ 游戏者的存留度低
+ 变现问题难

# 技术栈
目前，开发H5游戏的主流技术为：
+ **Canvas:**  
&emsp;&emsp;Canvas是HTML5新增的一个元素对象，名副其实就是一个画布，浏览器 js 配有相应的操作api，可以不再依赖其他的API或组件而直接绘图，相当于2D的API。  
&emsp;&emsp;一般用于绘制比较复杂的动画,做游戏之类的,由于canvas是HTML5带的,所以不支持低版本浏览器,特别是IE,canvas只是一个画布,*绘制上去的东西,例如图片,都是转换成像素点绘制上去的*,所以没有event事件,如果需要添加交互事件,需要自己手动计算绘制的对象所在坐标以及层级,还好这部分有第三方库。基本上除了HTML5游戏,一些酷炫的动画,正常的网页交互很少用到。  

+ **WebGL:**  
&emsp;&emsp;WebGL主要是3D为主，不过2D的绘图要求也可以变通来实现。WebGL无论如何都需要一个显示对象来呈现，这个对象就是 Canvas，仅此而已，WebGL不对Canvas有任何附加的操作API，那部分属于浏览器js支持的范畴。可看作能在浏览器上运行的OpenGL,WebGL的HTML节点名称用的也是canvas,但是他的渲染则和canvas不同,他可以支持硬件加速,支持3D,可用于3D游戏的开发,目前很少有3D的HTML5游戏,现在你能看到很多酷炫的图形交互的3D图表,大多用WebGL来渲染的。WebGL 也继承 OpenGL ES 2.0 的兼容性支持能力，在不同的设备上做有限的支持，需要运行时查询。  

# 实现
&emsp;&emsp;目前来说，若使用Canvas或WebGL的方式来开发一款完成度较高的H5游戏，时间成本和技术成本都会很高（俗称重复造轮子），所以现在主流的解决方案是使用现有的H5游戏开发引擎（包括微信小游戏，使用适配的引擎即可）。
+ **LayaAir:** 在渲染模式上，LayaAir 支持 Canvas 和 WebGL 两种方式；在工具流的支持程度上，主要是提供了 LayaAir IDE。LayaAir IDE 包括代码模式与设计模式，支持代码开发与美术设计分离，内置了 SWF 转换、图集打包、JS 压缩与加密、APP 打包、Flash 发布等实用功能。
+ **Egret Engine:** 白鹭引擎是企业级游戏引擎，有团队维护。Egret 在工作流的支持上做的是比较好的，从 Wing 的代码编写，到 ResDepot 和 TextureMerger 的资源整合，再到 Inspector 调试，最后到原生打包（支持 APP 打包），游戏开发过程中的每个环节基本都有工具支撑。官网上的示例，教程也是比较多。值得一提的是，今年5月白鹭引擎支持了 WebAssembly ，这对于性能的提升又是一大里程碑。
+ **Three.js:** 相信对于很多有关注 3D 游戏的开发者来说，Three.js 早已经耳熟能详了。实际上，Three.js 官方定位并不是游戏引擎，而是一个 JS 3D 库。Three.js 更倾向于展示型的视觉呈现，比较少直接拿 Three.js 来开发 H5 游戏。渲染环境上，Three.js 支持 WebGL 和 CSS3D 两种渲染模式。  

<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20191025014914.gif"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/%E6%BC%94%E7%A4%BA3.gif"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/%E6%BC%94%E7%A4%BA4.gif"/>
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/%E6%BC%94%E7%A4%BA2.gif"/>

## 关于微信小游戏开发的引擎：
&emsp;&emsp;许多开发者对小游戏对 Cocos、Egret、Laya、Unity 等游戏引擎的支持情况非常关心。但是小游戏是一个不同于浏览器的 JavaScript 运行环境，没有 BOM 和 DOM API。然而，基本上所有基于 HTML5 的游戏引擎都是依赖浏览器提供的 BOM 和 DOM API 的。所以如果要在小游戏中使用引擎，需要对引擎进行改造。  
+ **Cocos Creator:** http://docs.cocos.com/creator/manual/zh/publish/publish-wechatgame.html
+ **Egret:** http://developer.egret.com/cn/github/egret-docs/Engine2D/minigame/introduction/index.html
+ **LayaBox:** https://ldc.layabox.com/doc/?nav=zh-as-5-0-1  

详情请参考[微信官方文档](https://developers.weixin.qq.com/minigame/dev/tutorial/base/engine.html)

# 参考资料
[1] [Canvas、 SVG 和 WebGl三者之间的区别](https://blog.csdn.net/Thea12138/article/details/79723380)  
[2] [网页游戏用的是什么编程语言？](https://www.zhihu.com/question/20464392)  
[3] [什么是H5游戏？H5游戏和普通游戏有什么区别？](http://ask.zol.com.cn/q/2116045.html)  
[4] [请问如何从头开始学习制作一款HTML5 小游戏？](https://www.zhihu.com/question/19954833)  
[5] [目前有哪些比较成熟的 HTML5 游戏引擎？](https://www.zhihu.com/question/20079322)  
[6] [H5游戏开发：游戏引擎入门推荐](https://aotu.io/notes/2017/12/27/h5-game-engine-recommend/index.html)  
[7] [微信小游戏开发怎么选游戏引擎](https://zhuanlan.zhihu.com/p/66962195)