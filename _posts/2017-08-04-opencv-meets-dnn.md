---
layout: post
title:  "鱼和熊掌兼得，DNN加入 OpenCV 全家桶"
categories: OpenCV
tags:  OpenCV DNN Caffe TensorFlow
---

刚刚出炉的 OpenCV 3.3 版本增加了符合历史进程的新模块 dnn，也就是深度神经网络。dnn 模块其实很早就存在于 opencv_contrib 项目中，这次提到了主项目里，算是转正了。

小张琢磨了下，这个 dnn 模块的卖点有这么几个：

* content
{:toc}




零依赖
===
只依赖 opencv，如果你被 caffe 虐过，那么就知道零依赖是多么如沐清风。

支持导入其他框架的模型
===
- Caffe 1
- TensorFlow
- Torch/PyTorch

以下网络是经测试可用的：

- AlexNet
- GoogLeNet v1 (also referred to as Inception-5h)
- ResNet-34/50/...
- SqueezeNet v1.1
- VGG-based FCN (semantical segmentation network)
- ENet (lightweight semantical segmentation network)
- VGG-based SSD (object detection network)
- MobileNet-based SSD (light-weight object detection network)

API 极其简洁
===
以 GoogLeNet 为例，载入一个网络模型只需要一个函数

```C++
#include <opencv2/dnn.hpp>
Net net = dnn::readNetFromCaffe("bvlc_googlenet.prototxt", "bvlc_googlenet.caffemodel");
```
准备网络的输入数据，GoogLeNet 只接受 224x224 的 RGB 图片，所以多了转尺寸的步骤
```C++
Mat img = imread("image.jpg");
Mat inputBlob = blobFromImage(img, 1, Size(224, 224),
                              Scalar(104, 117, 123));
```
跑一遍网络，取输出结果
```C++
Mat prob = net.forward("prob");
```
结束了，就这么简单！


然而它也有一个致命的缺点，只能载入训练好的网络跑一遍 forward pass，不能 train 新的网络。

那有没有办法用 opencv 训练网络呢，答案当然是：

![]({{ site.url }}/media/to-be-continued.jpg)

【黑客与画家】@Vinjn张静 的原创技术专栏，关注人工智能 + 视觉计算。
