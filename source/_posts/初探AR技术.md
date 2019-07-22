---
title: 初探AR技术
date: 2019-07-15 23:56:05
tags: 
- AR
categories: 
- 移动开发
---
<style>
p {
    text-indent:2em;
}
</style>
# AR概述   
## 概念定义：
>增强现实（Augmented Reality，简称AR），也有对应VR虚拟实境一词的翻译称为实拟虚境或扩张现实，是指透过摄影机影像的位置及角度精算并加上图像分析技术，让屏幕上的虚拟世界能够与现实世界场景进行结合与交互的技术。这种技术于1990年提出。随着随身电子产品运算能力的提升，增强现实的用途也越来越广。   ----WIKI
<!-- more -->

其实，AR只是VR技术中的一个分支（增强式 VR 系统），它即允许用户看到真实的世界，也能同时看到叠加在真实世界上的虚拟对象。  
## 技术手段：
AR从其技术手段和表现形式上，可以明确分为大约两类：一是Vision based AR，即基于计算机视觉的AR，二是LBS based AR，即基于地理位置信息的AR
+ Vision based AR 
  基于识别的方式学名叫做Vision based AR，即基于计算机视觉的AR，该方式是利用计算机视觉方法建立现实世界与屏幕之间的映射关系，使我们想要绘制的图形或是 3D 模型可以如同依附在现实物体上一般展现在屏幕上，如何做到这一点呢？本质上来讲就是要找到现实场景中的一个依附平面，然后再将这个 3 维场景下的平面映射到我们 2 维屏幕上，然后再在这个平面上绘制你想要展现的图形，从技术实现手段上可以分为 2 类：
  1. Marker-Based AR  
  这种实现方法需要一个事先制作好的 Marker(例如: 绘制着一定规格形状的模板卡片或者二维码），然后把 Marker 放到现实中的一个位置上，相当于确定了一个现实场景中的平面，然后通过摄像头对 Marker 进行识别和姿态评估（Pose Estimation），并确定其位置，然后将该 Marker 中心为原点的坐标系称为 Marker Coordinates 即模板坐标系，我们要做的事情实际上是要得到一个变换从而使模板坐标系和屏幕坐标系建立映射关系，这样我们根据这个变换在屏幕上画出的图形就可以达到该图形依附在 Marker 上的效果，理解其原理需要一点 3D 射影几何的知识，从模板坐标系变换到真实的屏幕坐标系需要先旋转平移到摄像机坐标系（Camera Coordinates）然后再从摄像机坐标系映射到屏幕坐标系（其实由于硬件误差这中间还需要理想屏幕坐标系到实际屏幕坐标系的转换，这里不深究），见下图。  ![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716010255.png)
  2. Marker-Less AR  
  基本原理与 Marker based AR 相同，不过它可以用任何具有足够特征点的物体 (例如：书的封面) 作为平面基准，而不需要事先制作特殊的模板，摆脱了模板对 AR 应用的束缚。它的原理是通过一系列算法 (如：SURF，ORB，FERN 等) 对模板物体提取特征点，并记录或者学习这些特征点。当摄像头扫描周围场景，会提取周围场景的特征点并与记录的模板物体的特征点进行比对，如果扫描到的特征点和模板特征点匹配数量超过阈值，则认为扫描到该模板，然后根据对应的特征点坐标估计 Tm 矩阵，之后再根据 Tm 进行图形绘制(方法与 Marker-Based AR 类似)。
+ LBS based AR  
  其基本原理是通过GPS获取用户的地理位置，然后从某些数据源（比如wiki，google）等处获取该位置附近物体(如周围的餐馆，银行，学校等)的POI信息，再通过移动设备的电子指南针和加速度传感器获取用户手持设备的方向和倾斜角度，通过这些信息建立目标物体在现实场景中的平面基准(相当于marker)，之后坐标变换显示等的原理与Marker-Based AR类似。这种AR技术利用设备的GPS功能及传感器来实现，摆脱了应用对Marker的依赖，用户体验方面要比Marker-Based AR更好，而且由于不用实时识别Marker姿态和计算特征点，性能方面也好于Marker-Based AR和Marker-Less AR，因此对比Marker-Based AR和Marker-Less AR，LBS-Based AR可以更好的应用到移动设备上。

## 参考实例：  
1. 增强现实导航应用  
    ![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004025.png)
2. 虚拟颠球  
   ![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004126.png)
3. 实景翻译  
   ![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004210.png)
4. 时光机器  
   ![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004315.png)  

# AR解决方案

## AR SDK
什么是AR SDK?
## Web AR 
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190717233335.png)

