---
title: "BUUCTF 金三 1"
date: 2024-09-24 22:36:19
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "笔记"
- "密码学"
- "BUUCTF"
- "crypto"
- "CTF"
- "网络安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193543814.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193545912.png)

### 题目描述：

只有一个附件，下载下来有一张GIF图片。

### 解题思路：

本题一共有2种解法（本人找到的）

#### 方法一：

1、打开这张GIF图片，观察到不正常闪动，似乎有东西藏在图片中。
2、使用StegSolve工具，对图片进行逐帧查看。

在StegSolve中打开GIF图片

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193547762.png)

打开逐帧查看

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193549564.png)

在这里可以逐帧查看图片（理论上可以在这里看到逐帧的图片，但我这显示不出来）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193551191.png)

3、找到三张带有flag的图片，拼在一起得到flag。(中间的那张图片“he11o”，中间是两个数字1)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193552982.bmp)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193554839.bmp)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193556699.bmp)

#### 方法二：

1、使用Photoshop打开GIF图片，也可以逐帧查看图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193558520.png)

### flag：

```bash
flag{he11ohongke}
```