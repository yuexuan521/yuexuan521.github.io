---
title: "BUUCTF [GXYCTF2019] CheckIn 1"
date: 2024-09-25 20:38:12
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "安全"
- "网络"
- "网络安全"
- "crypto"
- "ctf"
- "密码"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251215172455660.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251215172455661.png)

### 题目描述：

### 密文：

```c
dikqTCpfRjA8fUBIMD5GNDkwMjNARkUwI0BFTg==
```

---

### 解题思路：

1、观察密文，一眼Base64加密，使用在线工具 [Base64加解密](https://www.qqxiuzi.cn/bianma/base64.htm) ，得到另一串密文。

```
v)*L*_F0<}@H0>F49023@FE0#@EN
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251215172455662.png)

2、尝试了很多方法，都没有成功。最后，根据此密文的ASCII码值都处于33 ~ 126范围，确定为ROT47加密。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251215172455663.png)

3、使用在线工具 [ROT47加解密](https://www.qqxiuzi.cn/bianma/ROT5-13-18-47.php) ，得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251215172455664.png)

### flag：

```c
GXY{Y0u_kNow_much_about_Rot}
```

### 加密原理：

ROT5、ROT13、ROT18、ROT47 编码是一种简单的码元位置顺序替换暗码。此类编码具有可逆性，可以自我解密，主要用于应对快速浏览，或者是机器的读取，而不让其理解其意。

ROT5 是 rotate by 5 places 的简写，意思是旋转5个位置，其它皆同。下面分别说说它们的编码方式：
ROT5：只对数字进行编码，用当前数字往前数的第5个数字替换当前数字，例如当前为0，编码后变成5，当前为1，编码后变成6，以此类推顺序循环。
ROT13：只对字母进行编码，用当前字母往前数的第13个字母替换当前字母，例如当前为A，编码后变成N，当前为B，编码后变成O，以此类推顺序循环。
ROT18：这是一个异类，本来没有，它是将ROT5和ROT13组合在一起，为了好称呼，将其命名为ROT18。
ROT47：对数字、字母、常用符号进行编码，按照它们的ASCII值进行位置替换，用当前字符ASCII值往前数的第47位对应字符替换当前字符，例如当前为小写字母z，编码后变成大写字母K，当前为数字0，编码后变成符号_。用于ROT47编码的字符其ASCII值范围是33－126，具体可参考 [ASCII编码](https://www.qqxiuzi.cn/bianma/ascii.htm) 。