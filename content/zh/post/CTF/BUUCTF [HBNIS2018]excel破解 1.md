---
title: "BUUCTF [HBNIS2018]excel破解 1"
date: 2024-09-22 20:35:18
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "安全"
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191418846.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191421329.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [
https://github.com/hebtuerror404/CTF_competition_warehouse_2018](https://github.com/hebtuerror404/CTF_competition_warehouse_2018) 

### 密文：

下载附件，得到一个attachment.xls文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191422914.png)

---

### 解题思路：

1、双击.xls文件，提示需要密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191423980.png)

2、使用010Editor查看该文件，查找文本“flag”，一条一条地看下来，找到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191425383.png)

第一次做的时候没有仔细看，结果没找到flag，后面看别人的题解才发现自己错过了flag。

### flag：

```bash
flag{office_easy_cracked}
```