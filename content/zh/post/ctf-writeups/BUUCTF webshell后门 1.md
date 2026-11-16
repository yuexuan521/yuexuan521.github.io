---
title: "BUUCTF webshell后门 1"
date: 2024-08-24 16:35:40
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "MISC"
- "Web安全"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190351253.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「webshell后门 1」，要求找出 webshell，密码的 md5 即答案](/images/blog/20251228190353358.png)

### 题目描述：

朋友的网站被黑客上传了webshell后门，他把网站打包备份了，你能帮忙找到黑客的webshell在哪吗？(Webshell中的密码(md5)即为答案)

### 密文：

下载附件，解压得到一个网站文件夹。

![站点目录结构：admin、data、ewebeditor、hack、inc、upload_files 等](/images/blog/20251228190355298.png)

---

### 解题思路：

1、使用工具扫描附件所给的文件夹，可以使用D盾、火绒安全之类的工具，这里我使用D盾。
打开D盾，点击自定义扫描，选择附件给的网站文件夹，点击确定。

![D盾_Web查杀主界面，红框标出「自定义扫描」](/images/blog/20251228190358121.png)

检查出两个文件，我们来挨个查看。

![扫描出两条记录，级别 3，说明分别是（内藏）Eval后门 与 base64_decode加密|system执行](/images/blog/20251228190400155.png)

2、第一个文件没有flag，但我们在第二个文件中找到了Webshell中的密码，密码也符合md5的格式，这个就是flag值。

![源码第 31 行 $pass = 'ba8e6c6f35a53933b871480bb9a9545c'; //angel](/images/blog/20251228190402659.png)

### flag：

```bash
flag{ba8e6c6f35a53933b871480bb9a9545c}
```