---
layout: post
title:  "Texture 是什么？"
tags: gpu opengl directx cuda vulkan
---

* content
{:toc}

Texture 的含义在不同的时代与上下文中有着不同的含义，待我细细说来:

- OpenGL 时代的 Texture 是一种名词
- D3D10+ 时代的 Texture 是另一种名词
- Vulkan 里的 Texture 是什么？
- 在 GPU 内部 Texture 既是名词也是动词
 



OpenGL 时代的 Texture 是一种名词
=

从应用开发者来看，texture 是一个名词，在物理上指的是 GPU 显存中一段连续的空间。

![]({{ site.url }}/media/gp104-mem.jpg)

这张图是一款 N 卡，最中间的是核心部分 GPU - GP104，texture 的物理位置就在 GPU 周围的 1-2-3-4-5-6-7-8 个显存颗粒里，这个 GPU 配着 8G 显存。
作为名词，texture 有着一些属性，比如宽度、高度、色彩通道数量，这部分属性和图片的属性相同的。

作为名词，texture 还有一些特殊的属性，比如 mipmap。当我们创建一张 texture 的时候，我们创建的通常是一组大小不一致的 texture。
![]({{ site.url }}/media/texture-mipmap.jpg)

这感觉就好像星巴克。

![]({{ site.url }}/media/starbucks-mipmap.jpg)

通常意义上 texture 是一个对 GPU 只读的资源，想写 texture 需要在它之上创建一个 fbo，这在逻辑上其实说不通。

![]({{ site.url }}/media/texture-fbo.jpg)


D3D10+ 时代的 Texture 是另一种名词
=

D3D10 中引入了 view 的概念，和 texture 相关的 view 有

- 只读的 view，SRV
- 可写的 view，RTV

![]({{ site.url }}/media/texture-srv-rtv.jpg)

这个创新点就很赞了，texture 本身变成了一个读写权限无关的资源，你既可以去读它，也可以去写它。
texture 依然是名词，SRV 和 RTV 是副词。View 这个名字取得很好——观点。创建了资源之后需要告诉 GPU 怎么去访问这个资源。

名词和副词，细细体会。


这里提出第一个令人困惑的观点，D3D10 的 SRV 其实是 opengl 里的 texture。既然 SRV 是副词，那么 texture 也应该是副词？

Vulkan 里的 Texture 是什么？
=
Vulkan 的函数里并没有 Texture 这个词，取而代之的是 Image 和 ImageView。

![]({{ site.url }}/media/texture-vulkan.jpg)

Vulkan 的编程模型里所有资源分为两种，Buffer 和 Image。
Image 是 D3D10 中的 Texture，但是逻辑上更贴近 GPU 的实现，以后有空再说。光有 Image 是不行的，和 D3D10 一样，需要 View，不过这里没有区分 SRV 和 RTV。针对 Image 的所有的 View，都叫做 ImageView。

Vulkan 里的编程模型里，已经没有 Texture 这个名词。OpenGL 时代的 Texture 和 FBO，现在变成了 Image 和 ImageView。

在 GPU 内部 Texture 既是名词也是动词
=

下图是 N 卡内部运行 shader 的核心单元——streaming multiprocessor （SM），每个 SM 内部有固定的子单元，就叫做 Tex。

![]({{ site.url }}/media/texture-in-sm.jpg)

怕你们不信，再看下 A 卡内部执行 shader 的单元 —— compute unit （CU），它也有固定单元执行类似指令。

![]({{ site.url }}/media/texture-in-cu.jpg)

所以对 GPU 而言 Texture 是名词，是实实在在的硬件单元。

但是我要提出第二个令人困惑的观点，Texture 还是动词，当 SM / CU 运行的 shader 中需要读写显存中的资源时，读操作也叫做 TEX。

![]({{ site.url }}/media/texture-ISA.jpg)


Texture 到底是什么？
=
它是名词、副词，也是动词，正确答案取决于上下文，以及是谁在问你。


----
vinjn.com - Democratizing Visual Computing
