---
title: "BUUCTF 你竟然赶我走 1"
date: 2024-09-24 22:37:55
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "笔记"
- "密码学"
- "网络安全"
- "crypto"
- "BUUCTF"
- "CTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192621766.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192624322.png)

### 题目描述：

下载附件后有一张图片。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192625993.jpeg)

### 解题思路：

有两种解题方法

#### 方法一：

1、使用StegSolve打开图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192627438.png)

2、打开FileFormat（文件格式）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192628780.png)

3、拉到最下面，找到flag。（注意要消除所有的空格）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192633841.png)

#### 方法二：

1、使用WinHex打开图片，找到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192635420.png)

### flag：

```bash
flag{stego_is_s0_bor1ing}
```