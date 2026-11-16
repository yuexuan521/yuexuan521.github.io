---
title: "BUUCTF 镜子里面的世界 1"
date: 2024-09-24 16:41:38
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
- "隐写"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228193600848.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「镜子里面的世界 1」，附件 a74988d1-9…](/images/blog/20251228193602837.png)

### 题目描述：

下载附件，解压得到一张.png图片。

### 密文：

![一张放大镜图片，镜片里写着 Look very closely ;)](/images/blog/20251228193604371.png)

### 解题思路：

1、材料只有一张图片，题目提示“镜子里面的世界”结合图片中的英文“look very closely”（翻译为“仔细观察”，暗示LSB隐写），认为是图片隐写中的LSB隐写。（其实，我是写出来后，才反推出他给出的线索是什么意思。）
[LSB例题，里面有LSB原理](https://blog.csdn.net/YueXuan_521/article/details/134053293?spm=1001.2014.3001.5502) 

2、将图片放到StegSolve中，然后打开Analyse（分析）选项卡，使用Data Extract（数据提取）选项，开始分析，得到提示信息。

![Stegsolve 的 Extract Preview：Bit Planes 勾选 RGB 的 0 位，右侧预览出一段英文](/images/blog/20251228193606492.png)

3、将提示的信息翻译过来，就找到了flag。

![Google 翻译：the secret key is : st3g0_saurus_wr3cks 译成中文](/images/blog/20251228193608333.png)

### flag：

```bash
flag{st3g0_saurus_wr3cks}
```