---
title: "BUUCTF [UTCTF2020]File Carving 1"
date: 2025-06-23 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "BUUCTF"
- "CTF"
- "misc"
- "ELF"
- "PNG隐写"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192056488.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：[UTCTF2020]File Carving](https://blog.csdn.net/mochu7777777/article/details/109855105) 
[BUUCTF之misc [UTCTF2020]File Carving](https://blog.csdn.net/m0_62107966/article/details/126806622) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192058540.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件，一张attachment.png图片

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192059647.png)

---

### 解题思路：

1、根据题目，似乎和文件有关，先在010Editor看一下，然后发现ZIP压缩包。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192101734.png)

另存为zip文件，解压得到hidden_binary文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192104292.png)

2、用kali中的file命令确定hidden_binary的文件格式为ELF文件，在kali中执行得到flag： `utflag{2fbe9adc2ad89c71da48cabe90a121c0}` 。

> `hidden_binary` 是一个 ELF（Executable and Linkable Format）格式的 64 位可执行文件。ELF 是 Linux 和 Unix 系统中常用的可执行文件格式。

```python
file hidden_binary 
./hidden_binary  
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192105460.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192107291.png)

另一种方法：使用 `strings` 命令来查看文件中包含的所有可打印字符串。

```shell
strings hidden_binary | grep "flag"
strings hidden_binary 
```

```python
utflag{2H
fbe9adc2H
ad89c71dH
a48cabe9H
0a121c0}H
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192108873.png)

另一种方法：直接在010 Editor中搜索flag，找到后再进行处理。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192110924.png)

### flag：

```bash
flag{2fbe9adc2ad89c71da48cabe90a121c0}
```