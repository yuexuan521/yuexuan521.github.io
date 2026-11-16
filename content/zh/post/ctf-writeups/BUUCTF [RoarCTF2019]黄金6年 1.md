---
title: "BUUCTF [RoarCTF2019]黄金6年 1"
date: 2024-08-21 20:38:23
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "MISC"
- "隐写"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191736799.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［RoarCTF2019］黄金6年 1」，附件 attachment.…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191738963.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到.mp4文件。

<div align="center" style="border: 3px solid gray;border-radius: 27px;overflow: hidden;"> <a class="link-info" href="https://live.csdn.net/v/embed/348339" rel="nofollow" title="attachment">attachment</a><iframe id="wxkPf7r3-1701582521906" frameborder="0" src="https://live.csdn.net/v/embed/348339" allowfullscreen="true" data-mediaembed="csdn" style="width: 100%; aspect-ratio: 2;"></iframe></div>

---

### 解题思路：

1、浅浅的看了一遍，没发现什么有用的内容。放到Kinovea中，慢倍速看了一遍，发现四个二维码。（我真的没想到还有第四个二维码，找了半天都没有，结果是Kinovea自动去掉了最后的一个片段，害苦我了，最后是用Microsoft Clipchamp找到的，点赞）

```bash
key1:i
```

![《C++ Primer Plus（第6版）中文版》书封，右上角有个二维码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191740591.png)

 ![QR Research 解出 key1：](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191743186.png)

```bash
key2:want
```

![《Web安全深度剖析》红色书封，左下角一个二维码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191745372.png)

 ![解出 key2:want](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191747483.png)

```bash
key3:play
```

![《逆向工程核心原理》书封，封面是一片叶子，右下角二维码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191749090.png)

 ![解出 key3:play](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191751386.png)

```bash
key4:ctf
```

![第四本书只露出暗部，左下角隐约有二维码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191753110.png)

 ![解出 key4:ctf](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191755237.png)

连接起来，得到key（而不是flag）。

```bash
iwantplayctf
```

2、提交flag无果后，开始尝试其他的方向。在010Editor中，在文件的最后发现一串经过Base64编码的字符串，解密的内容如下，感觉是rar压缩包的数据。

 ![十六进制里 55 6D 46 79… 一段 base64，Ascii 区是 UmFyiRoAA…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191757389.png)

 ![Base64 解码后全是乱码，但能认出 Rar! 与 flag.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191759584.png)

使用在线网站，直接解密并另存为rar文件。

![在线工具识别出【RAR-Lab】，可另存为 .rar，Version 5.0、共 18 个文件](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191801364.png)

3、使用之前得到的key作为密码，解压rar压缩包，得到flag.txt文件，打开文件得到flag。

 ![解开压缩包，flag.txt 里是 roarctf{CTF-from-RuMen-to-RuYuan}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191803558.png)

### flag：

```bash
flag{CTF-from-RuMen-to-RuYuan}
```