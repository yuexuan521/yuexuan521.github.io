---
title: "BUUCTF [BJDCTF2020]你猜我是个啥 1"
date: 2024-09-23 11:05:33
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "MISC"
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190704638.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190706870.png)

### 题目描述：

来源： [https://github.com/BjdsecCA/BJDCTF2020](https://github.com/BjdsecCA/BJDCTF2020) 

### 密文：

下载附件，得到一个zip压缩包。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190708523.png)

---

### 解题思路：

1、尝试解压压缩包，提示“attachment_10.zip”不是压缩文件。结合题目，猜测更改了文件格式，放到010 Editor看一下，发现是png文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190709940.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190711375.png)

2、将文件后缀修改为.png，再次打开文件，得到一张二维码图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190713162.png)

使用QR research扫描二维码，得到提示“flag不在这”。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190714709.png)

3、我们返回010Editor中，在文件的最后找到了flag。（一开始得到的压缩包在010 Editor中也可以找到这个flag。）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190716301.png)

### flag：

```bash
flag{i_am_fl@g}
```