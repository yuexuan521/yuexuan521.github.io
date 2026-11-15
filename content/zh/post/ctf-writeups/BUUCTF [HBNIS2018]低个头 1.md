---
title: "BUUCTF [HBNIS2018]低个头 1"
date: 2024-08-21 22:57:43
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "网络安全"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191427262.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［HBNIS2018］低个头 1」，附 GitHub 来源与 attachment.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191430231.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [
https://github.com/hebtuerror404/CTF_competition_warehouse_2018](https://github.com/hebtuerror404/CTF_competition_warehouse_2018) 

### 密文：

下载附件，得到一个.txt文件。
 ![题目文字：密文 EWAZX RTY TGB IJN IO KL，要求提交大写明文](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191432302.png)

---

### 解题思路：

1、低头看一下键盘，将描述中提到的字母连接起来，如下图，就得到了flag。

```bash
题目：低个头

描述：EWAZX RTY TGB IJN IO KL 请破解该密文 f

lag格式：XXX 明文

提交：直接提交明文（大写）
```

![键盘示意图，红线沿 QWERTY 与 ZXCV 行连出字母位置，提示看键盘下一行](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191433585.jpeg)

### flag：

```bash
flag{CTF}
```