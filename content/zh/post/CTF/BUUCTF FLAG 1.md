---
title: "BUUCTF FLAG 1"
date: 2024-06-24 16:32:15
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "笔记"
- "网络安全"
- "安全"
- "MISC"
- "wireshark"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185838079.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185840672.png)

### 题目描述：

注意：请将 hctf 替换为 flag 提交，格式 flag{}

### 密文：

下载附件，得到一张.png图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185842263.png)

---

### 解题思路：

1、因为附件是一张图片，先放到StegSolve中，在Analyse（分析）选项卡，使用Data Extract（数据提取），可以看到有zip文件的标志。（通过）
[跟这道题有异曲同工之妙](https://blog.csdn.net/YueXuan_521/article/details/133822506?spm=1001.2014.3001.5502) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185844648.png)

2、由pk文件头可以看出隐写内容为zip文件，按save Bin键保存为zip文件。解压获得一个名为1的文件，可以通过添加.txt后缀或在010 Editor中查看文件。仔细找，找到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185846505.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185847940.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185849683.png)

### flag：

```bash
flag{dd0gf4c3tok3yb0ard4g41n~~~}
```