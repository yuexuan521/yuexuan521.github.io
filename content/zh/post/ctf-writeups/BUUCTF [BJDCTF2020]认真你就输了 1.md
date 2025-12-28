---
title: "BUUCTF [BJDCTF2020]认真你就输了 1"
date: 2024-09-22 22:48:54
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "网络安全"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190810868.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190812878.png)

### 题目描述：

来源：https://github.com/BjdsecCA/BJDCTF2020

### 密文：

下载附件，解压得到一个.xls文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190814862.png)

---

### 解题思路：

1、双击文件，提示“10.xls”的文件格式和扩展名不匹配。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190816150.png)

在010Editor中打开10.xls文件，发现PK文件头，猜测为zip压缩包。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190818188.png)

2、将10.xls文件的后缀名改为.zip，修改后解压，得到很多文件夹。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190819946.png)

这一步之后不知道该如何分析本题，看了别人的题解，才知道原来flag就在xl文件夹下，只需要进入xl文件夹，在进入charts文件夹，就可以看到flag.txt文件。果然是“认真你就输了”！
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190821372.png)

打开flag.txt文件，得到flag。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190822741.png)

### flag：

```bash
flag{M9eVfi2Pcs#}
```