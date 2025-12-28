---
title: "BUUCTF 文件中的秘密 1"
date: 2024-09-24 22:44:51
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "安全"
- "Misc"
- "图片隐写"
- "笔记"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193029240.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193031339.png)

### 题目描述：

小明经常喜欢在文件中藏一些秘密。时间久了便忘记了，你能帮小明找到该文件中的秘密吗？

### 密文：

下载附件，解压得到JPEG图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193032942.jpeg)

### 解题思路：

1、根据题目提示，寻找文件中的秘密。在这里，我发现给到的文件是一张JPEG图片，使用StegSolve工具，查看图片文件格式。直接找到flag。（用WinHex也可以找到flag）
具体流程可以查看这篇文章： [https://blog.csdn.net/YueXuan_521/article/details/133817677?spm=1001.2014.3001.5502](https://blog.csdn.net/YueXuan_521/article/details/133817677?spm=1001.2014.3001.5502) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193035480.png)

2、将这段字符复制到word或记事本替换掉“.”，清除字符串里的空格，整理出flag。
[清除空格在线工具](http://www.esjson.com/delSpace.html) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193036956.png)

### flag：

```bash
flag{870c5a72806115cb5439345d8b014396}
```