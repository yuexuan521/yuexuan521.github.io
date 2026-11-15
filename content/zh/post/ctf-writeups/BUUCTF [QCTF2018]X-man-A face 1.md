---
title: "BUUCTF [QCTF2018]X-man-A face 1"
date: 2025-06-09 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191711309.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［QCTF2018］X-man-A face 1」，附 GitHub 来源与 attachment.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191713887.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源：https://github.com/hebtuerror404/CTF_competition_warehouse_2018

### 密文：

下载附件解压，得到Xman-Aface-61df10385eaccbb3627ca3926c6ae174.png

 ![一张马赛克脸，中央贴着二维码，底部写着「一脸懵逼」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191715886.png)

---

### 解题思路：

1、既然二维码是缺失的，第一反应去补全二维码。缺失的部分是定位图案，复制仅存的定位图案，粘贴在缺失的地方，用PS。

 ![二维码结构示意图，标注「定位图案」「数据范围」「码元」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191718209.png)

操作得到，完整二维码如下：

 ![补齐左上定位图案后的二维码，三个角都有定位框了](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191719863.png)

2、扫码得到一串密文。（我的QR_Research好像坏了，用手机夸克扫出来的，跟上一题一样）

```python
KFBVIRT3KBZGK5DUPFPVG2LTORSXEX2XNBXV6QTVPFZV6TLFL5GG6YTTORSXE7I=
```

 ![搜索框里是一长串 Base32：LTORSXEX2XNBXV6QTVPFZ…TORSXE7I=](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191722193.png)

观察密文特征，确定为Base32编码，解明文得到flag： `QCTF{Pretty_Sister_Who_Buys_Me_Lobster}` 。

[Base32编码解码](https://www.qqxiuzi.cn/bianma/base.php) 

 ![Base32 解码工具输出 QCTF{Pretty_Sister_Who_Buys_Me_Lobster}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191725191.png)

有故事的出题人

 ![翻译工具把它译成「给我买龙虾的漂亮妹妹」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191726792.png)

### flag：

```bash
flag{Pretty_Sister_Who_Buys_Me_Lobster}
```