---
title: "BUUCTF 爱因斯坦 1"
date: 2024-09-23 23:00:50
category: "BUUCTF Crypto"
tags:
- "网络安全"
- "CTF"
- "安全"
- "Misc"
- "图片隐写"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205058230.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205100371.png)

### 题目描述：

下载附件，解压得到一张.jpg图片。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205101940.jpeg)

---

### 解题思路：

1、因为题目没有什么提示，我们就一一尝试。将图片放到StegSolve中，在查看图片的File Format时，先看到一条有意义的文本，然后找到隐藏zip文件的信息。
[参考思路](https://blog.csdn.net/YueXuan_521/article/details/133822506?spm=1001.2014.3001.5502) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205103554.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205104700.png)

2、使用Kali中的binwalk工具进行检测，确认存在zip压缩包和flag.txt文件。使用Kali中的foremost工具，分离出misc.jpg中的压缩文件，使用ls命令查看，得到一个output目录，查看output目录下的文件，找到zip文件（如果提示错误尝试删除原有的output目录，再执行foremost）。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205106771.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205108693.png)

3、尝试解压zip压缩文件，需要密码，但是题目中没有关于密码的提示。寻找无果后，我使用fcrackzip进行暴力破解，在暴力破解的同时继续寻找密码。我开始在图片中寻找密码，在使用cat命令查看图片时，想起了在第一步中看到的文本“this_is_not_password”，尝试之后我惊奇的发现，解压成功了。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205110080.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205112133.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205114551.png)

4、查看flag.txt文件，得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205116400.png)

### flag：

```bash
flag{dd22a92bf2cceb6c0cd0d6b83ff51606}
```