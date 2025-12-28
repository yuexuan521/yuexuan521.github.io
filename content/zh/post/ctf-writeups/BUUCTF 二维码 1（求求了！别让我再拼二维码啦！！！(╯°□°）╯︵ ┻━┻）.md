---
title: "BUUCTF 二维码 1（求求了！别让我再拼二维码啦！！！(╯°□°）╯︵ ┻━┻）"
date: 2025-03-24 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "BUUCTF"
- "CTF"
- "misc"
- "Qr_research"
- "二维码"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192554081.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[CTF-Misc-二维码（二）撕破的二维码](https://blog.csdn.net/qq_45163122/article/details/106124042) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192557018.png)

### 题目描述：

一不小心把存放flag的二维码给撕破了，各位大侠帮忙想想办法吧 注意：得到的 flag 请包上 flag{} 提交

### 密文：

保存附件，得到860c6e1a-a433-4a70-bfd0-5a318e758705.jpg

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192558368.jpeg)

---

### 解题思路：

1、所见即所得，开始拼二维码吧。

首先，将所有二维码碎片截取出来。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192601525.png)

最后，用PS拼在一起。（糟糕的手艺活）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192603853.png)

2、得到二维码后，用QR_Research一直扫不出来，转用手机夸克扫出来，flag: `flag{7bf116c8ec2545708781fd4a0dda44e5}` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192606320.jpeg)

发现我拼的二维码，用什么在线网站都扫不出来，真是我拼的差吗？(╯°□°）╯︵ ┻━┻）

### flag：

```bash
flag{7bf116c8ec2545708781fd4a0dda44e5}
```