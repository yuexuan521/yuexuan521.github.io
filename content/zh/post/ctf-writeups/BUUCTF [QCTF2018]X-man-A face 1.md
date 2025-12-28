---
title: "BUUCTF [QCTF2018]X-man-A face 1"
date: 2025-06-09 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "BUUCTF"
- "CTF"
- "misc"
- "QCTF2018"
- "X-man-A face"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191711309.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191713887.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源：https://github.com/hebtuerror404/CTF_competition_warehouse_2018

### 密文：

下载附件解压，得到Xman-Aface-61df10385eaccbb3627ca3926c6ae174.png

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191715886.png)

---

### 解题思路：

1、既然二维码是缺失的，第一反应去补全二维码。缺失的部分是定位图案，复制仅存的定位图案，粘贴在缺失的地方，用PS。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191718209.png)

操作得到，完整二维码如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191719863.png)

2、扫码得到一串密文。（我的QR_Research好像坏了，用手机夸克扫出来的，跟上一题一样）

```python
KFBVIRT3KBZGK5DUPFPVG2LTORSXEX2XNBXV6QTVPFZV6TLFL5GG6YTTORSXE7I=
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191722193.png)

观察密文特征，确定为Base32编码，解明文得到flag： `QCTF{Pretty_Sister_Who_Buys_Me_Lobster}` 。

[Base32编码解码](https://www.qqxiuzi.cn/bianma/base.php) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191725191.png)

有故事的出题人

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191726792.png)

### flag：

```bash
flag{Pretty_Sister_Who_Buys_Me_Lobster}
```