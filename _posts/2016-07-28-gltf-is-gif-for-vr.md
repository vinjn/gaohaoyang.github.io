---
layout: post
title:  "你们最爱的微信表情包，到了 VR 时代会变成什么样？"
categories: glTF
tags:  Oculus glTF Khronos
---

![]({{ site.url }}/media/ge-you-tan-vr.jpg)

为何需要一种新格式？
=
互联网时代的表情包或者是静态的 jpg，或者是动态的 gif。

![]({{ site.url }}/media/cat-vr.gif)

人类进入 VR 时代后，打开 WeChatVR，输入的表情包也得是真三维的内容，转个身能看到不一样的风景。比如这样：

![]({{ site.url }}/media/cat-3d-hand.gif)




这就给段子手带来了一个难题，如何制作、传播这种三维的内容？

互联网上的内容交换格式不能是商业公司私有的格式，jpg 和 gif 都是开放的格式，开放的格式得到各种应用软件的支持。你在电脑浏览器里看到的图片，可以另存为到手机上，可以转载到朋友圈。

VR 时代也需要一种开放的、无处不在的三维格式。

热衷于制定图形领域工业标准的 Khronos Group（克洛诺斯组织）看到了这个机会，搞了个大新闻：

![]({{ site.url }}/media/gltf-logo.jpg)

glTF - GL Transmission Format，也就是为 OpenGL / WebGL 设计的交换格式。

这个格式得到了 VR 届的 VIP、Oculus CTO 卡马克的支持。

![]({{ site.url }}/media/gltf-john-carmack.jpg)

卡马克看的更远，看到了 VR 的下一个阶段 metaverse，一个超越了 AR、VR、互联网的虚拟世界。metaverse 是由 Neal Stephenson 在《雪崩》（Snow Crash）一书中描述的平行世界，所有现实世界中的人在 metaverse 中都有一个化身。

让你们感受下 glTF
=

一个 gltf 的三维模型由四种文件格式组成，gltf、bin、glsl、jpg / png。
.gltf，由 JSON 格式描述三维模型的内部结构、虚拟相机、材质等，人类可以阅读，长成这样：

```json
"accessor_21": {
    "bufferView": "bufferView_29",
    "byteOffset": 0,
    "byteStride": 0,
    "componentType": 5123,
    "count": 36,
    "type": "SCALAR"
},
"accessor_23": {
    "bufferView": "bufferView_30",
    "byteOffset": 0,
    "byteStride": 12,
    "componentType": 5126,
    "count": 24,
    "max": [
        0.5,
        0.5,
        0.5
    ],
    "min": [
        -0.5,
        -0.5,
        -0.5
    ],
    "type": "VEC3"
},
```

- .bin，描述顶点信息和动画信息，人类无法阅读的二进制文件，长这样：

![]({{ site.url }}/media/matrix-1010.jpg)

- .glsl，shader 图形着色语言，长这样：

![]({{ site.url }}/media/jacky-chen-wtf.jpg)

- 用作三维贴图的图片格式，对不起，我欺骗了你们，到了 VR 时代依然需要 jpg。

glTF 的生态圈已经成熟
=

从图中可以看到，生态上得到了三维建模软件霸主 Autodesk 以及开源软件 Blender 的支持，FBX、Collada、OBJ 等格式都可以导出到 glTF，开源的 C++ 库方面 Assimp 也已经支持 glTF 到导入和导出。渲染引擎方面第一时间支持 glTF 的是 WebGL 引擎，包括最受欢迎的 three.js。

行业方面，在这周的的 SIGGRAPH 图形学大会上，Oculus 也展示了 glTF 在 Mobile VR 中的重要性：

![]({{ site.url }}/media/gltf-oculus.jpg)

NVIDIA 的开源渲染引擎 nvpro-pipeline 也于这周加入了 glTF 的支持。
不论你是 AR、VR 的设备的提供商或是内容制作商，都可以把支持 glTF 放进 roadmap 了，这是现在看来最有未来的内容格式。

----
vinjn.com - Democratizing Visual Computing
