---
title: "BUUCTF 秘密文件 1"
date: 2024-08-23 11:03:30
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228193211909.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)

 ![题目页「秘密文件 1」：从监控流量里分析被偷走的机密文件](/images/blog/20251228193214117.png)

### 题目描述：

深夜里，Hack偷偷的潜入了某公司的内网，趁着深夜偷走了公司的秘密文件，公司的网络管理员通过通过监控工具成功的截取Hack入侵时数据流量，但是却无法分析出Hack到底偷走了什么机密文件，你能帮帮管理员分析出Hack到底偷走了什么机密文件吗？

### 密文：

下载附件，解压得到一个.pcapng文件。
 ![305df1f78bef4ccfd2a3bd0fe4a6c0d7.pcap，33 KB](/images/blog/20251228193216064.png)

---

### 解题思路：

1、双击文件，打开wireshark。翻阅流量时找到ftp的流量，将ftp流量过滤下来。
 ![过滤 ftp：220 HI, i know you are a hacker…、USER ctf、PASS ctf、PORT、RETR](/images/blog/20251228193217488.png)

追踪ftp流量，有出题者留下的话，并且发现流量中有flag.rar压缩包。

```bash
原文：HI, i know you are a hacker who is trying to hack me ,but can u find where is the flag？
翻译：你好，我知道你是黑客，想黑我，但你能找到flag在哪里吗？
```

![跟踪流得到完整会话，红框 RETR 6b0341642a8ddcbe2a927dae6d541.rar](/images/blog/20251228193219609.png)

2、使用Kali中的foremost工具，将rar压缩包从pcapng文件中提取出来。（wireshark 截取的流量中，会截取文件传输对应的流量，也就是说，这个流量包将包括 flag.rar压缩包）
 ![binwalk 显示 RAR archive data 4.x，foremost 分出 00000031.rar](/images/blog/20251228193221937.png)

尝试解压压缩包，需要密码。

 ![打开这个 rar 时提示输入密码](/images/blog/20251228193224234.png)

3、因为没有关于密码的提示，所以使用常用的4位纯数字进行破解。使用ARCHPR工具，选定参数，破解得到密码为1903。

 ![Advanced Archive Password Recovery 统计：总计口令 1,905、4s 642ms、口令 1903](/images/blog/20251228193225617.png)

使用密码解压压缩包，得到txt文件，打开得到flag。
 ![flag{d72e5a671aa50fa5f400e5d10eedeaa5}](/images/blog/20251228193227153.png)

### flag：

```bash
flag{d72e5a671aa50fa5f400e5d10eedeaa5}
```