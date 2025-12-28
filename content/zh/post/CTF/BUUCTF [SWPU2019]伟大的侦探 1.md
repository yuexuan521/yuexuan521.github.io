---
title: "BUUCTF [SWPU2019]伟大的侦探 1"
date: 2024-09-21 23:00:05
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191926718.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191928889.png)

### 题目描述：

下载附件，解压提示需要密码，但解压出一个密码.txt文件。

### 密文：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191930787.png)

---

### 解题思路：

1、打开密码.txt文件，提示如下。

```bash
压缩包密码:摂m墷m卪倕ⅲm仈Z
呜呜呜,我忘记了压缩包密码的编码了,大家帮我解一哈。
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191932123.png)

2、使用010Editor打开密码.txt文件，选择编辑方式为EBCDIC(B)，找到明文密码。（不明白这里的原理）

```bash
wllm_is_the_best_team!
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191933522.png)

3、使用密码解压压缩包，得到18个.jpg图片，图片为跳舞的小人图形密码（出自于《福尔摩斯探案集》跳舞的小人），使用如下密码对照表，可以获得明文。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191935067.png)

![w](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191937043.png)

4、对照密码表解密，得到flag值。

```bash
iloveholmesandwllm
```

### flag：

```bash
flag{iloveholmesandwllm}
```