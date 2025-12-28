---
title: "BUUCTF [MRCTF2020]CyberPunk 1"
date: 2024-12-23 07:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "CTF"
- "BUUCTF"
- "misc"
- "CyberPunk"
- "windows"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191452501.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191454519.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢天璇战队供题。

### 密文：

下载附件解压，得到cyberpunk! .exe文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191456196.png)

---

### 解题思路：

1、一开始我以为是逆向的题目，直接打开IDA了，没什么成果才返回。实际上，运行程序，根据提示修改电脑的时间就可以得到flag。

开始运行程序，提示时间必须在2020年9月17日才可以。

> **《赛博朋克2077》宣布跳票！延期至9月17日** 
> 我们今天有关于《赛博朋克2077》发行日期的重要消息想与大家分享。《赛博朋克2077》将不会在4月发布，我们将发布日期推迟到 **2020年9月17日** 。
> 
> 《赛博朋克2077》目前处于游戏已完成并且可玩的阶段，但仍有工作要做。夜之城规模庞大，这里充满了故事、内容和值得挖掘的地方。正由于它的庞大规模和复杂性，我们需要更多的时间来完成游戏测试、修复和完善。我们希望《赛博朋克2077》能够成为我们的最高成就，而延迟发行将会带来几个月的宝贵时间，让我们去完善游戏。
> 
> 我们真得很期待在夜之城与您相遇，感谢您一直以来的支持！
> 
> 创始人：Marcin lwinski
> 
> 工作室负责人：Adam Badowski

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191457741.png)

修改完成后，需要重新启动程序，这时就会出现flag

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191459544.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191501534.png)

### flag：

```bash
MRCTF{We1cOm3_70_cyber_security}
flag{We1cOm3_70_cyber_security}
```