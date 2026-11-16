---
title: "BUUCTF 九连环 1"
date: 2024-06-23 22:50:58
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
- "隐写"
---

![题图同样是那位蓝发女孩](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192459277.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「九连环 1」，附件 389a0c11-d0…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192501281.png)

### 题目描述：

下载附件，解压得到一张.jpg图片。

### 密文：

![「留下了委屈的泪水」的简笔画表情包](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192502988.jpeg)

---

### 解题思路：

1、一张图片，典型的图片隐写。放到Kali中，使用binwalk检测，确认图片中隐藏zip压缩包。
 ![binwalk 123456cry.jpg：JPEG 之后 19560 处是 asd/，48454 处是压缩过的 asd/qwe.zip，末尾两条 End of Zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192504355.png)

使用foremost分离图片中的压缩包，在output目录中找到隐藏的zip压缩包。
 ![foremost 分出 asd/PK、asd/good-已合并.jpg、asd/qwe.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192506443.png)

 ![tree ./output：audit.txt、jpg/00000000.jpg、zip/00000038.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192509137.png)

2、尝试解压得到的压缩包，需要密码。
 ![unzip 00000038.zip：抽出 asd/qwe.zip，good-已合并.jpg 因密码不对被跳过](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192511006.png)

因为没有密码提示，猜测该zip压缩包为伪加密，通过010 Editor修改具体参数将伪压缩文件恢复到未加密的状态。

```bash
50 4B 01 02：目录中文件文件头标记

3F 00：压缩使用的 pkware 版本 （不重要）
14 00：解压文件所需 pkware 版本 （不重要）
00 00：全局方式位标记（有无加密，这个更改这里进行伪加密，改为09 00打开就会提示有密码了）
```

![010 Editor 71A0h 一带红框 00 00 加密标志，右侧可见 asd/qwe.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192513174.png)

3、修改后，解压压缩包不需要密码，解压成功，得到一张jpg图片和zip压缩包。尝试解压压缩包，需要密码，但确定其中有flag.txt文件。
 ![打开 flag.txt 时提示需要密码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192515363.png)

猜测密码信息在jpg文件中，用010 Editor看了一下，没有藏zip包。使用Kali中的steghide发现图片有隐写文件。（安装steghide工具，使用如下命令）

```powershell
apt-get install steghide
```

 ![steg hide info 那个文件名已显示成乱码，但仍报出内嵌 ko.txt（48 字节，rijndael-128 cbc）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192517112.png)

将隐藏文件从载体中分离出来。

![steg hide extract 写出 ko.txt，ll 里显示 48 字节](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192518963.png)

打开ko.txt文件，找到疑似压缩包密码的东西。
 ![cat ko.txt 得到一串乱码，中间夹着 bV1g6t5wZDJif^J7](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192521179.png)

复制密码，解压qwe.zip压缩包，打开flag.txt文件，得到flag。

### flag：

```bash
flag{1RTo8w@&4nK@z*XL}
```