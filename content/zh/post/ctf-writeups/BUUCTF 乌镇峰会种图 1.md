---
title: "BUUCTF 乌镇峰会种图 1"
date: 2023-10-24 22:38:05
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "1024程序员节"
- "笔记"
- "密码学"
- "BUUCTF"
- "CTF"
---

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

![题目页「乌镇峰会种图 1」，附件 b56cde4f-be…](/images/blog/20251228204951651.png)

**题目描述：** 
乌镇互联网大会召开了，各国巨头汇聚一堂，他们的照片里隐藏着什么信息呢？（答案格式：flag｛答案｝，只需提交答案）
**密文：** 
下载附件，得到一张图片

![乌镇世界互联网大会会场照片，墙面上是世界地图与一排旗阵](/images/blog/20251228204954303.jpeg)

**解题思路：** 
1、分析题目，题目类型为图片隐写。尝试使用StegSolve对图片做各种分析，没有什么结果。
2、尝试010Editor，将图片放入010 Editor进行分析，翻到最后，找到flag。（可以用记事本打开，也可以找到flag）

![010 Editor 十六进制视图，红框标出 FF D9 之后紧跟 flag{97314e7864a8f62627b26f3f998c37f1}](/images/blog/20251228204956121.png)

![编辑器里大段乱码，末尾露出同一串 flag，状态栏显示行 186](/images/blog/20251228204957839.png)

**flag：** 

```bash
flag{97314e7864a8f62627b26f3f998c37f1}
```