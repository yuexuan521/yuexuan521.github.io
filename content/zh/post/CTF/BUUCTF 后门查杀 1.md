---
title: "BUUCTF 后门查杀 1"
date: 2024-09-24 16:36:45
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
- "MISC"
- "笔记"
- "webshell"
- "后门"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192720667.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192722932.png)

### 题目描述：

小白的网站被小黑攻击了，并且上传了Webshell，你能帮小白找到这个后门么？(Webshell中的密码(md5)即为答案)。

### 密文：

下载附件，解压得到一个网站文件夹。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192724587.png)

---

### 解题思路：

1、使用工具扫描附件所给的文件夹，可以使用D盾、火绒安全之类的工具，这里我使用D盾。
打开D盾，点击自定义扫描，选择附件给的网站文件夹，点击确定。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192727358.png)

检查出两个文件，我们挨个查看。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192729441.png)

2、第一个文件没有flag，但我们在第二个文件中找到了Webshell中的密码，密码也符合md5的格式，这个就是flag值。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192731588.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192733491.png)

### flag：

```bash
flag{6ac45fb83b3bc355c024f5034b947dd3}
```