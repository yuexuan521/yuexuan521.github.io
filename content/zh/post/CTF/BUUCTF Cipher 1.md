---
title: "BUUCTF Cipher 1"
date: 2024-09-25 20:40:06
category: "BUUCTF Crypto"
tags:
- "网络安全"
- "密码"
- "crypto"
- "ctf"
- "安全"
- "古典密码"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204830302.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204832579.png)

### 题目描述：

还能提示什么呢？公平的玩吧（密钥自己找） Dncnoqqfliqrpgeklwmppu 注意：得到的 flag 请包上 flag{} 提交, flag{小写字母}

### 密文：

```python
Dncnoqqfliqrpgeklwmppu
```

---

### 解题思路：

1、仔细阅读题目，从“公平的玩吧”一句中，得到加密方法，为playfair密码。

2、使用在线工具 [playfair加密解密](http://www.metools.info/code/playfair_186.html) 进行解密。

3、要求输入密钥，密钥提示是“公平的玩吧”一句，所指出的“playfair”密码，密钥为“playfair”。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204834115.png)

4、输入密文、密钥，得到明文为“itisnotaproblemhavefun”。

### flag：

```c
itisnotaproblemhavefun
```

### playfair密码原理：

Playfair密码是一种经典的双字母替换密码，使用一个5x5的矩阵来进行加密和解密。在这个矩阵中，字母J通常与I合并，因为它们在许多语言中具有相似的发音和外观。

**加密过程如下：** 

1. 去掉明文中所有非字母字符并将所有字母转换为大写。

2. 如果明文中有连续重复的字母，插入一个X或Q来分隔它们。

3. 如果明文的长度为奇数，可以在末尾添加一个X或Q来使其成为偶数。

4. 将明文拆分成两个字母一组，如果有奇数个字母，则最后一组只有一个字母。

5. 对于每一组字母，使用以下步骤进行加密：

- 如果两个字母在矩阵的同一行，则用该行相邻的字母进行替换，并保持其在同一行。

- 如果两个字母在矩阵的同一列，则用该列相邻的字母进行替换，并保持其在同一列。

- 如果两个字母在矩阵的不同行不同列，则用与第一个字母在同一行的第二个字母和与第二个字母在同一行的第一个字母进行替换，并保持其在同一行和同一列。

1. 将加密后的每组字母连接起来形成密文。

解密过程与加密过程相反。