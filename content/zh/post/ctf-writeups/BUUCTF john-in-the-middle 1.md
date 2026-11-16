---
title: "BUUCTF john-in-the-middle 1"
date: 2024-06-21 20:46:52
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185935089.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「john-in-the-middle 1」，附件 e307bf8b-1c…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185937188.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到john-in-the-middle.pcap文件。
 ![john-in-the-middle.pcap，图标上印着 010101](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185938725.png)

---

### 解题思路：

1、双击文件，打开wireshark。

 ![包列表里源和目的都是 192.30.252.154，GET / 与 GET /css/bootstrap.min.css](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185940117.png)

看到很多http流量，导出文件看一下。

 ![文件 → 导出对象 → HTTP...](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185942605.png)

导出得文件如下。

 ![导出列表里一堆资源：bootstrap.min.css、cini.png、codef_3d.js、logo.png、repy_cv.png、scanlines.png、texture.png](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185944922.png)

2、将图片放到StegSolve中，在logo.png文件的多个通道发现flag，大多数不是很清晰，有一个通道比较清晰，得到flag。

 ![把 logo.png 放大，左下角显出 flag{J0hn_th3_Sn1ff3r}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185946799.png)

还有一个方法：
观察发现logo.png图片中间有条缺口
 ![logo.png 原图只是黑三角加一面蓝旗](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185948571.png)

而使用stegslove打开scanlines.png，在很多通道都可以发现有一条线

 ![scanlines.png 打开是全黑，中间一条细横线](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185950267.png)

将两张图片在stegslove进行ImageCombiner对比（条件比较苛刻，需要先用stegslove打开scanlines.png文件，在此基础上与logo.png进行Image Combiner）

 ![StegSolve 里看 logo.png，红绿蓝平面各留残影，左下还是 flag{J0hn_th3_Sn1ff3r}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185951556.png)

### flag：

```bash
flag{J0hn_th3_Sn1ff3r}
```