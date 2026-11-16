---
title: "BUUCTF 后门查杀 1"
date: 2024-06-24 16:36:45
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
- "MISC"
- "笔记"
- "Web安全"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228192720667.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「后门查杀 1」：网站被上传 webshell，密码的 md5 即答案](/images/blog/20251228192722932.png)

### 题目描述：

小白的网站被小黑攻击了，并且上传了Webshell，你能帮小白找到这个后门么？(Webshell中的密码(md5)即为答案)。

### 密文：

下载附件，解压得到一个网站文件夹。

![站点根目录列表，admin、cache、data、include、upload 等目录与 .htaccess、article.php](/images/blog/20251228192724587.png)

---

### 解题思路：

1、使用工具扫描附件所给的文件夹，可以使用D盾、火绒安全之类的工具，这里我使用D盾。
打开D盾，点击自定义扫描，选择附件给的网站文件夹，点击确定。

![D盾_Web查杀主界面，红框标出「自定义扫描」](/images/blog/20251228192727358.png)

检查出两个文件，我们挨个查看。

![D盾_Web查杀扫出两条：web.php 可疑引用 ${$_GET［act］…，include.php 为 base64_decode 加密|system 执行](/images/blog/20251228192729441.png)

2、第一个文件没有flag，但我们在第二个文件中找到了Webshell中的密码，密码也符合md5的格式，这个就是flag值。

![web.php 全部三行：<?php include($_GET［act］ . ".php"); ?>](/images/blog/20251228192731588.png)

![include.php 第 30 行 $pass = '6ac45fb83b3bc355c024f5034b947dd3'; //angel](/images/blog/20251228192733491.png)

### flag：

```bash
flag{6ac45fb83b3bc355c024f5034b947dd3}
```