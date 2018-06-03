---
layout: post
title:  "青年问禅师之是否应该学 Vulkan"
categories: Vulkan
tags:  Vulkan OpenGL
---

![]({{ site.url }}/media/master-and-noob.jpg)

小白：在嘛？

禅师：在，请说。

小白：听说 Vulkan 是来自未来的图形 API，毁天灭地让农企翻身做主人。

禅师：额。。。

小白：我想咨询下我是否应该学 Vulkan。

禅师：你读过《提问的智慧》么？

小白：没读过。

禅师：下一位。




----

小黄：我读过《提问的智慧》。

禅师：好吧，你学 Vulkan 的动机是什么？

小黄：我想学习图形编程（graphics programming）

禅师：慢着别动，先从简单点的 API 入手，比如 OpenGL。如果市面上有合适的 Vulkan 库，那么也可以试试。从零开始学 Vulkan 不建议。

禅师：下一位。

----

小红：我是一名有好奇心的程序员，我懂图形编程想再学点。

禅师：good，但是提醒你在 Vulkan 里画个三角形要一百行代码，不过在这过程中你能学到很多 GPU 的知识。

禅师：祝你好运。

----

小绿：我有这么一个 OpenGL 的程序。

禅师：它运行得太慢 / 太耗电？

小绿：没有，跑得挺欢的。但是我想体验最新的 GPU 特性，比如 VR 渲染。

禅师：你想体验的 OpenGL 都能满足，等新显卡的诞生了，显卡商也会支持新的 OpenGL 扩展。别浪费时间重新造轮子，你又不是轮子哥。

禅师：下一位。

----

小粉：我有这么一个 OpenGL 的程序，很卡。

禅师：瓶颈在 CPU 上么？

小粉：不是，但是延迟（latency）太高。

禅师：驱动（driver）为了增加渲染运算的吞吐量（throughput），会缓存多帧（frame）的工作量。Vulkan 给你更多的自由度，可以显示控制 swap chain。

禅师：祝你好运。

----

小紫：我有这么一个 OpenGL 的程序，非常卡。

禅师：瓶颈在 CPU 上么？

小紫：是的。

禅师：是因为驱动的原因？

小紫：不是驱动问题。

禅师：那就是你的问题咯…… Vulkan 会降低驱动的负担，如果瓶颈不在驱动上 Vulkan 也帮不了你。

禅师：下一位。

----

小灰：我有这么一个 OpenGL 的程序，非常卡，瓶颈在 CPU 上。

禅师：你听说过 AZDO 嘛？

小灰：没，我只听说过安利。

禅师：AZDO，就是 Approaching Zero Driver Overhead 的简称，是一组专门降低 CPU 消耗的 OpenGL 函数，试试吧。

禅师：下一位。

----

小灰：我有这么一个 OpenGL 的程序，非常卡，瓶颈在 CPU 上。

禅师：你听说过 AZDO 嘛？

小灰：听过，我是小灰啊，我已经试过 AZDO 了，好用是好用，但还不够给力。

禅师：让你只支持 N 卡，你肯么？

小灰：我是 N 粉！

禅师：试试 ARB_bindless_texture 和 NV_command_list。

禅师：下一位。

----

小张：我有这么一个 OpenGL 的程序，非常卡，瓶颈在 CPU 上，试过 AZDO。

禅师：让你只支持 N 卡，你肯么？

小张：NOOOOOOOOOOOOOO

禅师：恭喜你，Vulkan 适合你！

---
本文改编自 https://twitter.com/renderpipeline/status/699346348651507713

原作者：Robert Menzel

----
http://vinjn.com - 关注人工智能+视觉计算