---
title: "BUUCTF [SCTF2019]电单车 1"
date: 2025-04-21 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
- "音频隐写"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191805436.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [使用HackCube-Special分析固定码信号](https://www.freebuf.com/articles/wireless/191534.html)
- [［SCTF2019］电单车](https://blog.csdn.net/weixin_49656607/article/details/120323952)


 ![题目页「［SCTF2019］电单车 1」，附件 attachment…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191807528.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到attachment.wav

 ![attachment.wav，10,400 KB，显示为 WAV 文件](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191809387.png)

---

### 解题思路：

1、用Audacity打开attachment.wav，波形图如下：

 ![Audacity 打开，0.03–0.06 处有一段密集波形，之后全是平线](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191810497.png)

放大突出的部分，可以看到波形大致分两种：长波和短波。

 ![放大到 0.023–0.067，每段波形的间隔清晰可数](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191812587.png)

将长波替换为 `1` ，短波替换为 `0` ，得到数据： `00111010010101010011000100` 。

 ![在波形下方逐个标注 0 1 1 1 0 1 0 0 1 0 1 0 1 0 0 1 1 0 0 0 1 0 0](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191814676.png)

2、这里的音频其实是 **PT224X信号** ，一种固定码遥控信号，我们需要将信号中的地址位作为flag提交。

[使用HackCube-Special分析固定码信号](https://www.freebuf.com/articles/wireless/191534.html) 

> 钥匙信号(PT224X) = 同步引导码(8bit) + 地址位(20bit) + 数据位(4bit) + 停止码(1bit).

 ![PT224X 协议帧结构：同步码 8Bit、地址位 20Bit、数据位 4Bit、停止码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191817476.png)

 ![同一波形上红框分区标出同步码、地址位、数据位与停止码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191819095.png)

`0 （同步码） 01110100101010100110（地址位） 0010 （数据位） 0（停止码）` 

### flag：

```bash
flag{01110100101010100110}
```