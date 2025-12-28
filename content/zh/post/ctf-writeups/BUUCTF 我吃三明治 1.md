---
title: "BUUCTF 我吃三明治 1"
date: 2024-08-21 20:50:33
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193005917.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193007997.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到一张.jpg图片。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193009640.jpeg)

---

### 解题思路：

1、使用010Editor打开.jpg文件，在.jpg文件尾的位置发现了第二张图片，以及夹在两张jpg图片之间的一串Base32编码的字符。

```bash
MZWGCZ33GZTDCNZZG5SDIMBYGBRDEOLCGY2GIYJVHA4TONZYGA2DMM3FGMYH2
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193011225.png)

2、使用在线网站进行解密，得到flag。

[Base32编码解码](https://www.qqxiuzi.cn/bianma/base.php)  ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193013271.png)

```bash
"flag"经过Base32编码后是”MZWGCZY=”。
```

### flag：

```bash
flag{6f1797d4080b29b64da5897780463e30}
```