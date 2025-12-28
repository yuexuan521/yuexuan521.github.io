---
title: "BUUCTF [SCTF2019]电单车 1"
date: 2025-04-21 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "BUUCTF"
- "CTF"
- "MISC"
- "Audacity"
- "PT224X信号"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191805436.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[使用HackCube-Special分析固定码信号](https://www.freebuf.com/articles/wireless/191534.html) 
[[SCTF2019]电单车](https://blog.csdn.net/weixin_49656607/article/details/120323952) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191807528.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到attachment.wav

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191809387.png)

---

### 解题思路：

1、用Audacity打开attachment.wav，波形图如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191810497.png)

放大突出的部分，可以看到波形大致分两种：长波和短波。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191812587.png)

将长波替换为 `1` ，短波替换为 `0` ，得到数据： `00111010010101010011000100` 。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191814676.png)

2、这里的音频其实是 **PT224X信号** ，一种固定码遥控信号，我们需要将信号中的地址位作为flag提交。

[使用HackCube-Special分析固定码信号](https://www.freebuf.com/articles/wireless/191534.html) 

> 钥匙信号(PT224X) = 同步引导码(8bit) + 地址位(20bit) + 数据位(4bit) + 停止码(1bit).

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191817476.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191819095.png)

`0 （同步码） 01110100101010100110（地址位） 0010 （数据位） 0（停止码）` 

### flag：

```bash
flag{01110100101010100110}
```