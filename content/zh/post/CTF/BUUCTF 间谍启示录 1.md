---
title: "BUUCTF 间谍启示录 1"
date: 2024-09-21 20:39:36
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "安全"
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "隐藏文件"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193610283.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193612307.png)

### 题目描述：

在城际公路的小道上，罪犯G正在被警方追赶。警官X眼看他正要逃脱，于是不得已开枪击中了罪犯G。罪犯G情急之下将一个物体抛到了前方湍急的河流中，便头一歪突然倒地。警官X接近一看，目标服毒身亡。数分钟后，警方找到了罪犯遗失物体，是一个U盘，可惜警方只来得及复制镜像，U盘便报废了。警方现在拜托你在这个镜像中找到罪犯似乎想隐藏的秘密。 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，得到一个.iso文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193614154.png)

---

### 解题思路：

1、用Bandzip打开.iso文件，看到内部有很多文件。（010Editor也可以看到）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193615499.png)

使用foremost分离文件，在output目录中找到四个文件。（可以直接解压.iso文件，会得到很多文件）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193617370.png)

而在rar压缩包中，我们找到flag.exe文件。（解压systemzx.exe文件也可以得到flag.exe文件，使用WinRAR）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193619442.png)

2、解压压缩包，运行flag.exe，得到机密文件.txt文件，打开得到flag。（注意：获得的机密文件.txt，是隐藏文件，需要开启查看隐藏项目选项）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193621241.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193622641.png)

---

开启查看隐藏项目选项

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193624536.png)

### flag：

```bash
flag{379:7b758:g7dfe7f19:9464f:4g9231}
```