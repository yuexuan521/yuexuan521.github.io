---
title: "BUUCTF [SWPU2019]伟大的侦探 1"
date: 2024-09-21 23:00:05
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191926718.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「［SWPU2019］伟大的侦探 1」，签到 flag{buu_ctf}，附 QQ 频道与群链接](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191928889.png)

### 题目描述：

下载附件，解压提示需要密码，但解压出一个密码.txt文件。

### 密文：

 ![资源管理器里的 misc 文件夹与 密码.txt（1 KB）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191930787.png)

---

### 解题思路：

1、打开密码.txt文件，提示如下。

```bash
压缩包密码:摂m墷m卪倕ⅲm仈Z
呜呜呜,我忘记了压缩包密码的编码了,大家帮我解一哈。
```

 ![Notepad++ 打开密码.txt，压缩包密码是一串乱码，并写着「我忘记了压缩包密码的编码了」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191932123.png)

2、使用010Editor打开密码.txt文件，选择编辑方式为EBCDIC(B)，找到明文密码。（不明白这里的原理）

```bash
wllm_is_the_best_team!
```

 ![把编码切成 EBCDIC 后，同一行显示出 wllm is the best team! 等可读字符](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191933522.png)

3、使用密码解压压缩包，得到18个.jpg图片，图片为跳舞的小人图形密码（出自于《福尔摩斯探案集》跳舞的小人），使用如下密码对照表，可以获得明文。

 ![18 张 1.jpg–18.jpg 缩略图，每张都是一个跳舞的小人剪影](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191935067.png)

![w](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191937043.png)

4、对照密码表解密，得到flag值。

```bash
iloveholmesandwllm
```

### flag：

```bash
flag{iloveholmesandwllm}
```