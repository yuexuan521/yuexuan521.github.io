---
title: "BUUCTF N种方法解决 1"
date: 2024-06-24 22:41:47
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "密码学"
- "笔记"
- "CTF"
- "Crypto"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190115330.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190117757.png)

### 题目描述：

下载附件，解压得到一个.exe文件

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190119418.png)

### 解题思路：

1、双击.exe文件，出现一个错误，切换其他的方法。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190120587.png)

2、将KEY.exe文件放到010Editor，分析这个文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190123212.png)

3、观察到jpg、base64等关键词，将base64后的字符串解码，点击另存为png文件。
在对base64的初次解码中，发现文件可能为png文件的线索。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190126013.png)

[https://the-x.cn/encodings/Base64.aspx](https://the-x.cn/encodings/Base64.aspx) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190153298.png)

4、打开下载的png文件，发现是一张二维码，使用QR Research扫描，得到flag。（通过手机扫码也可以得到flag，为扫码跳转网页的网址）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190155925.png)

### flag：

```bash
flag{dca57f966e4e4e31fd5b15417da63269}
```