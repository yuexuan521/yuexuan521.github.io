---
title: "BUUCTF 变异凯撒 1"
date: 2024-09-25 21:34:48
category: "BUUCTF Crypto"
tags:
- "密码"
- "网络安全"
- "CTF"
- "CRTPTO"
- "Crypto"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205029283.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205031623.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

```
加密密文：afZ_r9VYfScOeO_UL^RWUc
格式：flag{ }

```

### 解题思路：

1、结合题目，直接给出加密类型为变异凯撒，只是我们不知道加密规则是什么。但是结合凯撒加密的加密原理（文章末尾有凯撒加密原理），我们根据给出的加密密文和格式，找出它们的ASCII码值。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205032995.jpeg)

```python
ASCII码值
a-->97
f-->102
Z-->90
_-->95
```

```python
ASCII码值
f-->102
l-->108
a-->97
g-->103
```

2、找出它们的对应关系，结合凯撒加密原理，得出：从第一个字母开始，每对一位字母进行加密，偏移量依次增加1（偏移量从5开始）。这就是本道题的加密规则。

```python
a-->97+5-->102-->f
f-->102+6-->108-->l
Z-->90+7-->97-->a
_-->95+8-->103-->g
```

3、在得到加密规则后，动手编写Python代码。

```python
txt = 'afZ_r9VYfScOeO_UL^RWUc'
j = 5
for i in txt:
    print(chr(ord(i)+j), end='')
    j += 1
```

4、执行代码，得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205034500.png)

### flag：

```
flag{Caesar_variation}
```

### 凯撒加密原理：

凯撒加密，也叫移位加密，是一种简单的加密方法。它的原理是将明文中的每个字母按照固定的偏移量向后（或向前）移动，得到密文。偏移量称为密钥，只有知道密钥的人才能解密。

例如，假设密钥是3，明文为“hello”，则加密后的密文为“khoor”。

凯撒加密是一种古老的加密方法，在历史上经常被用于保护军事、政治和商业机密。但是，由于它太过简单，容易被破解，现在已不再被广泛使用。