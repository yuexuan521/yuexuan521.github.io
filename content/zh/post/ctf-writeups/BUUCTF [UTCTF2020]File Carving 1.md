---
title: "BUUCTF [UTCTF2020]File Carving 1"
date: 2025-06-23 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
- "逆向"
- "隐写"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192056488.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [BUUCTF：［UTCTF2020］File Carving](https://blog.csdn.net/mochu7777777/article/details/109855105)
- [BUUCTF之misc ［UTCTF2020］File Carving](https://blog.csdn.net/m0_62107966/article/details/126806622)


 ![题目页「［UTCTF2020］File Carving 1」，附件 attachment.p…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192058540.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件，一张attachment.png图片

 ![格里菲斯天文台的昼景照片](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192059647.png)

---

### 解题思路：

1、根据题目，似乎和文件有关，先在010Editor看一下，然后发现ZIP压缩包。

 ![十六进制 1:F300h，红框 50 4B 03 04，右侧可见 hidden_binaryUT](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192101734.png)

另存为zip文件，解压得到hidden_binary文件。

 ![分离出的 hidden_binary 文件，大小 17 KB](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192104292.png)

2、用kali中的file命令确定hidden_binary的文件格式为ELF文件，在kali中执行得到flag： `utflag{2fbe9adc2ad89c71da48cabe90a121c0}` 。

> `hidden_binary` 是一个 ELF（Executable and Linkable Format）格式的 64 位可执行文件。ELF 是 Linux 和 Unix 系统中常用的可执行文件格式。

```python
file hidden_binary 
./hidden_binary  
```

 ![kali 里 file hidden_binary：ELF 64-bit LSB pie executable, x86-64, dynamically linked](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192105460.png)

 ![直接运行 ./hidden_binary，输出 Ah, you found me! 与 utflag{2fbe9adc2ad89c71da48cabe90a121c0}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192107291.png)

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

 ![strings hidden_binary | grep "flag" 命中 utflag{2H…，全量 strings 里能看到它被拆成多行](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192108873.png)

另一种方法：直接在010 Editor中搜索flag，找到后再进行处理。
 ![十六进制 1180h 处给出完整 utflag{2fbe9adc2ad89c71da48cabe90a121c0}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192110924.png)

### flag：

```bash
flag{2fbe9adc2ad89c71da48cabe90a121c0}
```