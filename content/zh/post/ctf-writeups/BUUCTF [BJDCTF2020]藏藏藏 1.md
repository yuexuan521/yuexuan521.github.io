---
title: "BUUCTF [BJDCTF2020]藏藏藏 1"
date: 2024-08-23 22:47:52
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190734376.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［BJDCTF2020］藏藏藏 1」，来源是 BJD 官方 GitHub](/images/blog/20251228190736857.png)

### 题目描述：

来源：https://github.com/BjdsecCA/BJDCTF2020

### 密文：

下载附件，解压得到一张.jpg图片和一个.txt文件。
 ![两个文件：藏藏藏.jpg（蓝盾 LANDUN 的 logo）与 题目及答案.txt](/images/blog/20251228190738424.png)

---

### 解题思路：

1、一张图片，典型的图片隐写。在010Editor中找到PK文件头，确认隐藏zip压缩包。放到Kali中，使用binwalk检测，也确认图片中有zip压缩包。（第一个“藏”）
 ![十六进制 C7F0h 处红框 FF D9 后紧跟 50 4B，再往后是 50 4B 03 04 与 .docx](/images/blog/20251228190739774.png)

 ![binwalk 藏藏藏.jpg：0 处 JPEG JFIF 1.01，63967（0xF9DF）End of Zip footer length 22](/images/blog/20251228190750764.png)

使用foremost分离图片中的压缩包，在output目录中找到隐藏的zip压缩包。
 ![foremost 分出 福利 .docx；tree ./output 得到 audit.txt、jpg/00000000.jpg、zip/00000099.zip](/images/blog/20251228190752579.png)

尝试解压压缩包，得到一个.docx文件。
 ![unzip 时提示 mismatching "local" filename，改用中央目录里的文件名继续](/images/blog/20251228190754623.png)

查看.docx文件，再次发现PK文件头。
 ![cat 福利 .docx 是 PK 开头的一堆 XML，能看到 docProps/app.xml](/images/blog/20251228190756416.png)

2、使用binwalk检测，发现有很多zip压缩包。（第二个“藏”）
 ![binwalk 福利 .docx 列出内部条目：docProps、word/styles.xml、word/theme/theme1.xml](/images/blog/20251228190758477.png)

使用foremost分离压缩包，在output目录中找到zip压缩包并解压，得到很多文件。
 ![foremost 福利 .docx 又分出 docProps/PK、app.xml、custom.xml 三段](/images/blog/20251228190800819.png)

 ![分离出的 00000000.zip 解开是完整 docx 结构：_rels、customXml、docProps、word](/images/blog/20251228190803438.png)

3、在很多文件中发现一个.png图片，查看一下，发现是一张二维码。（第三个“藏”）
 ![其中 word/media/image1.png 被绿色高亮](/images/blog/20251228190805561.png)

 ![这张 image1.png 其实是个二维码](/images/blog/20251228190807454.png)

使用QR research扫描二维码，得到flag。（也可以使用手机扫码）
 ![扫码结果：版本 3、纠错等级 Q、掩码 6，内容 flag{you are the best!}](/images/blog/20251228190809199.png)

### flag：

```bash
flag{you are the best!}
```