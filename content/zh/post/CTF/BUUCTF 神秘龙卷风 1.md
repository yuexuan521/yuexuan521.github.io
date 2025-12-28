---
title: "BUUCTF 神秘龙卷风 1"
date: 2024-09-24 16:40:33
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "笔记"
- "网络安全"
- "安全"
- "MISC"
- "zip"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193158696.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193200847.png)

### 题目描述：

神秘龙卷风转转转，科学家用四位数字为它命名，但是发现解密后居然是一串外星人代码！！好可怕！

### 密文：

下载附件，解压得到一个.rar压缩包。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193202425.png)

---

### 解题思路：

1、解压压缩包需要密码，根据题目得知密码为四位纯数字，在RARP中打开压缩包，选择字符范围和密码长度，点击开始破解。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193203655.png)

得到压缩包密码为5463。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193205887.png)

2、解压压缩包，得到神秘龙卷风.txt文件。打开后，显示内容如下：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193207460.png)

内容由“+”、“.”、“>”三种符号组成，我刚开始认为这是一种密文，经过搜索，确认这是一种名为“Brainfuck”的计算机语言。
[Brainfuck](https://baike.baidu.com/item/Brainfuck) 

3、结合题目提示的“代码”，我找到一个brainfuck 在线工具可以运行brainfuck代码。将记事本中的代码复制到在线工具中，运行得到flag。
[Brainfuck 在线工具](https://www.w3cschool.cn/tryrun/runcode?lang=brainfuck) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193209294.png)

### flag：

```bash
flag{e4bbef8bdf9743f8bf5b727a9f6332a8}
```