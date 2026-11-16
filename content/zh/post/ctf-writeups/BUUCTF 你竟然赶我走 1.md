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
- "BUUCTF"
- "CTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228192621766.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「你竟然赶我走 1」，附件 d6541cef-5…](/images/blog/20251228192624322.png)

### 题目描述：

下载附件后有一张图片。

### 密文：

![一张黑白简笔画兔子表情包，配字「你竟然赶我走」](/images/blog/20251228192625993.jpeg)

### 解题思路：

有两种解题方法

#### 方法一：

1、使用StegSolve打开图片。

![Stegsolve 主界面，红框标出 File → Open](/images/blog/20251228192627438.png)

2、打开FileFormat（文件格式）

![Stegsolve 的 Analyse 菜单，红框标出 File Format](/images/blog/20251228192628780.png)

3、拉到最下面，找到flag。（注意要消除所有的空格）

![File Format Analysis 窗口：End of Image 之后 Additional bytes = 38，Ascii 区可见 flag{stego_is_s0_borling}](/images/blog/20251228192633841.png)

#### 方法二：

1、使用WinHex打开图片，找到flag。

![WinHex 打开 biubiu.jpg，0x6360h 行的 ASCII 区高亮 -flag is flag{stego_is_s0_borling}](/images/blog/20251228192635420.png)

### flag：

```bash
flag{stego_is_s0_bor1ing}
```