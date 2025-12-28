---
title: "[WUSTCTF2020]alison_likes_jojo 1"
date: 2024-04-21 20:22:28
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "密码学"
- "outguess"
- "BUUCTF"
- "MISC"
- "CTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523167.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523169.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢 Iven Huang 师傅供题。
比赛平台： [https://ctfgame.w-ais.cn/](https://ctfgame.w-ais.cn/) 

### 密文：

下载附件解压，得到两张jpg图片和一个文本文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523170.jpeg)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523171.jpeg)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523172.png)

---

### 解题思路：

1、使用010 Editor打开图片，发现boki.jpg图片隐藏了一个ZIP文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523173.png)

在Kali中，使用binwalk检测，确认图片中隐藏zip压缩包。

```bash
binwalk boki.jpg 
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523174.png)

使用foremost分离图片中的压缩包，在output目录中找到隐藏的zip压缩包。

```bash
tree ./output 
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523175.png)

2、尝试解压得到的压缩包，需要密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523176.png)

因为没有关于密码的提示，尝试用Ziperello进行6位纯数字爆破，得到密码888866。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523177.png)

3、使用密码解压压缩包，得到beisi.txt文件，内容如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523178.png)

尝试使用base64进行解密，并发现该密文为base64多重加密，最后得到的明文如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523179.png)

4、目前还有一张图片和刚得到的“killerqueen”没有使用，看了题解才知道jljy图片应该使用outguess解密，而“killerqueen”就是解密密钥。

在Kali中，使用outguess对jljy.jpg文件进行解密，导出隐写的内容到flag.txt。查看flag.txt文件，得到flag。

```bash
outguess -k "killerqueen" -r jljy.jpg flag.txt

outguess：这是命令行工具的名字，用于进行隐写术操作，即将秘密信息嵌入到载体文件中。

-k "killerqueen"：这个选项表示使用的密钥或密码短语是"killerqueen"。

-r jljy.jpg：-r 参数指定的是源图像文件，即载体文件，在本例中是名为"jljy.jpg"的JPEG格式图片文件。

flag.txt：存放隐藏信息的文件。
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251224133523180.png)

### flag：

```bash
flag{pretty_girl_alison_likes_jojo}
```