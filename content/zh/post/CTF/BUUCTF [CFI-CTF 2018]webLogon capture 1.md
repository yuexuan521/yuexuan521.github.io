---
title: "BUUCTF [CFI-CTF 2018]webLogon capture 1"
date: 2025-07-07 08:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "网络安全"
- "安全"
- "CFI-CTF"
- "wireshark"
- "webLogon"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190856167.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190858220.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件，解压得到tmp文件夹，内有logon.pcapng。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190859752.png)

---

### 解题思路：

1、没想到这道题这么简单。双击logon.pcapng文件，在Wireshark中打开，流量很少。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190901506.png)

直接追踪HTTP流，看一下内容。发现是登录流量，尝试登录三次，都失败了。但用户名和密码并没有变化，只是尝试登录了三次。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190903449.png)

登录所使用的 `password` ，使用URL编码。使用在线网站进行解码，得到flag： `CFI{1ns3cur3_l0g0n}` 。

[在线urlencode编码、urldecode解码、url编码解码、百分号编码](http://web.chacuo.net/charseturlencode) 

```bash
%20%43%46%49%7b%31%6e%73%33%63%75%72%33%5f%6c%30%67%30%6e%7d%20
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190906009.png)

### flag：

```bash
 flag{1ns3cur3_l0g0n} 
```