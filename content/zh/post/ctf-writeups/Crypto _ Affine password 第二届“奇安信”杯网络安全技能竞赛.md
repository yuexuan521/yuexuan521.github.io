---
title: "Crypto | Affine password 第二届“奇安信”杯网络安全技能竞赛"
date: 2024-09-24 22:29:43
category: "“奇安信”杯网络安全技能竞赛"
categories: 
  - "CTF"
tags:
- "web安全"
- "安全"
- "CTF"
- "笔记"
- "网络安全"
- "密码学"
---

---



相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/202512081856849.png)

### 题目描述：

明文经过仿射函数y=3x+9加密之后变为JYYHWVPIDCOZ，请对其进行解密，flag的格式为flag{明文的大写形式}。

### 密文：

```bash
JYYHWVPIDCOZ
```

---

### 解题思路：

1、使用在线网站直接破解或手工计算破解，获得flag。（参数a=3，b=9，对应仿射函数y=3x+9）
[仿射密码加密_仿射密码解密](http://www.metools.info/code/affinecipher183.html) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/202512081856851.png)

手工计算使用解密函数为D(x) = a^-1(x - b) (mod m)，也可以获得flag。

### flag：

```bash
AFFINECRYPTO
```

### 仿射密码简介：

单码加密法的另一种形式称为仿射加密法（affinecipher）。在仿射加密法中，字母表的字母被赋予一个数字。例如a=0，b=1，c=2…z=25。仿射加密法的密钥为0-25直接的数字对。