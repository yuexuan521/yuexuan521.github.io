---
title: "BUUCTF easyre 1"
date: 2024-09-23 22:53:22
category: "BUUCTF Reverse"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "安全"
- "IDA"
- "Reverse"
---



![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251218085607601.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 





---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251218085607602.png)

### 题目描述：

下载附件，解压得到一个.exe文件。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251218085607603.png)

---

### 解题思路：

1、使用IDA pro打开exe文件，在反汇编窗口（IDA View-A），直接找到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251218085607604.png)

也可以通过使用快捷键shift+F12：自动分析出参考字符串，找到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251218085607605.png)

### flag：

```bash
flag{this_Is_a_EaSyRe}
```