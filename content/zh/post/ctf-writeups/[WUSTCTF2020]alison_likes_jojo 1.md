---
title: "[WUSTCTF2020]alison_likes_jojo 1"
date: 2024-04-21 20:22:28
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "密码学"
- "隐写"
- "BUUCTF"
- "MISC"
- "CTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523167.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［WUSTCTF2020］alison_likes_jojo 1」，感谢 Iven Huang 师傅供题](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523169.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢 Iven Huang 师傅供题。
比赛平台： [https://ctfgame.w-ais.cn/](https://ctfgame.w-ais.cn/) 

### 密文：

下载附件解压，得到两张jpg图片和一个文本文件。

![黑白漫画：吉良吉影，右上角日文「平穏を求める静かなる殺人鬼」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523170.jpeg)

![紫色西装的手比了个大拇指，JOJO 里的招牌手势](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523171.jpeg)

 ![记事本里只有一句 As we known, Alison is a pretty girl.](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523172.png)

---

### 解题思路：

1、使用010 Editor打开图片，发现boki.jpg图片隐藏了一个ZIP文件。

 ![010 Editor 里 boki.jpg 的 FF D9 之后紧跟 50 4B 03 04，后面是 beisi.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523173.png)

在Kali中，使用binwalk检测，确认图片中隐藏zip压缩包。

```bash
binwalk boki.jpg 
```

 ![binwalk boki.jpg：0 处 JPEG JFIF 1.01，177115（0x2B3DB）起是加密 Zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523174.png)

使用foremost分离图片中的压缩包，在output目录中找到隐藏的zip压缩包。

```bash
tree ./output 
```

 ![tree ./output：audit.txt、jpg/00000000.jpg、zip/00000345.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523175.png)

2、尝试解压得到的压缩包，需要密码。

 ![unzip 00000345.zip 时 beisi.txt 要密码，直接 skipping](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523176.png)

因为没有关于密码的提示，尝试用Ziperello进行6位纯数字爆破，得到密码888866。

 ![Ziperello：当前密码长度 6，弹窗给出「密码：888866」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523177.png)

3、使用密码解压压缩包，得到beisi.txt文件，内容如下：

 ![解出来的第一行是 WVRKc2MyS4yhWbmxqV0Zac1dsYzBQT09](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523178.png)

尝试使用base64进行解密，并发现该密文为base64多重加密，最后得到的明文如下：

 ![beisi.txt 一共四行，最后一行是明文的 killerqueen](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523179.png)

4、目前还有一张图片和刚得到的“killerqueen”没有使用，看了题解才知道jljy图片应该使用outguess解密，而“killerqueen”就是解密密钥。

在Kali中，使用outguess对jljy.jpg文件进行解密，导出隐写的内容到flag.txt。查看flag.txt文件，得到flag。

```bash
outguess -k "killerqueen" -r jljy.jpg flag.txt

outguess：这是命令行工具的名字，用于进行隐写术操作，即将秘密信息嵌入到载体文件中。

-k "killerqueen"：这个选项表示使用的密钥或密码短语是"killerqueen"。

-r jljy.jpg：-r 参数指定的是源图像文件，即载体文件，在本例中是名为"jljy.jpg"的JPEG格式图片文件。

flag.txt：存放隐藏信息的文件。
```

 ![outguess -k 密码 -r jljy.jpg flag.txt，再 cat 得到 wctf2020{pretty_girl_alison_likes_jojo}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523180.png)

### flag：

```bash
flag{pretty_girl_alison_likes_jojo}
```