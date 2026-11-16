---
title: "BUUCTF N种方法解决 1"
date: 2024-06-24 22:41:47
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "密码学"
- "笔记"
- "CTF"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190115330.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「N种方法解决 1」，附件 f64ca6fa-11…](/images/blog/20251228190117757.png)

### 题目描述：

下载附件，解压得到一个.exe文件

### 密文：

![文件名是 KEY.exe](/images/blog/20251228190119418.png)

### 解题思路：

1、双击.exe文件，出现一个错误，切换其他的方法。

![双击弹出「此应用无法在你的电脑上运行」，说明它并不是真正的 PE](/images/blog/20251228190120587.png)

2、将KEY.exe文件放到010Editor，分析这个文件。

![文本编辑器打开，开头是 data:image/jpg;base64,iVBORw0KGgoAAAANSUhEUgAA…](/images/blog/20251228190123212.png)

3、观察到jpg、base64等关键词，将base64后的字符串解码，点击另存为png文件。
在对base64的初次解码中，发现文件可能为png文件的线索。

![Base64 在线解码，上面显示 PNG 的十六进制，底部提示原本 3848 字节、解码后 5548 字节](/images/blog/20251228190126013.png)

[https://the-x.cn/encodings/Base64.aspx](https://the-x.cn/encodings/Base64.aspx) 

![hex 视图里插件识别出「PNG Image，尺寸: 133x133」](/images/blog/20251228190153298.png)

4、打开下载的png文件，发现是一张二维码，使用QR Research扫描，得到flag。（通过手机扫码也可以得到flag，为扫码跳转网页的网址）

![QR Research 解出 KEY{dca57f966e4e4e31fd5b15417da63269}](/images/blog/20251228190155925.png)

### flag：

```bash
flag{dca57f966e4e4e31fd5b15417da63269}
```