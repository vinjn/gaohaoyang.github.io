---
layout: post
title:  "计算机视觉、图形学和图像处理，三者有什么联系？"
tags: opencv graphics computer-vision
---

* content
{:toc}

![]({{ site.url }}/media/13th-floor.jpg)

先说区别
=

## Computer Graphics，简称 CG ##

输入的是对虚拟场景的描述，通常为多边形数组，而每个多边形由三个顶点组成，每个顶点包括三维坐标、贴图坐标、rgb 颜色等。输出的是图像，即二维像素数组。

    [xyz xyz xyz ... xyz] -> 图片

## Computer Vision，简称 CV ##

输入的是图像或图像序列，通常来自相机、摄像头或视频文件。输出的是对于图像序列对应的真实世界的理解，比如检测人脸、识别车牌、区分猫狗。

    图片 -> dog or cat?
    图片 -> [xyz xyz xyz ... xyz]




## Digital Image Processing，简称 DIP ##

输入的是图像，输出的也是图像。Photoshop 中对一副图像应用滤镜就是典型的一种图像处理。常见操作有模糊、灰度化、增强对比度等。

    图片 -> ps后的图片

再说联系
=

## CG 中也会用到 DIP ##

现今的三维游戏为了增加表现力都会叠加全屏的后期特效，原理就是 DIP，只是将计算量放在了显卡端。通常的做法是绘制一个全屏的矩形，在 Pixel Shader 中进行图像处理。


## CV 大量依赖 DIP 来打杂活 ##

比如对需要识别的照片进行预处理，增强对比度、去除噪点。

## 最后还要提到今年的热点——增强现实（AR）##

它既需要 CG，又需要 CV，当然也不会漏掉 DIP。它用 DIP 进行预处理，用 CV 进行跟踪物体的识别与姿态获取，用 CG 进行虚拟三维物体的叠加。

----
vinjn.com - Democratizing Visual Computing
