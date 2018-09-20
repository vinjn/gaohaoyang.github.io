---
layout: post
title: "WWDC 有感：苹果爱自己，它爱开发者嘛？"
tags: opengl opencl apple usdz vulkan
---

* content
{:toc}

![]({{ site.url }}/media/1984-apple.jpg)

在 1984 年的超级碗广告中，苹果将自己描述为一锤子打破老大哥统治的叛逆者，这个广告获得了极大的成功。彼时的老大哥指的是 IBM。

在 2018 年的今天，苹果已经不再是一个叛逆者，它变得保守、封闭、与世隔绝，它成了老大哥。

老大哥的作风在这次的 WWDC 的发布中无处不在，我单说两则图形相关的新闻：
- 苹果喜提 USDZ
- 苹果抛弃 OpenGL




苹果针对增强现实发布 3D 格式 USDZ
=

USDZ 是苹果和皮克斯共同推出的。USD 格式在电影特效行业比较常见，是皮克斯发明的。至于 USDZ 本质上就是把 USD 文件 ZIP 压缩一下（蛤？）。

这格式本身没问题，问题在于这世界上的 3D 格式已经够多，以开源界最常用的格式导入库 assimp 为例，它支持的 3D 格式多达 50 种。

为了解决 AR、VR 应用中的 3D 格式问题，名为 glTF 的工业标准早好几年就提出了，并得到了整个行业的认可和支持（不包括苹果）。

我们看下支持 glTF 的公司列表，都是巨头：


![]({{ site.url }}/media/gltf-industry.jpg)


在列表中有发现微软嘛？微软为了支持 glTF 动了真感情，Office 原生支持 glTF 格式，比如来自 @pissang1 的这张 PPT 实图。


![]({{ site.url }}/media/gltf-office.jpg)


回到苹果这边，USDZ 的推出，除了给开发者添加麻烦，有什么帮助呢？

macOS 10.14 Mojave 将抛弃 OpenGL 和 OpenCL
=
先放出官方原文

    Deprecation of OpenGL and OpenCL

    Apps built using OpenGL and OpenCL will continue to run in macOS 10.14, but these legacy technologies are deprecated in macOS 10.14. Games and graphics-intensive apps that use OpenGL should now adopt Metal. Similarly, apps that use OpenCL for computational tasks should now adopt Metal and Metal Performance Shaders.

    Metal is designed from the ground up to provide the best access to the modern GPUs on iOS, macOS, and tvOS devices. Metal avoids the overhead inherent in legacy technologies and exposes the latest graphics processing functionality. Unified support for graphics and compute in Metal lets your apps efficiently utilize the latest rendering techniques.

再翻译翻译什么叫做惊喜

    我们抛弃 OpenGL 和 OpenCL 了

    老的应用在 macOS 10.14 下还能折腾，折腾不了几天。请尽快投降，换成 Metal。对了，隔壁的 OpenCL 应用也快快缴枪，换成 Metal 和 Metal Performance Shaders。

    Metal 是最棒的最棒的最棒的最棒的。老的 API 不好。Metal 是最棒的最棒的最棒的最棒的。


OpenGL 和 OpenCL 是不是该淘汰？是的，它们有缺陷，应该淘汰。

事实上，为了解决这些缺陷，名为 Vulkan 的工业标准在 2016 年就提出了，并得到了整个行业的认可和支持（再一次不包括苹果）。


![]({{ site.url }}/media/vulkan-industry.jpg)


然而苹果是怎么做的呢？OpenGL 游戏如果不用 Metal 推翻重写，在未来的 macOS 上将无法运行。

隔壁天天被骂的微软，即便在 Windows 10 上强推 DirectX 12，依然很老实地支持上古时代的游戏，比如基于 DirectX 8.1 的魔兽 3。

受影响的不仅仅是游戏，开源的三维建模软件 Blender，同时使用 OpenGL 和 OpenCL。因为苹果这个决定，代码得重写多少？

Processing、openFrameworks 这些数字艺术家最爱的创意编码工具，底层都用 OpenGL，整个开发者社区也都围绕着 OpenGL 建立。因为苹果这个决定，就要逼着他们叛逃 Windows？

今天，我们对照着乔布斯的这句名言，是不是很讽刺呢？

![]({{ site.url }}/media/steve-jobs-customer-exp.jpg)


----
相关技术文章
- [glTF](https://www.vinjn.com/tag/#glTF)
- [Vulkan](https://www.vinjn.com/tag/#Vulkan)
- [OpenGL](https://www.vinjn.com/tag/#OpenGL)
- [Processing](https://www.vinjn.com/tag/#Processing)
- [openFrameworks](https://www.vinjn.com/tag/#openFrameworks)

----
vinjn.com - Democratizing Visual Computing
