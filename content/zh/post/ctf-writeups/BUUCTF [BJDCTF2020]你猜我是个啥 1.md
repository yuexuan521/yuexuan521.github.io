---
title: "BUUCTF [BJDCTF2020]你猜我是个啥 1"
date: 2024-09-22 11:05:33
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "MISC"
- "笔记"
- "CTF"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190704638.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)

 ![题目页「［BJDCTF2020］你猜我是个啥 1」，附 GitHub 来源与 attachment.zip](/images/blog/20251228190706870.png)

### 题目描述：

来源： [https://github.com/BjdsecCA/BJDCTF2020](https://github.com/BjdsecCA/BJDCTF2020) 

### 密文：

下载附件，得到一个zip压缩包。

![attachment_10.zip，大小 2 KB](/images/blog/20251228190708523.png)

---

### 解题思路：

1、尝试解压压缩包，提示“attachment_10.zip”不是压缩文件。结合题目，猜测更改了文件格式，放到010 Editor看一下，发现是png文件。
 ![Bandizip 报错「attachment_10.zip 不是压缩文件」](/images/blog/20251228190709940.png)

![十六进制视图开头是 89 50 4E 47，红框标出 PNG 字样](/images/blog/20251228190711375.png)

2、将文件后缀修改为.png，再次打开文件，得到一张二维码图片。

![把后缀改成 png 后打开，是一张二维码](/images/blog/20251228190713162.png)

使用QR research扫描二维码，得到提示“flag不在这”。
 ![二维码解码只得到一句「flag不在这」](/images/blog/20251228190714709.png)

3、我们返回010Editor中，在文件的最后找到了flag。（一开始得到的压缩包在010 Editor中也可以找到这个flag。）

 ![IEND 之后还跟着一段数据，红框里能看到 {i_am_fl@g}](/images/blog/20251228190716301.png)

### flag：

```bash
flag{i_am_fl@g}
```