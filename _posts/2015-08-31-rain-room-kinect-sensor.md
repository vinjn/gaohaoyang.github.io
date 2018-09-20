---
layout: post
title: "花150元淋10分钟雨？在你的朋友圈将被“雨屋”刷爆前，知道这些会让你更酷"
tags: opencv kinect processing
---

* content
{:toc}

![]({{ site.url }}/media/rain-room-preface.jpg)

你打算花 150 元排 3 小时队只为淋 10 分钟雨？小张的这篇文章让你排队的时候多点谈资。这次分享的是关于九月份即将引爆上海滩和朋友圈的《雨屋》，也就是题图中的场景。

将覆盖以下技术点：
- 《雨屋》是什么？
- 输入部分使用什么传感器？
- 处理部分怎么编程？
- 如何合并多个传感器的结果？
- 这项技术还能应用到什么场景？




《雨屋》是什么？
=
先插播新华网的一则新闻 ——《雨屋》登陆上海余德耀美术馆 花150元淋10分钟雨?

    轰动伦敦和纽约的大型装置艺术《雨屋》，9月1日-12月31日入驻上海余德耀美术馆。在昨天的媒体预展上，记者提前感受了这场淋不到的雨，结果发现，只要你动作稍微快一点，雨还是淋得到的，人和机器互动居然还带着点自虐的快感。

    余德耀美术馆引进的《雨屋》，场地面积达到150平方米，是迄今最大的一件《雨屋》。这件作品最吸引人的地方在于，它通过天花板上的高科技装置，能瞬间感知观众的行为，观众所到之处，大雨便戛然而止。《雨屋》让观众在和雨的互动中完成艺术创作，以此探讨人和技术、人和自然之间的互动关系。

《雨屋》是怎么实现的？
=
整个装置可以分成三部分：输入、处理和输出。

输入负责室内人体数据（像素）的采集，处理负责从像素信息中得到位置（x，y坐标），输出负责控制顶部水流喷头的开和关。

![]({{ site.url }}/media/rain-room-1.jpg)

输出部分类似下图中上海世博会鹿特丹案例馆的水幕，给个 ON 信号就出水，给个 OFF 信号水就停掉，本文不展开介绍。

![]({{ site.url }}/media/rain-room-2.jpg)

输入部分使用什么传感器？
=
在展览展示行业中这种互动装置被称为“地面互动投影”，传感器和投影仪都位于顶部。传感器通常是带补光的红外摄像头（IR camera）或深度传感器（depth sensor）。投影仪在这个装置中被水幕所代替，原理是一样的，程序中的 RGB 像素变成 ON/OFF 开关而已。

那么，这个装置用的是红外摄像头还是深度传感器呢？看看这款装置的艺术家是怎么说的：

    Designed by England’s Random International, the Rain Room’s secret is in 3D camera sensors installed across the dark room. Though artist Hannes Koch won’t reveal all the details (it’s “part of the magic”), he says the sensors recognize movements and detect objects – allowing rain to stop falling at precise spots at which the objects stand. When the sensors realize the object has moved away, the rain returns. Each spot gets an approximate five-foot radius of dryness, which is why the Rain Room is limited to 10 visitors at any given time. If the room were to fill up, the rain would stop altogether. 


艺术家 Hannes Koch 来自英国的 Random Internatinal 设计工作室，披露了室内安装了多个 3D 传感器，传感器侦测人体的移动，允许雨滴在人体所在的位置停止。当人离开后，雨继续落下。每个雨水开关都有 30 英寸的干燥半径，也就是 90 厘米的半径。因此雨屋参展人数不能太多（在纽约展出的时候是限制 10 人），不然屋里的雨都会停掉。

所以用的是 3D 的深度传感器，按照行业惯例，最有可能的还是 Kinect 一代。关于深度传感器的综述，请阅读《市售体感设备横评：微软、Intel、苹果、LeapMotion 哪家强！ - 黑客与画家 - 知乎专栏》

![]({{ site.url }}/media/kinect-v1.jpg)

处理部分怎么编程？
=

互动装置都是需要编程的，这个行业涉及到的编程语言有 C++、Java 等，开发工具则是 Visual Studio、Processing、Unity3D、OpenCV 等。如果读者感兴趣，我会在后续的专栏中介绍我之前开发的一些互动装置。

从深度传感器处得到的数据如下图，离传感器越近的值越亮。

![]({{ site.url }}/media/rain-room-processing.jpg)

程序在处理时将整个场景划分为一个个格子，每个格子对应一个喷水口。伪代码如下：

    for each cell:
        if there is non-zero pixel in the cell:
            send ON signal
        else:
            send OFF signal

下面的示意图由 Processing 生成：

![]({{ site.url }}/media/rain-room-cell.jpg)

如何合并多个传感器的结果？
=
一个深度传感器的覆盖面积在 10 平米以下，而《雨屋》的室内面积达到 150 平方米，因此需要近 20 台传感器。传感器之间的画面难免会重叠，一个人会同时出现在多个传感器点画面中，在下面的示意图中整个场景由四个传感器覆盖，实际情况比这个更复杂。（示意图由 Processing 生成。）

![]({{ site.url }}/media/rain-room-fushion.jpg)

因此程序中还需要设置重叠部分像素的归属权。

这项技术还能应用到什么场景？
=
虚拟现实（Virtual Reality）中可以使用这项技术，当你戴上虚拟现实头盔（HMD），漫步在空无一物的房间中，室内定位系统检测到你的真实位置（x、y、z），头盔中渲染的则是虚拟世界中你能看到的场景（由真实的x、y、z映射到虚拟世界中）。

![]({{ site.url }}/media/the-void.jpg)

届时，你看到的不只是雨了，而是一个全新的世界，走得快了也不会被雨淋到。（附图为 The VOID 和 黑客帝国 3）

![]({{ site.url }}/media/the-void-2.jpg)

![]({{ site.url }}/media/matrix-smith.jpg)

----
vinjn.com - Democratizing Visual Computing
