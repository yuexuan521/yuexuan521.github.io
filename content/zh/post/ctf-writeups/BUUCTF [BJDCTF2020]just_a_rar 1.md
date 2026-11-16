---
title: "BUUCTF [BJDCTF2020]just_a_rar 1"
date: 2024-05-23 11:02:25
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190641317.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)

 ![题目页「［BJDCTF2020］just_a_rar 1」，附 GitHub 来源与 attachment.rar](/images/blog/20251228190643420.png)

### 题目描述：

来源： [https://github.com/BjdsecCA/BJDCTF2020](https://github.com/BjdsecCA/BJDCTF2020) 

### 密文：

下载附件，解压得到一个.rar压缩包。
 ![压缩包里的 4位数.rar，大小 95 KB](/images/blog/20251228190645009.png)

---

### 解题思路：

1、根据压缩包的名字提示我们使用4位纯数字进行破解。使用ARCHPR工具，选定参数，破解得到密码为5790。
 ![Advanced Archive Password Recovery 统计：总计口令 2,021、文件口令 2016、十六进制 32 30 31 36](/images/blog/20251228190646365.png)

用密码解压rar压缩包，得到一张名为flag的jpg图片。

![一张黑白插画，画的是海贼王里的青雉 KUZAN](/images/blog/20251228190647915.jpeg)

2、使用010Editor打开图片，查阅数据时，发现flag。（flag中的“W”要注意大写！）

 ![十六进制视图，107h 行后出现 flag{Wa.df.1.2.3}，紧接着是 tp://ns.adobe.com/xap/1.0/ 的 XMP 元数据](/images/blog/20251228190650180.png)

### flag：

```bash
flag{Wadf_123}
```