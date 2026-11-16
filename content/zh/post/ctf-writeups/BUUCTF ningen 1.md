---
title: "BUUCTF ningen 1"
date: 2024-07-24 16:34:24
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
- "隐写"
- "笔记"
---

![题图同样是那位蓝发女孩](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190050890.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「ningen 1」：秋明重组基因造出 ningen，截获的通信是一张特写；他讨厌六位银行密码、偏爱四位数](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190052957.png)

### 题目描述：

人类的科学日益发展，对自然的研究依然无法满足，传闻日本科学家秋明重组了基因序列，造出了名为ningen的超自然生物。某天特工小明偶然截获了日本与俄罗斯的秘密通信，文件就是一张ningen的特写，小明通过社工，知道了秋明特别讨厌中国的六位银行密码，喜欢四位数。你能找出黑暗科学家秋明的秘密么？

### 密文：

下载附件，得到一张.jpg图片。

![ningen 的照片：冰面上一个白色人形怪物](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190055343.jpeg)

---

### 解题思路：

1、本来想使用StegSolve先看一下的，但感觉题目后半部分的密码方向提示，很想我之前做的一道图片隐藏zip文件的题。使用010 Editor打开这个图片，果然找到了zip文件的痕迹。（在StegSolve的File Format中，也可以找到zip文件的痕迹）
[本题思路可以参考这道例题](https://blog.csdn.net/YueXuan_521/article/details/133822506?spm=1001.2014.3001.5502) 

![十六进制 9720h 处，红框 50 4B 03 04，行首绿底是 FF D9，后面可见 ningen.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190056952.png)

![StegSolve 的 File Format Analysis 里能看到 PK 开头与 ningen.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190059014.png)

2、有方向就很简单了。在Kali中，使用binwalk确认是否存在隐藏文件。找到zip压缩包和其中的ningen.txt文件。

![binwalk：JPEG JFIF 1.01，0x9721 起是 Zip archive data, encrypted，name: ningen.txt（compressed 50、uncompressed 38）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190101012.png)

3、分离这张图片中的隐藏文件。使用foremost分离出文件，找到output目录查看。

![foremost 输出 foundat=ningen.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190102820.png)

![ls output 得到 audit.txt、jpg、zip 三项](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190104616.png)

4、尝试解压zip压缩包，需要密码。根据题目信息，密码为四位纯数字，使用fcrackzip破解zip压缩包的密码，得到密码为8368。

![unzip 输错密码，提示 skipping: ningen.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190106055.png)

![fcrackzip -b -c1 -l 4 -u ./output/zip/00000075.zip，PASSWORD FOUND!!!!: pw == 8368](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190107996.png)

```bash
fcrackzip参数说明:
  -b 表示使用暴利破解的方式
  -c 指定字符集，字符集 格式只能为 -c 'aA1!:' 
  1 表示阿拉伯数字[0-9]
  -l 1-10 表示需要破解的密码长度为1到10位
  -u 表示只显示破解出来的密码，其他错误的密码不显示出
```

[fcrackzip工具详细用法](https://blog.csdn.net/weixin_43272781/article/details/100751375) 

5、使用密码解压，得到ningen.txt文件，打开文件得到flag。

![cat ningen.txt → flag{b025fc9ca797a67d2103bfbc407a6d5f}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190113343.png)

### flag：

```bash
flag{b025fc9ca797a67d2103bfbc407a6d5f} 
```