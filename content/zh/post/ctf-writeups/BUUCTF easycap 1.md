---
title: "BUUCTF easycap 1"
date: 2024-05-24 16:29:01
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

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228185829179.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「easycap 1」，附件 876932b1-5…](/images/blog/20251228185831284.png)

### 题目描述：

下载附件，解压得到一个.pcap文件。

### 密文：

![easycap.pcap 文件，大小 7 KB](/images/blog/20251228185832712.png)

---

### 解题思路：

1、这道题和它的名字一样，真的很easy。双击easycap.pcap文件，打开Wireshark。在Wireshark中，不管你追踪那个流量的TCP流，你都可以找到flag。

![Wireshark 右键菜单里选择 Follow → TCP Stream](/images/blog/20251228185834098.png)

![跟踪 TCP 流窗口里直接显示 FLAG:385b87afc8671dee07550290d16a8071](/images/blog/20251228185836174.png)

### flag：

```bash
flag{385b87afc8671dee07550290d16a8071}
```