---
title: "BUUCTF [ACTF新生赛2020]outguess 1"
date: 2024-09-22 20:30:39
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

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190515100.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190517595.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到一堆文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190519174.png)

---

### 解题思路：

1、根据题目和flag.txt文件提示，猜测为outguess隐写。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190521004.png)

```bash
outguess下载安装
kail 终端命令输入git clone https://github.com/crorvick/outguess，安装包下载完成到文件夹。
打开文件夹，右键空白处选终端打开，输入命令./configure && make && make install进行安装。
```

2、查看mmm.jpg图片属性，得到一段经过社会主义核心价值观编码加密的密文：“公正民主公正文明公正和谐”。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190522026.png)

使用在线网站进行解密，得到明文：“abc”，作为outguess解密的密钥。
[在线网站 https://sym233.github.io/core-values-encoder/](https://sym233.github.io/core-values-encoder/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190523484.png)

3、mmm.jpg是最重要的文件，在Kali中，使用刚安装的outguess对mmm.jpg文件进行解密，导出隐写的内容。

```bash
outguess -k "abc" -r  mmm.jpg flag.txt
隐写的内容最终会导入flag.txt文件。
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190524834.png)

执行成功后，我们查看flag.txt文件，得到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190526630.png)

### flag：

```bash
flag{gue33_Gu3Ss!2020}
```