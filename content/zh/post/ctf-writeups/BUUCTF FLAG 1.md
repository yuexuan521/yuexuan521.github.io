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
- "MISC"
- "Wireshark"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228185838079.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「FLAG 1」，要求把 hctf 换成 flag 提交](/images/blog/20251228185840672.png)

### 题目描述：

注意：请将 hctf 替换为 flag 提交，格式 flag{}

### 密文：

下载附件，得到一张.png图片。

![灌篮高手截图，字幕是「教练，我…我想打CTF」](/images/blog/20251228185842263.png)

---

### 解题思路：

1、因为附件是一张图片，先放到StegSolve中，在Analyse（分析）选项卡，使用Data Extract（数据提取），可以看到有zip文件的标志。（通过）
[跟这道题有异曲同工之妙](https://blog.csdn.net/YueXuan_521/article/details/133822506?spm=1001.2014.3001.5502) 

![Stegsolve 的 Extract Preview，红框显示 50 4b 03 04，即 ZIP 文件头](/images/blog/20251228185844648.png)

2、由pk文件头可以看出隐写内容为zip文件，按save Bin键保存为zip文件。解压获得一个名为1的文件，可以通过添加.txt后缀或在010 Editor中查看文件。仔细找，找到flag。

![分离出来的文件名为 1，没有扩展名，显示 0 KB](/images/blog/20251228185846505.png)

![十六进制视图，红框标出 hctf{dd0gf4c3tok3yb0ard4g41n~~~}](/images/blog/20251228185847940.png)

![编码错乱的文本块中间夹着同一串 hctf{…}](/images/blog/20251228185849683.png)

### flag：

```bash
flag{dd0gf4c3tok3yb0ard4g41n~~~}
```