---
title: "BUUCTF snake 1"
date: 2024-04-23 11:04:41
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "BUUCTF"
- "笔记"
- "CTF"
- "MISC"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190218014.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190220093.png)

### 题目描述：

下载附件，解压得到一张snake的图片。

### 密文：

这里有一张蛇的图片，本人害怕不敢放，想看自己下载附件解压。（吐槽一下，我做这道题，全程没有看那张图片，结果搜了个snake的翻译，下面出现一排snake的照片，当场给我送走，希望别让我再碰到这种题了！）

---

### 解题思路：

1、拿到图片，放010Editor看一下，找到PK标识，说明有隐藏的zip压缩包。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190221763.png)

使用Kali中的binwalk工具进行检测，确实存在zip压缩包，用foremost工具分离出zip压缩包，在output目录下查看。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190223629.png)

2、解压zip压缩包，解压成功，得到两个文件：cipher、key。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190226491.png)

3、 **key** 文件打开后，显示一串用Base64加密过的密文字符串。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190229031.png)

解密之后，得到一个明文字符串“What is Nicki Minaj’s favorite song that refers to snakes?”，翻译过来“尼基-米娜最喜欢哪首提到蛇的歌曲？”。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190230837.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190232924.png)

我搜索了一下，找到了提示所指向的内容。《Anaconda》是美国说唱女歌手妮琪·米娜演唱的一首说唱歌曲，《Anaconda》在美国公告牌单曲榜上最高名次为第2名，是妮琪·米娜成绩最高的歌曲之一。
“anaconda”就是我们要找的key。
[https://baike.baidu.com/item/anaconda/15222448](https://baike.baidu.com/item/anaconda/15222448) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190235571.png)

**cipher：** 是一个数据格式的文件。我是看了别人的题解，才知道使用的是serpent加密算法，同时法语“serpent”翻译过来也是蛇的意思，切合题目。
[SERPENT算法学习心得](https://blog.csdn.net/douqingl/article/details/50256931) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190237423.png)

使用在线工具进行解密，再加上之前得到的key，最后得到flag。
[在线工具](http://serpent.online-domain-tools.com/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190239013.png)

### flag：

```bash
flag{who_knew_serpent_cipher_existed}
```