---
layout: post
title: "《光线追踪精粹》译者序"
tags: rtx nvidia
---

![]({{ site.url }}/media/typewriter.jpg)

本书终于和大家见面了。

小张依然会记得 10 多年前，作为图形学初学者在书店遇到《GPU 精粹》三部曲时的兴奋感。彼时固定管线还在向可编程管线转变，shader 是个新鲜概念，CUDA 更是尚未诞生。NVIDIA 深知新生事物在萌芽期需要额外呵护，因此召集行业精英撰写了这三本书，将业界最先进的技术和思想进行传播。文章质量之高，使得书名中的精粹一词当之无愧，其中部分文章到了今日依然有着参考意义。

后来有幸加入 NVIDIA 并工作了八年，见证了 RTX 这个疯狂的想法从萌芽、质疑到问世。全球各地工程师、研究员的智慧与协作使得 RTX ON 成为一种信仰。基于同样的逻辑，NVIDIA 也召集全球的大牛们撰写出了《光线追踪精粹》。

现在可能是开始学习光线追踪最好的时间点。经过 NVIDIA 两代显卡的普及，消费者开始接受并拥抱光追。桌面端的竞争对手们纷纷认可这个方向，甚至手机端也有厂商选择跟进。我在两年前决定翻译本书，也是希望能在合适的时间点对国内的图形行业做一些微小的贡献。

我在光线追踪领域涉猎广但是不深，有幸的是在本书的初稿阶段得到了众多网友的帮助。在此由衷地感谢参与翻译的伙伴们付出的劳动，在初稿之后我们又进行了多轮交叉审稿，最终才得以成稿。以下是本次参与翻译的小伙伴（按 github 账号首字母）：AltmIce 张鹏飞、AmesingFlank 卢敦凡、Angiewei 魏安琪、bluesummer 王晓波、butterfly0923 张帅、ch3coohlink、chiantiYZY 颜卓仪、Cielrin、FaithZL  朱翎、FantasyVR 禹鹏、gigichq、haiyuem 麻海玥、jingjingshenye、lifangjie 李方杰、LIJIE 李杰、liyongnupt 李勇、Nicholas10128 金鑫、papalqi 黄琦、qingqhua 华清沁、RazorYhang 杨宇辰、renyuhuiharrison 任宇晖、rockyvon、skyfeiyun 董东、slongle 邓俊辰、tankijong 张天毅、tavechiang 蒋大为、vcl-pku 马雷、wubochang 吴博昌、XBOOS、YanFeiGao 刘龙腾、yuxing5555、zhan2586 张闻宇、zheng95z 曾峥、zzk224 钟凯。

此外需要特别感谢 papalqi 黄琦在整个翻译前中后期的大力支持，他负责了与初稿译者们的沟通以及前中期进度的追踪。也要一并感谢 butterfly0923 张帅与 AltmIce 张鹏飞，他们带来了高质量的初稿并替我分担了后期繁重的审阅工作。此外要感谢本书编辑喻永光的支持和督（cui）促（gao），这是我俩的第二次合作，非常期待下一次。

感谢读者们的耐心等待。本书涵盖的内容之广，话题之深远远超过我立项时的估计，即便反复审稿也不免会有错误之处。也希望大家能多多谅解，遇到 bug 可以发我邮件 vinjn@qq.com。

最后要感谢妻子 M 和女儿 BLB 的支持，你们的微笑给了我深夜肝书的动力。

----

vinjn.com - Democratizing Visual Computing