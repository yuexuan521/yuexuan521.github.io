---
title: "BUUCTF 隐藏的钥匙 1"
date: 2024-09-24 16:30:17
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "笔记"
- "网络安全"
- "隐写"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193626662.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「隐藏的钥匙 1」，One Piece 主题，附件 8f6577ab-40…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193629181.png)

### 题目描述：

路飞一行人千辛万苦来到了伟大航道的终点，找到了传说中的One piece，但是需要钥匙才能打开One Piece大门，钥匙就隐藏在下面的图片中，聪明的你能帮路飞拿到钥匙，打开One Piece的大门吗？

### 密文：

下载附件，解压得到一张.jpg图片。

![路飞戴草帽竖大拇指的壁纸，左下角是 ONE PIECE 标志](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193631074.jpeg)

---

### 解题思路：

1、使用StegSolve查看图片，点击File Format选项卡，但我的StegSolve到这一步就卡死了。无奈使用010 Editor查看图片，然后在浏览的过程中，我找到了经过base64编码的flag值。

![十六进制视图，FF D9 之后跟 2A…，右侧可见 ..flag:base64:(Mzc3Y2Jh…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193632955.png)

2、复制括号内的字符，用在线工具进行base64解码，得到flag。
[BASE64加密解密
](https://base64.supfree.net/) 

![BASE64 工具里输入 377cbadda1eca2f2f73d36277781f00a 并点解密](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193635193.png)

### flag：

```bash
flag{377cbadda1eca2f2f73d36277781f00a}
```