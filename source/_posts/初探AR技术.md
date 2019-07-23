---
title: 初探AR技术
date: 2019-07-15 23:56:05
tags: 
- AR
categories: 
- 移动开发
---

# AR概述   
## 概念定义：
>&emsp;&emsp;增强现实（Augmented Reality，简称AR），也有对应VR虚拟实境一词的翻译称为实拟虚境或扩张现实，是指透过摄影机影像的位置及角度精算并加上图像分析技术，让屏幕上的虚拟世界能够与现实世界场景进行结合与交互的技术。这种技术于1990年提出。随着随身电子产品运算能力的提升，增强现实的用途也越来越广。   ----WIKI
<!-- more -->

&emsp;&emsp;其实，AR只是VR技术中的一个分支（增强式 VR 系统），它即允许用户看到真实的世界，也能同时看到叠加在真实世界上的虚拟对象。  
## 技术手段：
&emsp;&emsp;AR从其技术手段和表现形式上，可以明确分为大约两类：一是Vision based AR，即基于计算机视觉的AR，二是LBS based AR，即基于地理位置信息的AR
+ Vision based AR 
  &emsp;&emsp;基于识别的方式学名叫做Vision based AR，即基于计算机视觉的AR，该方式是利用计算机视觉方法建立现实世界与屏幕之间的映射关系，使我们想要绘制的图形或是 3D 模型可以如同依附在现实物体上一般展现在屏幕上，如何做到这一点呢？本质上来讲就是要找到现实场景中的一个依附平面，然后再将这个 3 维场景下的平面映射到我们 2 维屏幕上，然后再在这个平面上绘制你想要展现的图形，从技术实现手段上可以分为 2 类：
  1. Marker-Based AR  
  &emsp;&emsp;这种实现方法需要一个事先制作好的 Marker(例如: 绘制着一定规格形状的模板卡片或者二维码），然后把 Marker 放到现实中的一个位置上，相当于确定了一个现实场景中的平面，然后通过摄像头对 Marker 进行识别和姿态评估（Pose Estimation），并确定其位置，然后将该 Marker 中心为原点的坐标系称为 Marker Coordinates 即模板坐标系，我们要做的事情实际上是要得到一个变换从而使模板坐标系和屏幕坐标系建立映射关系，这样我们根据这个变换在屏幕上画出的图形就可以达到该图形依附在 Marker 上的效果，理解其原理需要一点 3D 射影几何的知识，从模板坐标系变换到真实的屏幕坐标系需要先旋转平移到摄像机坐标系（Camera Coordinates）然后再从摄像机坐标系映射到屏幕坐标系（其实由于硬件误差这中间还需要理想屏幕坐标系到实际屏幕坐标系的转换，这里不深究），见下图。  
  <img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716010255.png" width="500px">
  2. Marker-Less AR  
  &emsp;&emsp;基本原理与 Marker based AR 相同，不过它可以用任何具有足够特征点的物体 (例如：书的封面) 作为平面基准，而不需要事先制作特殊的模板，摆脱了模板对 AR 应用的束缚。它的原理是通过一系列算法 (如：SURF，ORB，FERN 等) 对模板物体提取特征点，并记录或者学习这些特征点。当摄像头扫描周围场景，会提取周围场景的特征点并与记录的模板物体的特征点进行比对，如果扫描到的特征点和模板特征点匹配数量超过阈值，则认为扫描到该模板，然后根据对应的特征点坐标估计 Tm 矩阵，之后再根据 Tm 进行图形绘制(方法与 Marker-Based AR 类似)。
+ LBS based AR  
  &emsp;&emsp;其基本原理是通过GPS获取用户的地理位置，然后从某些数据源（比如wiki，google）等处获取该位置附近物体(如周围的餐馆，银行，学校等)的POI信息，再通过移动设备的电子指南针和加速度传感器获取用户手持设备的方向和倾斜角度，通过这些信息建立目标物体在现实场景中的平面基准(相当于marker)，之后坐标变换显示等的原理与Marker-Based AR类似。这种AR技术利用设备的GPS功能及传感器来实现，摆脱了应用对Marker的依赖，用户体验方面要比Marker-Based AR更好，而且由于不用实时识别Marker姿态和计算特征点，性能方面也好于Marker-Based AR和Marker-Less AR，因此对比Marker-Based AR和Marker-Less AR，LBS-Based AR可以更好的应用到移动设备上。

## 参考实例：  
1. 增强现实导航应用  
    <img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004025.png" width="500px">
2. 虚拟颠球  
   <img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004126.png" width="500px">
3. 实景翻译  
   <img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004210.png" width="500px">
4. 时光机器  
   <img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190716004315.png" width="500px">

# AR解决方案

## AR SDK
&emsp;&emsp;什么是AR SDK?AR SDK是AR应用的技术核心，驱动着AR App的发展和创新。AR SDK的作用是将数字内容和信息与现实世界融合。SDK的功能最终将决定AR应用程序中的特性和功能，因此根据项目要求选择正确的平台至关重要。AR SDK包含许多应用程序组件，像内容呈现，AR跟踪和场景识别等。内容呈现意味着可以将数字信息和3D对象覆盖在现实世界物体之上，跟踪表示“应用程序的眼睛”，场景识别充当应用程序的中枢神经系统。每个AR SDK都将配备自己独特的属性，使AR开发人员能够以最佳方式开发出具有识别，渲染和跟踪功能的AR应用程序。下面来介绍几个常用的AR SDK：
### ARKit
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723101438.png)
#### 简介
&emsp;&emsp;ARKit 是一个移动端 AR 平台，用于在 iOS 上开发增强现实 app。可以在目前的数千万台 iOS 设备上运行。但为了获得 ARKit 的完整功能，需要 A9 及以上芯片。其实也就是大部分运行 iOS 11 的设备，包括 iPhone 6S。
ARKit提供了以下功能：
+ SLAM tracking and sensor fusion(SLAM场景识别)
+ Ambient lighting estimation(环境光评估)
+ Scale estimations(刻度估量)
+ Vertical and horizontal plane estimation with basic boundaries(基本边界的垂直和水平面检测)
+ Stable and fast motion tracking(快速稳定的动作捕捉)
+ Multiple Face Tracking(多重人脸追踪)
+ Collaborative Sessions(多人会话)
+ Simultaneous Front and Back Camera(同步前置、后置摄像头)
+ People Occlusion

