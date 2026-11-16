---
title: "BUUCTF easyre 1"
date: 2024-09-23 22:53:22
category: "BUUCTF Reverse"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "逆向"
---



![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251218085607601.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 





---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「easyre 1」，附件 easyre.zip，提示「非常简单的逆向」](/images/blog/20251218085607602.png)

### 题目描述：

下载附件，解压得到一个.exe文件。

### 密文：

![解压出的 easyre.exe，93 KB](/images/blog/20251218085607603.png)

---

### 解题思路：

1、使用IDA pro打开exe文件，在反汇编窗口（IDA View-A），直接找到flag。

![IDA 反汇编，红框标出 flag{this_Is_a_EaSyRe}，另一分支输出 sorry,you can't get flag](/images/blog/20251218085607604.png)

也可以通过使用快捷键shift+F12：自动分析出参考字符串，找到flag。

![IDA 的 Strings 窗口，第一行就是 flag{this_Is_a_EaSyRe}](/images/blog/20251218085607605.png)

### flag：

```bash
flag{this_Is_a_EaSyRe}
```