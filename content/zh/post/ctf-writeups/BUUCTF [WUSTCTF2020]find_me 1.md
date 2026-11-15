---
title: "BUUCTF [WUSTCTF2020]find_me 1"
date: 2024-09-22 20:27:15
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192226580.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［WUSTCTF2020］find_me 1」，附件 attachment.jpg](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192228808.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢 Iven Huang 师傅供题。
比赛平台： [https://ctfgame.w-ais.cn/](https://ctfgame.w-ais.cn/) 

### 密文：

下载附件，得到一个.jpg图片。

![圆形蓝色徽标，写着「西安科技大学信息安全协会 Information Security Association」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192230913.jpeg)

---

### 解题思路：

1、得到一张图片，先查看它的属性。

 ![文件属性 → 详细信息，备注栏是一串盲文点阵，作者 52HeRtz](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192232727.png)

```bash
⡇⡓⡄⡖⠂⠀⠂⠀⡋⡉⠔⠀⠔⡅⡯⡖⠔⠁⠔⡞⠔⡔⠔⡯⡽⠔⡕⠔⡕⠔⡕⠔⡕⠔⡕⡍=
```

2、在备注发现一串字符，似乎是盲文，使用在线网站进行解密，得到flag。
[盲文加解密在线网站](https://www.qqxiuzi.cn/bianma/wenbenjiami.php?s=mangwen) 

 ![文本转盲文工具解码，得到 wctf2020{y$0$u_f$1$n$d$_M$e$e$e$e$e}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192234429.png)

### flag：

```bash
flag{y$0$u_f$1$n$d$_M$e$e$e$e$e}
```