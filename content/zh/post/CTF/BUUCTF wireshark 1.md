---
title: "BUUCTF wireshark 1"
date: 2024-08-24 22:42:44
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "1024程序员节"
- "笔记"
- "密码学"
- "网络安全"
- "crypto"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190404475.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190406495.png)

### 题目描述：

黑客通过wireshark抓到管理员登陆网站的一段流量包（管理员的密码即是答案)

### 密文：

下载附件，解压后得到一个.pcap文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190408043.png)

### 解题思路：

1、双击文件，在wireshark打开。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190409344.png)

2、分析题目，需要找到管理员登陆网站的流量，并从中找到登录密码。使用“http.request.method==POST”语句过滤流量包，得到一条登录流量。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190411511.png)

3、分析这条流量，在数据包详细信息中找到登陆密码“password”，得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190413288.png)

### flag：

```bash
flag{ffb7567a1d4f4abdffdb54e022f8facd}
```