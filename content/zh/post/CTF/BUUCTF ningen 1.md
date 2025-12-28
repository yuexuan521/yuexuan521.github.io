---
title: "BUUCTF ningen 1"
date: 2024-09-24 16:34:24
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "CTF"
- "BUUCTF"
- "图片隐写"
- "笔记"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190050890.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190052957.png)

### 题目描述：

人类的科学日益发展，对自然的研究依然无法满足，传闻日本科学家秋明重组了基因序列，造出了名为ningen的超自然生物。某天特工小明偶然截获了日本与俄罗斯的秘密通信，文件就是一张ningen的特写，小明通过社工，知道了秋明特别讨厌中国的六位银行密码，喜欢四位数。你能找出黑暗科学家秋明的秘密么？

### 密文：

下载附件，得到一张.jpg图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190055343.jpeg)

---

### 解题思路：

1、本来想使用StegSolve先看一下的，但感觉题目后半部分的密码方向提示，很想我之前做的一道图片隐藏zip文件的题。使用010 Editor打开这个图片，果然找到了zip文件的痕迹。（在StegSolve的File Format中，也可以找到zip文件的痕迹）
[本题思路可以参考这道例题](https://blog.csdn.net/YueXuan_521/article/details/133822506?spm=1001.2014.3001.5502) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190056952.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190059014.png)

2、有方向就很简单了。在Kali中，使用binwalk确认是否存在隐藏文件。找到zip压缩包和其中的ningen.txt文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190101012.png)

3、分离这张图片中的隐藏文件。使用foremost分离出文件，找到output目录查看。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190102820.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190104616.png)

4、尝试解压zip压缩包，需要密码。根据题目信息，密码为四位纯数字，使用fcrackzip破解zip压缩包的密码，得到密码为8368。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190106055.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190107996.png)

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

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190113343.png)

### flag：

```bash
flag{b025fc9ca797a67d2103bfbc407a6d5f} 
```