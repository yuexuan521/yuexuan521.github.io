---
title: "BUUCTF john-in-the-middle 1"
date: 2024-09-21 20:46:52
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "安全"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185935089.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185937188.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到john-in-the-middle.pcap文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185938725.png)

---

### 解题思路：

1、双击文件，打开wireshark。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185940117.png)

看到很多http流量，导出文件看一下。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185942605.png)

导出得文件如下。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185944922.png)

2、将图片放到StegSolve中，在logo.png文件的多个通道发现flag，大多数不是很清晰，有一个通道比较清晰，得到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185946799.png)

还有一个方法：
观察发现logo.png图片中间有条缺口
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185948571.png)

而使用stegslove打开scanlines.png，在很多通道都可以发现有一条线

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185950267.png)

将两张图片在stegslove进行ImageCombiner对比（条件比较苛刻，需要先用stegslove打开scanlines.png文件，在此基础上与logo.png进行Image Combiner）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185951556.png)

### flag：

```bash
flag{J0hn_th3_Sn1ff3r}
```