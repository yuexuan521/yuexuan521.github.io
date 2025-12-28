---
title: "BUUCTF [UTCTF2020]docx 1"
date: 2024-09-21 20:41:56
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

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192042619.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192044729.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到一个.docx文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192046365.png)

---

### 解题思路：

1、打开文件，内容如下，没有flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192047497.png)

使用010Editor打开.docx文件，发现存在PK文件的文件头，确认为zip文件。（office的文件似乎都是zip压缩包，通过某种技术呈现为我们看到的样子）
（ZIPArchive(zip)，文件头：50 4B 03 04）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192049298.png)

将.docx文件的后缀改为.zip，进行解压，得到如下文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192051087.png)

2、进入word文件夹，再进入media文件夹，发现flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192053144.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192055113.png)

### flag：

```bash
flag{unz1p_3v3ryth1ng}
```