---
title: "BUUCTF 基础破解 1"
date: 2024-09-24 22:43:42
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "密码学"
- "CTF"
- "Crypto"
- "BUUCTF"
- "笔记"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192811347.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192813583.png)

### 题目描述：

给你一个压缩包，你并不能获得什么，因为他是四位数字加密的哈哈哈哈哈哈哈。。。不对= =我说了什么了不得的东西。。

### 密文：

下载附件解压，发现一个rar压缩包。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192815192.png)

### 解题思路：

1、尝试解压rar压缩包，找到flag.txt文件，似乎flag就在里面，但需要解压密码。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192816582.png)

2、根据题目的提示，密码是四位数字。我们使用RARP来暴力破解这个rar压缩包，选择密码长度和字符集，可以大大减少破解所需要的时间。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192818399.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192820216.png)

3、破解出的压缩包密码为2563，使用密码解压flag.txt，得到一串经过base64加密的字符串。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192822257.png)

4、使用在线工具解码base64字符串，得到flag。
[在线工具](https://base64.supfree.net/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192823636.png)

### flag：

```bash
flag{70354300a5100ba78068805661b93a5c}
```