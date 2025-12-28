---
title: "BUUCTF 小易的U盘 1"
date: 2024-08-21 20:25:04
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "笔记"
- "网络安全"
- "BUUCTF"
- "Misc"
- "inf"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192934645.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192936731.png)

### 题目描述：

小易的U盘中了一个奇怪的病毒，电脑中莫名其妙会多出来东西。小易重装了系统，把U盘送到了攻防实验室，希望借各位的知识分析出里面有啥。请大家加油噢，不过他特别关照，千万别乱点他U盘中的资料，那是机密。 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.iso文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192938517.png)

---

### 解题思路：

1、用Bandzip打开.iso文件，看到内部有很多文件。（010 Editor可以看到Rar文件头，能用WinRAR直接解压）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192940265.png)

使用foremost分离文件，在output目录中找到一个rar文件。（可以修改文件后缀解压，殊途同归）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192942390.png)

最后都是一堆的文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192944440.png)

2、直接查看.inf文件，得到提示信息“autoflag - 副本 (32)”。（autorun.inf文件也可用于双击磁盘时，自动运行文件）

> INF是Device INFormation File的英文缩写，是Microsoft公司为硬件设备制造商发布其驱动程序推出的一种文件格式，是Windows操作系统下用来描述设备或文件等数据信息的文件。INF文件中包含硬件设备的信息或脚本以控制硬件操作。在INF文件中指明了硬件驱动该如何安装到系统中，源文件在哪里、安装到哪一个文件夹中、怎样在注册表中加入自身相关信息等等。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192946463.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192947800.png)

3、使用IDA打开autoflag - 副本 (32).exe文件，搜索flag，如下图得到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192949182.png)

### flag：

```bash
flag{29a0vkrlek3eu10ue89yug9y4r0wdu10}
```