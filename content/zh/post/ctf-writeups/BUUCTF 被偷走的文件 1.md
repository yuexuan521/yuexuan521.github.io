---
title: "BUUCTF 被偷走的文件 1"
date: 2024-09-23 11:10:08
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "CTF"
- "Misc"
- "BUUCTF"
- "wireshark"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193407556.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193431277.png)

### 题目描述：

一黑客入侵了某公司盗取了重要的机密文件，还好管理员记录了文件被盗走时的流量，请分析该流量，分析出该黑客盗走了什么文件。

### 密文：

下载附件，解压得到一个被偷走的文件.pcapng文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193433008.png)

---

### 解题思路：

双击文件，打开wireshark。翻阅流量时找到ftp的流量，将ftp流量过滤下来。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193434804.png)

追踪ftp流量，发现流量中有flag.rar压缩包。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193436768.png)

从这里开始有二种解题的方法：

**第一种方法：** 使用Kali中的foremost工具，将rar压缩包从pcapng文件中提取出来。（wireshark 截取的流量中，会截取文件传输对应的流量，也就是说，这个流量包将包括 flag.rar压缩包）
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193438365.png)

**第二种方法：** ftp协议有个ftp-data是ftp的数据通道，过滤出ftp-data的流量。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193440054.png)

追踪TCP流，将数据类型选择原始数据，另存为rar压缩包。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193442161.png)

得到压缩包后，尝试解压压缩包，但是需要密码。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193444426.png)

因为没有关于密码的提示，所以使用常用的4位纯数字进行破解。使用ARCHPR工具，选定参数，破解得到密码为5790。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193445810.png)

用密码解压rar压缩包，得到flag.txt文件，打开得到flag。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193447394.png)

### flag：

```bash
flag{6fe99a5d03fb01f833ec3caa80358fa3}
```