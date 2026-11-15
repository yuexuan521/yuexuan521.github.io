---
title: "BUUCTF LSB 1"
date: 2024-06-24 22:45:48
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190005317.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「LSB 1」，附件 72f46f3b-8a…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190007338.png)

### 题目描述：

下载附件，解压得到一张png图片。

### 密文：

![中国矿业大学的圆形校徽，下方写着 1909](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190008906.png)

### 解题思路：

1、根据题目的提示，这道题涉及LSB隐写。使用StegSolve工具打开flag11.png文件，打开Analyse（分析）选项卡，使用Data Extract（数据提取）选项，进行分析。

![Stegsolve 的 Analyse 菜单，高亮 Data Extract](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190010664.png)

[LSB原理](https://blog.csdn.net/qq_38154820/article/details/122694645) 
2、提取Red，Green和Blue的0通道信息，在这三个颜色的0通道上打勾，并按下Preview键。

![Data Extract 面板：勾选 Red、Green、Blue 的 0 位，Extract By Row、LSB First、RGB 顺序](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190016635.png)

（拉到最上面）当隐写的内容为图片时如下所示：

![Extract Preview 的十六进制开头是 89 50 4E 47，即 PNG 文件头](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190018683.png)

由PNG文件头可以看出隐写内容为PNG文件，按save Bin键保存为PNG文件。

3、得到一张二维码图片，通过扫描二维码，获得flag。

![Save Bin 存出来的图片，是一张二维码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190026032.png)

![二维码解码结果：cumtc{1sb_i4_s0_Ea4y}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190027655.png)

### flag：

```bash
flag{1sb_i4_s0_Ea4y}
```