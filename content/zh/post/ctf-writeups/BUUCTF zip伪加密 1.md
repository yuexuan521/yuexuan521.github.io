---
title: "BUUCTF zip伪加密 1"
date: 2024-08-24 22:48:07
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "笔记"
- "网络安全"
- "安全"
- "MISC"
- "CTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190433141.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190435366.png)

### 题目描述：

下载附件，得到一个zip压缩包。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190436962.png)

### 解题思路：

1、刚开始尝试解压，看到了flag.txt文件，但需要解压密码。结合题目，确认这是zip伪加密，不需要浪费时间寻找密码了。将文件在010 Editor中打开，分析它的文件格式，修改相关参数。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190438359.png)

[zip伪加密原理](https://blog.csdn.net/qq_26187985/article/details/83654197) 
2、通过修改压缩源文件数据区和目录区的全局方式位标记（下图红色标识），达到将伪压缩文件恢复到未加密的状态的目的。将下方的两个“09 00”修改为“00 00”，保存文件完成还原。

```bash
未加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为00 00

伪加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为09 00

真加密：

文件头中的全局方式位标记为09 00

目录中源文件的全局方式位标记为09 00

ps:也不一定要09 00或00 00，只要是奇数都视为加密，而偶数则视为未加密
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190440610.png)

3、将修改后的压缩包解压，获得flag.txt文件，打开文件得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190442820.png)

### flag：

```bash
flag{Adm1N-B2G-kU-SZIP}
```