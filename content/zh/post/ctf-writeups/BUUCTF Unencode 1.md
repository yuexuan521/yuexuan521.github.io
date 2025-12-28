---
title: "BUUCTF Unencode 1"
date: 2024-09-25 21:41:03
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "Crypto"
- "网络安全"
- "CTF"
- "密码"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204912131.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204917942.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

```
89FQA9WMD<V1A<V1S83DY.#<W3$Q,2TM]
```

### 解题思路：

1、观察密文，尝试Base85、Base91等编码，均失败。
2、结合题目，联想到UUencode编码，尝试后成功，得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204919304.png)

[UUencode加解密](http://web.chacuo.net/charsetuuencode) 

### flag：

```
flag{dsdasdsa99877LLLKK}
```

### UUencode编码：

UUencode编码是一种基于ASCII编码的编码方式，它可以将二进制数据转换成可打印的ASCII字符，以便在邮件、新闻组等文本传输协议中传输。UUencode编码是Unix操作系统中原生支持的编码方式。

UUencode编码的基本原理是将3个字节（24位）的二进制数据分为4个6位的数据组，每个6位的数据组对应一个ASCII字符。编码的过程如下：

1. 将3个字节的二进制数据分成4组，每组6个位。

2. 对每组6位的数据分别加上一个固定值（通常是32），得到一个在可打印ASCII范围内的值。

3. 将这四个ASCII字符按顺序组成一个字符串。

4. 在编码的开头添加一个“begin”标识，结尾添加一个“end”标识。

5. 如果编码的数据长度不能被3整除，则在末尾添加1或2个0字节，使其长度能被3整除。

6. 在编码的开头添加一个mode标识，用于指定解码时的文件权限。

UUencode编码的缺点是编码比Base64更加复杂，编码后的数据量较大。但是，UUencode编码仍然在某些Unix系统中被广泛使用。