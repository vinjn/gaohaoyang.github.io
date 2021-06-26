---
layout: post
title: "你对手游包体的大小感到焦虑吗？试试 TapTap 的这款开源软件吧"
tags: taptap xd xindong apk ipa optimization
---

* content
{:toc}

在 NVIDIA 做性能分析的时候，接触的 PC 大作基本很多都是 10G 以上的，因此也从没有想过包体大小会是个重要的优化项目。来到心动网络后，与自研的项目进行过合作，也和 TapTap 上的独家游戏团队有过交流，这才意识到包体大小对于手游的重要性，总结下有两点：

- 影响转化率，大包体会直接遏制玩家的点击下载欲望，以及降低下载完成的概率。
- 影响游戏性能，引起包体膨胀的一大原因是错误的贴图设置，比如过高的分辨率、未使用合适的压缩格式等。此类问题同时也会增加游戏运行时的内存占用及带宽消耗，并直接引起掉帧和发热。（对发热的处理也是个手游特有的焦虑项，日后细说。）




# 这个问题，小张用三个步骤来解决

在 10 多年的工作历程中小张遇到过各种难缠问题，每次小张都会将问题的解决过程拆成三步

- 收集数据
- 处理数据
- 展示数据

对于优化手游包体的大小这个问题，最难的是第一步。你可能会问了，对于拥有源代码的自研项目，为什么收集数据会是个难点呢？这是因为心动是一家非主流的游戏公司，它拥有 TapTap 这个互联网平台。因此当我在帮助内部项目组，就在思考对外发布的可行性。一个工具，能够解决自身的痛点固然是好事，但是如果能帮助到 TapTap 上的游戏，甚至没有在 TapTap 上发布的游戏，那么这件事情的意义就会大更多。

在这个假设下，我开发的工具需要有无敌的适应性，只要有 apk 文件，不论是 Unity 还是 Unreal 游戏，不论是否有游戏的源代码都应该支持。

这个软件我取名为 [pkg-doctor / 包体医生](https://github.com/taptap/pkg-doctor)，对于游戏包体它就像个医生，望闻问切、抽血CT，一番操作后给出报告。

使用方法也非常简单粗暴

> pkg-doctor.exe /path/to/game.apk

对于 Unity 项目，我使用 Perfare 的 [Asset Studio](https://github.com/Perfare/AssetStudio) 来解决收集资源文件名、尺寸、格式等数据的需求。以下是它的用户界面，能看到文件名、类型、尺寸、内容预览等信息，可以满足第一步的需求。

![]({{ site.url }}/media/asset-studio.jpg)

经过我修改后的 Asset Studio 有能力读取一个 Unity 游戏的 apk 文件，输出包含资产名称、类型、尺寸、预览等信息的 CSV 文件，供第下一步使用。下图是由 Unity 官方范例产生的 CSV 文件：

```
Name,Container,Type,Dimension,Format,Size,FileName,Hash,OriginalFile
AreaTex,,Texture2D,160x560,RGB24,268948,,b86fcc9bb386113bd09eaf566973b0e0,globalgamemanagers.assets
Arial,,Font,,,286,,,Resources/default
box,,Texture2D,12x12,ARGB4444,400,,fad9a48b7bec21aaa79672dee0cbcaa9,Resources/default
brush_low,,Mesh,vtx:456 idx:1911 uv:912 n:1824,,4260,,e9d2f24e17bc510d2b627520ab149e5e,sharedassets0.assets
button,,Texture2D,12x12,ARGB4444,404,,9999ba253411b30f30f33fb360dab1e3,Resources/default
button active,,Texture2D,12x12,RGBA32,700,,69d58ba7c15aedb7bec58d84826293aa,Resources/default
button hover,,Texture2D,12x12,ARGB4444,408,,39a14ee6b2de862a7627e85f6436be22,Resources/default
button on,,Texture2D,12x12,ARGB4444,408,,d1cdbbe0c16aff9b693d261b66e1d630,Resources/default
button on hover,,Texture2D,12x12,ARGB4444,412,,fe15605fca2f0fd5d3f56ae013b031ec,Resources/default
Capsule,,Mesh,vtx:550 idx:2496 uv:1100 n:1650,,36196,,eb6e9adcd5af6b8c90bdced7c02055fa,Resources/default
Combined Mesh (root: scene) 2,,Mesh,vtx:30360 idx:85401 uv:60720 n:121440,,172972,Mesh/\Combined_Mesh__root__scene__2_802.obj,,level0
```

第二步和第三步都在 [Python 端](https://github.com/taptap/pkg-doctor/blob/main/AssetStudio/pkg.py) 进行，这两部分的改动频率较高，随着我经手的游戏变多，新的优化方向会浮出水面，更多的用户反馈也会带来更友好的数据展示方式。

# 举个例子

这是一款用来测试工具的项目，我们来看下包体医生生成的报告中 `包体概览` 部分：

![]({{ site.url }}/media/image-20210626233147977.png)

从这里能看到包体的重头戏是 贴图 / Texture，而重中之重则是未经压缩的图片；此外还有不可忽视的重复入包资产。

知道这些信息后，我们可以查看 `重复入包 TOP 榜单` 找到出问题的资产名称、浪费的包体尺寸、预览、prefab 名称等等。

![]({{ site.url }}/media/image-20210626234645718.png)

然后是 `未压缩贴图 TOP 榜`，也是体贴地给出了资产名称、浪费的包体尺寸、预览、prefab 名称等等。

![image-20210626234935117]({{ site.url }}/media/image-20210626234935117.png)

对于这个测试项目，包体医生现有的功能便可以大幅地降低包体尺寸。

# 未来展望

下一阶段优先级最高的任务是增加对 Unreal 项目的支持，而由于我将解决问题的过程分成了三步

- 收集数据
- 处理数据
- 展示数据

第 2 步和第 3 步几乎是可以复用的。

----

vinjn.com - Democratizing Visual Computing