---
title: "BUUCTF [AFCTF2018]Morse 1"
date: 2024-09-25 21:37:33
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "buuctf"
- "网络安全"
- "密码"
- "ctf"
- "Crypto"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251213232842801.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251213232842802.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

```
-..../.----/-..../-..../-..../...--/--.../....-/-..../-..../--.../-.../...--/.----/--.../...--/..---/--.../--.../....-/...../..-./--.../...--/...--/-----/...../..-./...--/...--/...--/....-/...--/...../--.../----./--.../-..
```

---

### 解题思路：

1、观察密文，我想大家不陌生，看一眼题目，直接确定摩斯密码。
[摩斯密码加解密](https://rumkin.com/tools/cipher/morse-code/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251213232842803.png)

2、得到一次明显不是flag的东东，去除一下空格，看下字符串长度。
[文本去除工具](http://www.esjson.com/delSpace.html) 
[字符串长度计算](https://www.toolbaba.cn/d/dev_str_count) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251213232842804.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251213232842805.png)

3、看字符长度，显然不是md5，尝试base32、64后，仍无法解出结果。最后，是通过Hex编码，得到明文。
[Hex编码/解码](https://www.107000.com/T-Hex) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251213232842806.png)

---

### flag：

```python
afctf{1s't_s0_345y}
```

**提交的时候，记得把afctf换成要求的前缀名。** 