---
layout: post
title: "#纯干货#加速 VR 渲染地狱难度进阶篇之降低图形 API 调用次数"
tags: virtual-reality directx
---

* content
{:toc}

![]({{ site.url }}/media/vr-oculus-header.jpg)

准备好，中文互联网终于有人开始严肃地讨论 VR 技术了。纯干货文章，直奔主题。

VR 渲染的难点是什么？
=

![]({{ site.url }}/media/vr-stereo.jpg)

左眼和右眼看到的场景是不同的，因此同样的 API 需要调用两次。以 Oculus DK2 为例，头戴显示器分辨率为 1920 X 1080，viewport 均分为左右两部分，各为 960 X 1080。这意味着 CPU 和 GPU 的工作量相比传统三维游戏都要增加，如何降低额外的一次绘制的开销是这篇文章的重点。

![]({{ site.url }}/media/vr-viewport.jpg)

VR 地狱有几层？
=
先来四层：

- 整个场景画两次：先绘制左眼看到的场景，然后绘制右眼看到的场景。
- 每个物体画两次：对于场景中的物体依次绘制，物体 1 左眼 -> 物体 1 右眼 -> 物体 2 左眼 -> 物体 2 右眼 -> 按照左右左右的顺序绘制。
- 硬件 Geometry Shader 技术：只绘制一次，用硬件 Geometry Shader 生成左右两个场景。
- 硬件 Instancing 技术：只绘制一次，用 instancing 技术生成左右两个场景。




第一层：整个场景画两次
=
优点 － 最容易实现

缺点 － 效率低下，不论是 CPU 还是 GPU

![]({{ site.url }}/media/vr-stereo-1.jpg)

左眼渲染结束后，同样的 API 在右眼部分依然需要重复。

第二层：每个物体画两次
=
优点 － 容易实现，可显著降低 API 调用，贴图、Shader、顶点缓存等不用修改。

SV_ViewportIndex 可以用于选择多个 Viewport。

缺点 － draw call 依然存在重复。

![]({{ site.url }}/media/vr-stereo-2.jpg)

仅仅需要修改右眼的摄像机的状态（View Matrix）和 视口（Viewport）。

第三层：硬件 Geometry Shader 技术
=
优点 － 看上去很美，硬件加速，不需要重复 API 调用。

缺点 － 需要修改 Shader 流水线以支持 GS，同时部分硬件平台不支持 GS，而且 GS 的效率非常糟糕（某显卡：怪我咯）！

![]({{ site.url }}/media/vr-stereo-3.jpg)

第四层：硬件 Instancing 技术
=
优点 － 不涉及额外的状态切换，比 GS 快三倍！

缺点 － 不支持两只眼睛渲染到不同到 render target 上（暂时不是问题），同时老版本的 GLES 不支持 Instancing。

Instancing 的特点是 CPU 只上传一次顶点数据，让 GPU 批量渲染多次。也就是场景中所有物体的 Vertex Buffer、Index Buffer、Texture 以及渲染状态只设置一次，一个物体也仅仅 draw 一次。

但是前面提到左右眼的 View Matrix 和 Viewport 是不同的，只 draw 一次怎么解决这个问题呢？

```C++
float4x4 WorldToEyeClipMatrix[2]; // 由 VR 设备的 SDK 获取
float4 EyeClipEdge[2]={(-1,0,0,1), (1,0,0,1)};
float EyeOffsetScale[2]={0.5,-0.5};
uint eyeIndex = SV_InstanceID & 1; // 最低位作为眼睛的索引值：0 为左眼，1 为右眼。

float4 clipPos = worldPos * WorldToEyeClipMatrix[eyeIndex];
SV_CullDistanceOut.x = SV_ClipDistanceOut.x = clipPos · EyeClipEdge[eyeIndex];

clipPos.x *= 0.5; // 渲染范围的宽度缩小一半
clipPos.x += EyeOffsetScale[eyeIndex] * clipPos.w; // 根据是左眼 or 右眼来向左 or 向右移动
clipPositionOut = clipPos;
```

正确答案是这段 HLSL Vertex Shader 伪代码，详细的解释留待下一篇文章。

总共十八层的地狱，小张只探索到第四层，等大家打怪练级经验涨上去了，后续关卡会依次开放。

​本文部分选自 <Timothy Wilson - High performance stereo rendering for VR>

----
vinjn.com - Democratizing Visual Computing
