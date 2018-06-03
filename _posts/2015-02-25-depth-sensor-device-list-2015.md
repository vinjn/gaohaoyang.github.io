---
layout: post
title:  "市售体感设备横评：微软、Intel、苹果、LeapMotion 哪家强！"
categories: Kinect
tags:  Kinect OpenNI
---

![]({{ site.url }}/media/fish-and-bear.jpg)

这篇文章中我整理了市面上常见的深度摄像头、优缺点及使用场景。
- Microsoft Kinect
- Microsoft Kinect One
- Intel / Creative / SoftKinetic
- Leap Motion
- //DUO
- PrimeSense / Apple / 华硕（ASUS）

Microsoft Kinect
=
Kinect 一代的问世是具有历史意义，自此深度摄像头进入了普通消费者的视野。它本是微软 Xbox 360 游戏机的一个外设，微软并没有提供 PC 上的驱动和开发环境。怎想到，销售没多久便遭到（褒义的）黑客破解，它不仅在 Windows 上运行，还可以在 OS X 和 Linux 上运行。微软没有禁止破解的行为，反而顺势而为推出了官方的 Kinect SDK，一时传为佳话。

![]({{ site.url }}/media/kinect-v1.jpg)

## 优点 ##

- 可以获取深度数据（320＊240）、rgb 数据（640＊480）、声音、骨骼节点（20个）
- 拥有三套 SDK：微软 SDK、OpenNI、libfreenect
- 后两个 SDK 是跨平台，支持各种开发语言
- 价格便宜
- 社区成熟，开发资源丰富

![]({{ site.url }}/media/kinect-v1-power.jpg)

## 缺点 ##

- 传感器分辨率不够，看不清手指
- 由于使用结构光技术，深度传感器的可视范围无法重叠
- OpenNI 和 libfreenect 虽然跨平台，但是功能远不如微软 SDK
- 设备尺寸大，需要一坨电源线

## 致命缺点 ##

微软已宣布停止生产 Kinect 一代

## 使用场景 ##

- Windows 及其他操作系统
- 人离设备三米左右
- 对精度要求不高

Microsoft Kinect One
=

二代是随着 Xbox One 出现的，二代又叫做 Kinect One，最初和游戏主机是捆绑销售的。相比一代，增强了不少功能，因为苛刻的软硬件需求，也引起一些不便。

![]({{ site.url }}/media/kinect-v2.jpg)

## 优点 ##

- 分辨率更大、可以看到更广阔的场景
- 可以检测到的人体关节点更多（25个），能看清手指
- 拥有两套 SDK：微软 SDK、libfreenect2
- 可以开发 Windows Store 应用

![]({{ site.url }}/media/kinect-v2-power.jpg)

## 缺点 ##

- libfreenect2 基本不能检测骨骼，功能缺太多，同时 OpenNI 也不支持它，因此局限于 Windows 平台
- 设备尺寸比一代更大
- 需要一坨电源线
- 比一代贵一些

## 致命缺点 ##

- 只能运行在 64 位 Windows 8 以上操作系统
- 必须使用 USB 3.0 端口

## 使用场景 ##

对精度要求高
能满足致命缺点里的三项，部分软件在 Windows 8 上不稳定

Intel / Creative / SoftKinetic
=

![]({{ site.url }}/media/softkinetic.jpg)

Intel 2013 年搞了个感知计算大赛，Intel 提供了 Perceptual Computing 体感开发包 配上 Creative Senz3D 传感器。这个 Senz3D 设备表面上看是 Creative 制造的，其实从里到外都是 Soft Kinetic 的。很多人可能没听说过 Soft Kinetic，这是一家比利时的深度传感器技术公司，它的产品有三种：

- 深度感应芯片，授权给德州仪器等半导体公司
- 深度传感器设备，以成品的形式卖给普通消费者
- 体感中间件 iisu，授权给索尼在 PlayStation 4 的摄像头上实现了体感

![]({{ site.url }}/media/softkinetic-2.jpg)

## 优点 ##
- 小巧，普通 USB 摄像头的尺寸
- 不需要外界电源线
- 近距离使用，可实现表情分析和手势识别

## 缺点 ##

- 不适合远距离交互，也无法检测完整的身体
- 只能在中高端的 Intel CPU 上才能运行

## 致命缺点 ##
- 无

## 使用场景 ##

依附在屏幕上，让坐在电脑桌前的人使用

Leap Motion
=
## 优点 ##

- 小巧，一根 usb 线就可以使用
- 跨平台
- 支持的开发语言比较多，甚至通过 WebSocket 实现了浏览器中的 JavaScript API
- 跟踪手指和手掌，精度较高

![]({{ site.url }}/media/leapmotion.jpg)

## 缺点 ##

- 检测范围小，手臂酸疼（见上图）
- 不能检测身体和脸部
- 作为生产力工具，完全无法替代鼠标键盘

![]({{ site.url }}/media/leapmotion-2.jpg)

## 致命缺点 ##

- 找不到合适的使用场景 -___-

## 使用场景 ##

- 无

//DUO
=
DUO 属于剑走偏分，并不面向终端消费者，而是研究人员。产品名字也比较另类，甚至于你搜索 duo 是搜不到它滴，需要搜 duo3d。

![]({{ site.url }}/media/duo.jpg)

## 优点 ##

- 小巧
- 高速摄像头（120 fps）
- 跨平台
- 可用于嵌入式设备

## 缺点 ##

- 价格贵
- 社区不成熟
- 没有成功的商业案例
- SDK 的功能有限，没有任何体感功能

## 使用场景 ##

需要自定义的硬件功能，并且有体感算法研发能力的公司

PrimeSense / Apple / 华硕（ASUS）
=

PrimeSense 是 Kinect 一代的芯片供应商，位于以色列，也是开源体感开发包 OpenNI 的维护者。自从被 Apple 收购后，销声匿迹，OpenNI 也停止更新。预计 Apple 不久的将来会举动，有可能与电视或游戏主机相结合，再一次改变世界。

![]({{ site.url }}/media/primsense.jpg)

华硕的 xtion 用的是 PrimeSense 芯片的授权，在不久的未来 PrimeSense 会停止供应芯片，到时候市场上就买不到 xtion 了。除此以外和 Kinect 一代的优缺点类似，不再重复了。

![]({{ site.url }}/media/asus.jpg)

结论
=
服不服，先来跑个分：

- Kinect：功能 4 分，价格 5 分，小张打分 5 分
- Kinect One：功能 5 分，价格 4 分，小张打分 4 分
- xtion：功能 4 分，价格 4 分，小张打分 4 分
- SoftKinetic：功能 3 分，价格 4 分，小张打分 3 分
- DUO：功能 3 分，价格 3 分，小张打分 3 分
- Leap Motion：功能 2 分，价格 3 分，小张打分 2 分

如果按照开发需求，也可以这么推荐：
- 低精度的远距离体感开发，用 Kinect 一代或 xtion，在不久的将来它们都将停产，所以囤货吧
- 高精度的远距离体感开发，用 Kinect 二代
- 近距离基于人脸或手势的互动，用 SoftKinetic
- 自己有硬件团队和算法团队，且前三项都无法满足需求，用 DUO
- 人傻钱多，用 Leap Motion

或者，我们再等等苹果？


----
http://vinjn.com - 关注人工智能+视觉计算