---
title: "BUUCTF 萌萌哒的八戒 1"
date: 2024-09-25 21:47:31
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "网络安全"
- "密码学"
- "CTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228205117798.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「萌萌哒的八戒 1」，附件 76ad2edd-8e…](/images/blog/20251228205120303.png)

### 题目描述：

萌萌哒的八戒原来曾经是猪村的村长，从远古时期，猪村就有一种神秘的代码。请从附件中找出代码，看看萌萌哒的猪八戒到底想说啥

### 密文：

解压是一张图片

![一只戴警帽的小猪玩偶照片](/images/blog/20251228205121979.jpeg)

---

### 解题步骤：

1、看图，观察密文特征，结合题目，判断为猪圈加密。直接使用在线工具。
[猪圈密码解密工具](http://www.metools.info/code/c90.html) 

![猪圈密码工具：加密内容是一串方框符号，解密得到 whenthepigwanttoeat](/images/blog/20251228205123446.png)

2、直接对应密文输入，解出密文，得到flag。

---

### flag:

```
whenthepigwanttoeat
```