1. AR.js
   AR.js是由Jerome Etienne开发维护的一款WebAR库，代码托管在[GitHub](https://github.com/jeromeetienne/AR.js)。其实，AR.js就是将WebAR所需的各种组件与库经过整理、维护，拼凑成了一把锋利精美的瑞士军刀，可以流畅地以高帧率运行在各种设备上。其中，AR.js主要封装了以下几个库：
   + JSARToolKit：老牌开源AR库ARToolKit的JavaScript版本,主要提供了识别和追踪 marker 的功能。
   + Three.js:Three.js是一款开源的主流3D绘图JS引擎，我们知道WebGL是一种网页3D绘图标准，和jQuery简化了HTML DOM操作一样，Three.js可以简化WebGL编程。
   + aframe-ar.js:A-Frame 团队发布的 其中包括了一些 Web AR 组件，用 A-Frame 的各种组件可以让你用很少的代码构建出 AR 所需要的 3D 立体世界。A-Frame.js是一个用来构建虚拟现实（VR）应用的网页开发框架（部分API也可以开发AR应用），是当下用来开发WebVR内容主流技术方案。A-Frame基于HTML，容易上手。但是A-Frame不仅仅是一个3D场景渲染引擎或者一个标记语言。其核心思想是基于Three.js来提供一个声明式、可扩展以及组件化的编程结构。  
   在AR.js中，分为两种写法：使用Three.js结合JS库以及使用A-Frame.js，若选择使用Three.js结合JS库的方式，需要学习了解很多JS库的用法，代码量比较大，学习成本较高；而使用A-Frame.js，则可以大量减少代码量，比如著名的[10行代码实现AR](https://medium.com/arjs/augmented-reality-in-10-lines-of-html-4e193ea9fdbf)。
2. WebARonARKit,WebARonARCore  
   ARKit 和 ARCore 分别是苹果和谷歌两大巨头出品的移动 AR SDK，提供的功能也类似：运动追踪、环境感知和光线感应，我相信很多对 AR 感兴趣的开发者对这两个 SDK 都不陌生。但这两个都是移动 AR 的 SDK，于是谷歌的 AR 团队提供了 WebARonARKit 和 WebARonARCore 两个库，以便开发者能用 Web 技术来基于 ARKit 和 ARCore 开发，从而实现 WebAR。这两个库虽然功能强大，但因为是ARKit和ARCore的Web衍生版本，所以国内支持这两个库的移动设备仍然很少。
3. EasyAR
    EasyAR是国内首款免费全平台AR引擎，提供了EasyAR SDK以及WebAR等解决方案，可以快速构建AR应用。

# 总结
目前，在web端，无论采用哪种实现手段，最主要是运行环境对webRTC、webGL 两个特性的支持程度。而且，Web端目前只有基于标记图Marker的应用有着比较良好的体验，无标记图的方式据我所知AR.js开发团队目前已有进展，但国内外对webAR 的讨论和demo 案例目前都比较少，能够搜到的资料几乎都是英文版。

# 本文参考：
https://www.jianshu.com/p/3aac1ec6ea2f  
https://zhuanlan.zhihu.com/p/48281104  
https://blog.csdn.net/u010922186/article/details/41554171
http://skyroxas.tw/web-ar-%E5%85%A5%E9%96%80%EF%BC%9A-ar-js-tracking-js-three-js-a-frame-js/
Web 前端中的增强现实（AR）开发技术 https://geekplux.com/2018/01/18/augmented-reality-development-tech-in-web-frontend/
12 Best Augmented Reality SDKs https://dzone.com/articles/12-best-augmented-reality-sdks