#### 扩展
据了解,ARKit结合Apple自家的机器学习开发库Core ML，可以实现AR场景下的物体识别，详见
[利用ar技术进行物体识别](https://www.bilibili.com/video/av49136585)
(真实性未知)
### ARCore
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723104123.png" width="500px">

#### 简介
&emsp;&emsp;ARCore是Google专有的增强现​​实SDK(对标ARKit)。它使开发人员能够在部分支持ARCore的智能手机和平板电脑上启动并运行AR应用。ARCore最值得注意的功能之一是它还支持部分iOS的设备。ARCore拥有三个重要功能：
+ 光估测：ARCore 可以检测其环境光线的相关信息，并为您提供给定摄像头图像的平均光强度和色彩校正。 此信息让您能够使用与周围环境相同的光照来照亮您的虚拟物体，提升它们的真实感。
+ 环境理解：ARCore 会通过检测特征点和平面来不断改进它对现实世界环境的理解。ARCore 可以查找看起来位于常见水平或垂直表面（例如桌子或墙）上的成簇特征点，并让这些表面可以由您的应用用作平面。 ARCore 也可以确定每个平面的边界，并将该信息提供给您的应用。 您可以使用此信息将虚拟物体置于平坦的表面上。由于 ARCore 使用特征点来检测平面，因此可能无法正确检测像白墙一样没有纹理的平坦表面。
+ 运动跟踪：当您的手机在现实世界中移动时，ARCore 会通过一个名为并行测距与映射（或 COM）的过程来理解手机相对于周围世界的位置。 ARCore 会检测捕获的摄像头图像中的视觉差异特征（称为特征点），并使用这些点来计算其位置变化。 这些视觉信息将与设备 IMU 的惯性测量结果结合，一起用于估测摄像头随着时间推移而相对于周围世界的姿态（位置和方向）。
+ 用户交互：ARCore 利用命中测试来获取对应于手机屏幕的 (x,y) 坐标（通过点按或您希望应用支持的任何其他交互提供），并将一条射线投影到摄像头的视野中，返回这条射线贯穿的任何平面或特征点以及交叉位置在现实世界空间中的姿态。 这让用户可以选择环境中的物体或者与它们互动。
+ 共享：借助 ARCore Cloud Anchor API，您可以创建适用于 Android 和 iOS 设备的协作性或多人游戏应用。使用云锚点，一台设备可以将锚点和附近的特征点发送到云端进行托管。 可以将这些锚点与同一环境中 Android 或 iOS 设备上的其他用户共享。 这使应用可以渲染连接到这些锚点的相同 3D 对象，从而让用户能够同步拥有相同的 AR 体验。
+ 定向点：借助定向点，您可以将虚拟物体置于倾斜的表面上。 当您执行会返回特征点的命中测试时，ARCore 将查看附近的特征点并使用这些特征点估算表面在给定特征点处的角度。 然后，ARCore 会返回一个将该角度考虑在内的姿态。由于 ARCore 使用成簇特征点来检测表面的角度，因此可能无法正确检测像白墙一样没有纹理的表面。
+ 锚点和可跟踪对象：姿态会随着 ARCore 改进它对自身位置和环境的理解而变化。 当您想要放置一个虚拟物体时，您需要定义一个锚点来确保 ARCore 可以跟踪物体随时间推移的位置。 很多时候，您需要基于命中测试返回的姿态创建一个锚点，如用户交互中所述。
+ 增强图像：使用增强图像，您可以构建能够响应特定 2D 图像（如产品包装或电影海报）的 AR 应用。 用户可以在将手机的摄像头对准特定图像时触发 AR 体验，例如，他们可以将手机的摄像头对准电影海报，使人物弹出，然后引发一个场景。

#### ARCore 的工作原理
&emsp;&emsp;从本质上讲，ARCore 在做两件事：在移动设备移动时跟踪它的位置和构建自己对现实世界的理解。
&emsp;&emsp;ARCore 的运动跟踪技术使用手机摄像头标识兴趣点（称为特征点），并跟踪这些点随着时间变化的移动。 将这些点的移动与手机惯性传感器的读数组合，ARCore 可以在手机移动时确定它的位置和屏幕方向。
除了标识关键点外，ARCore 还会检测平坦的表面（例如桌子或地面），并估测周围区域的平均光照强度。 这些功能共同让 ARCore 可以构建自己对周围世界的理解。
&emsp;&emsp;借助 ARCore 对现实世界的理解，您能够以一种与现实世界无缝整合的方式添加物体、注释或其他信息。 您可以将一只打盹的小猫放在咖啡桌的一角，或者利用艺术家的生平信息为一幅画添加注释。 运动跟踪意味着您可以移动和从任意角度查看这些物体，即使您转身离开房间，当您回来后，小猫或注释还会在您添加的地方。

#### 优缺点
&emsp;&emsp;Google出品，必属精品，但唯一美中不足的是，目前(2019年)ARCore在国内支持的设备真的是少之又少（[支持的设备](https://developers.google.com/ar/discover/supported-devices?hl=zh-cn)）,这就让许多想尝试ARCore强大功能的人不得不放弃这个念头(比如我)。
### Vuforia
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723104748.png" width="500px">

#### 简介
&emsp;&emsp;Vuforia是高通推出的针对移动设备扩增实境应用的软件开发工具包。它利用计算机视觉技术实时识别和捕捉平面图像或简单的三维物体（例如盒子），然后允许开发者通过照相机取景器放置虚拟物体并调整物体在镜头前实体背景上的位置。
功能：
+ 识别模型目标<img src="https://library.vuforia.com/content/dam/vuforia-library/articles/solution/unity-model-target/mt_gif_02-optimized.gif" width="500px">
+ 识别图片目标<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723105225.png" width="500px">
+ 识别对象目标<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723105309.png" width="500px">
+ 多重目标<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723105453.png" width="500px">
+ 柱体目标<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723105539.png" width="500px">
+ VuMark：二维码\条形码
+ 使用外置摄像头
+ 平面识别：允许在水平面防止模型内容<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723105801.png" width="500px">

#### 优缺点
&emsp;&emsp;据了解，Vuforia是相对较早的AR SDK，因此在各方面的功能都较为成熟，尤其是与Unity结合开发游戏的效果非常不错，只不过好像商用版本价格有点高。
### Wikitude
<img src="https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723110354.png" width="500px">

#### 简介
&emsp;&emsp;Wikitude提供了一体式增强现实SDK，并结合了3D跟踪技术（基于SLAM）、顶级图像识别和跟踪，以及移动、平板电脑和智能眼镜的地理位置AR，支持可扩展的Unity、Cordova、Titanium和Xamarin框架。可以使用Wikitude SDK构建惊人的基于位置、标记或无标记的AR体验。企业、机构和独立开发人员受益于Wikitude的工具，用于开发适用于Android，iOS，智能手机，平板电脑，智能眼镜的AR应用程序。
功能：
+ 即时跟踪：挖出标记。即时跟踪是使用WikitudeSLAM技术的第一个功能。它可以轻松地映射环境并显示AR内容，无需目标图像，此功能适用于室内和室外环境。
+ 扩展跟踪：一旦目标图像被识别，用户可以通过自由移动设备来继续AR体验，而不需要将标记保持在相机视图中。此功能现在拥有与Wikitude即时跟踪功能相同的SLAM算法，为基于Wikitude的应用提供了强大的性能。
+ 图像识别：Wikitude SDK嵌入了内置的图像识别和跟踪技术，而且这些功能是开箱即用的。它最多可以拍摄1000张可以离线识别的图像。开发人员可以在实时摄像机图像中增强识别的图像和地理位置的兴趣点之间实现无缝切换。
+ 基于位置的服务与GEO数据：Wikitude SDK提供了许多方便的功能，简化了使用地理参考数据。用户兴趣点的设计和布局是完全可定制的，以满足各种需要。
+ 3D增强：Wikitude SDK可以在增强现实场景中加载和渲染3D模型。从行业工具（比如Autodesk Maya 3D或Blender等工具）导入3D模型，每个3D模型基于新的Native API，Wikitude提供了一个用于Unity3D框架的插件，因此开发者可以将Wikitude的计算机视觉引擎整合到基于Unity3D的游戏或应用程序中。
+ 云识别：Wikitude的云识别服务允许开发人员处理云中托管的数千个目标图像。Wikitude的云识别技术是一个可扩展的解决方案，它具有响应时间快、识别率高的特点。每个月向开发者提供了对云识别服务的1,000,000次的免费扫描调用，还为企业提供了专用的服务器选项和定制服务。
### EasyAR
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190723110655.png)
#### 简介
&emsp;&emsp;EasyAR是Easy Augmented Reality的缩写，是视辰信息科技（上海）有限公司的增强现实解决方案系列的子品牌，意义是：让增强现实变得简单易实施，让客户都能将该技术广泛应用到广告、展馆、活动、app等之中。
功能：
+ 表面跟踪
+ 3D物体跟踪
+ 平面图像跟踪
+ 录屏
+ 内容与交互支持
+ API多语言优化
+ Image Target Data

#### 优缺点
&emsp;&emsp;EasyAR是优秀的国产AR解决方案提供者，不仅有价格合理的AR SDK，还有像Web AR、姿态识别、手势识别、云识别等产品。由于是国产平台，咨询问题、疑难解答等都十分方便。不过缺点就是某些高端功能可能不如国外SDK功能强劲，并且使用文档看上去不是很详细，有点廉价的感觉。
## Web AR 
![](https://raw.githubusercontent.com/VinceMar/hexo_pic/master/img/20190717233335.png)

1. AR.js
   &emsp;&emsp;AR.js是由Jerome Etienne开发维护的一款WebAR库，代码托管在[GitHub](https://github.com/jeromeetienne/AR.js)。其实，AR.js就是将WebAR所需的各种组件与库经过整理、维护，拼凑成了一把锋利精美的瑞士军刀，可以流畅地以高帧率运行在各种设备上。其中，AR.js主要封装了以下几个库：
   + JSARToolKit：老牌开源AR库ARToolKit的JavaScript版本,主要提供了识别和追踪 marker 的功能。
   + Three.js:Three.js是一款开源的主流3D绘图JS引擎，我们知道WebGL是一种网页3D绘图标准，和jQuery简化了HTML DOM操作一样，Three.js可以简化WebGL编程。
   + aframe-ar.js:A-Frame 团队发布的 其中包括了一些 Web AR 组件，用 A-Frame 的各种组件可以让你用很少的代码构建出 AR 所需要的 3D 立体世界。A-Frame.js是一个用来构建虚拟现实（VR）应用的网页开发框架（部分API也可以开发AR应用），是当下用来开发WebVR内容主流技术方案。A-Frame基于HTML，容易上手。但是A-Frame不仅仅是一个3D场景渲染引擎或者一个标记语言。其核心思想是基于Three.js来提供一个声明式、可扩展以及组件化的编程结构。  
   在AR.js中，分为两种写法：使用Three.js结合JS库以及使用A-Frame.js，若选择使用Three.js结合JS库的方式，需要学习了解很多JS库的用法，代码量比较大，学习成本较高；而使用A-Frame.js，则可以大量减少代码量，比如著名的[10行代码实现AR](https://medium.com/arjs/augmented-reality-in-10-lines-of-html-4e193ea9fdbf)。
2. WebARonARKit,WebARonARCore  
   &emsp;&emsp;ARKit 和 ARCore 分别是苹果和谷歌两大巨头出品的移动 AR SDK，提供的功能也类似：运动追踪、环境感知和光线感应，我相信很多对 AR 感兴趣的开发者对这两个 SDK 都不陌生。但这两个都是移动 AR 的 SDK，于是谷歌的 AR 团队提供了 WebARonARKit 和 WebARonARCore 两个库，以便开发者能用 Web 技术来基于 ARKit 和 ARCore 开发，从而实现 WebAR。这两个库虽然功能强大，但因为是ARKit和ARCore的Web衍生版本，所以国内支持这两个库的移动设备仍然很少。
3. EasyAR
    &emsp;&emsp;EasyAR是国内首款免费全平台AR引擎，提供了EasyAR SDK以及WebAR等解决方案，可以快速构建AR应用。

# 总结
&emsp;&emsp;目前来说，使用AR SDK来开发AR应用技术层面已经较为成熟完整，开发者只需要根据项目需要选择合适的AR SDK，因此AR产品是否有优势取决于其是否有独特创意。
而在web端，无论采用哪种实现手段，最主要是运行环境对webRTC、webGL 两个特性的支持程度。而且，Web端目前只有基于标记图Marker的应用有着比较良好的体验，WebAROnARCore和WebAROnARKit所支持的MarkerLess方案都还仅仅是测试阶段，并且国内外对webAR 的讨论和demo 案例目前都比较少，能够搜到的资料几乎都是英文版，总的来说使用WebAR来实现一些简单的功能是可以的，完整的商业项目暂不推荐。

# 本文参考
AR技术原理 https://www.jianshu.com/p/3aac1ec6ea2f  
AR技术这么火 但你知道其中背后的工作原理吗？https://zhuanlan.zhihu.com/p/48281104  
增强现实技术 https://blog.csdn.net/u010922186/article/details/41554171
Web AR 入門：AR.js Tracking.js Three.js A-frame.js
http://skyroxas.tw/web-ar-%e5%85%a5%e9%96%80%ef%bc%9a-ar-js-tracking-js-three-js-a-frame-js/
Web 前端中的增强现实（AR）开发技术 https://geekplux.com/2018/01/18/augmented-reality-development-tech-in-web-frontend/
12 Best Augmented Reality SDKs https://dzone.com/articles/12-best-augmented-reality-sdks