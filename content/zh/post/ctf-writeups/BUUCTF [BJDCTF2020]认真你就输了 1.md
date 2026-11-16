---
title: "BUUCTF [BJDCTF2020]认真你就输了 1"
date: 2024-09-22 22:48:54
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190810868.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［BJDCTF2020］认真你就输了 1」，附 GitHub 来源与 attachment.rar](/images/blog/20251228190812878.png)

### 题目描述：

来源：https://github.com/BjdsecCA/BJDCTF2020

### 密文：

下载附件，解压得到一个.xls文件。
 ![解压出的 10.xls，20 KB，显示为 Microsoft Excel 97-…](/images/blog/20251228190814862.png)

---

### 解题思路：

1、双击文件，提示“10.xls”的文件格式和扩展名不匹配。
 ![Excel 弹窗「"10.xls"的文件格式和扩展名不匹配」](/images/blog/20251228190816150.png)

在010Editor中打开10.xls文件，发现PK文件头，猜测为zip压缩包。
 ![十六进制开头是 50 4B 03 04，右侧可见 /charts/flag.txt 与 docProps/PK](/images/blog/20251228190818188.png)

2、将10.xls文件的后缀名改为.zip，修改后解压，得到很多文件夹。
 ![把 xls 当 zip 解开，得到 _rels、docProps、xl 目录与 ［Content_Types］.xml](/images/blog/20251228190819946.png)

这一步之后不知道该如何分析本题，看了别人的题解，才知道原来flag就在xl文件夹下，只需要进入xl文件夹，在进入charts文件夹，就可以看到flag.txt文件。果然是“认真你就输了”！
 ![xl/charts 目录里有 chart1.xml、chart2.xml 与 flag.txt](/images/blog/20251228190821372.png)

打开flag.txt文件，得到flag。
 ![记事本打开 flag.txt：flag{M9eVfi2Pcs#}](/images/blog/20251228190822741.png)

### flag：

```bash
flag{M9eVfi2Pcs#}
```