---
title: "BUUCTF [BJDCTF2020]鸡你太美 1"
date: 2024-08-23 11:01:20
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "MISC"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190824014.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)

 ![题目页「［BJDCTF2020］鸡你太美 1」，附 GitHub 来源与 attachment.rar](/images/blog/20251228190825977.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [
https://github.com/BjdsecCA/BJDCTF2020](https://github.com/BjdsecCA/BJDCTF2020) 

### 密文：

下载附件，解压得到两个.gif图片。
第一个gif图片：

![一张打篮球的 GIF 动图](/images/blog/20251228190827635.gif)

第二个gif图片无法打开。

---

### 解题思路：

1、使用010 Editor或WinHex打开，可以发现第二个gif文件缺少相应的gif文件头。

```bash
gif的文件头：47 49 46 38
```

![010 Editor 打开 篮球.gif，红框 47 49 46 38，但后面缺了 39 61](/images/blog/20251228190831154.png)

 ![篮球副本.gif 开头是 39 61 68 01，正好缺 47 49 46 38](/images/blog/20251228190832950.png)

2、我们给第二个gif文件加上gif的文件头，插入文件头的工具我使用WinHex，没有找到在010 Editor中插入的方法。

复制前一个文件的文件头
 ![篮球.gif 的头：47 49 46 38 39 61 B4 00](/images/blog/20251228190835208.png)

在第二个文件的开头粘贴
 ![篮球副本.gif 的头：47 49 46 38 39 61 68 01](/images/blog/20251228190837278.png)

3、保存文件，查看图片发现flag。
文件太大了，无法上传，放张图片吧。

![把两段的头拼起来后动图正常显示，左侧竖排 zhi-yin-you-are-beautiful](/images/blog/20251228190838954.png)

 ![同一张动图的另一帧，文字完整可读](/images/blog/20251228190841087.png)

### flag：

```bash
flag{zhi_yin_you_are_beautiful}
```