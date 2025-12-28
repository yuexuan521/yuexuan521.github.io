---
title: "BUUCTF [SUCTF 2019]Game 1"
date: 2025-10-13 08:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "misc"
- "SUCTF 2019"
- "网络安全"
- "安全"
- "3DES"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191821473.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191823927.png)

### 题目描述：

感谢菠萝吹雪师傅出题。

flag 请替换 SUCTF{} 为 flag{} 后提交。

### 密文：

下载附件，得到一张图片和一个网站源代码

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191825575.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191828044.png)

---

### 解题思路：

1、在网站源代码index.html中，发现经过Base32加密后的密文： `ON2WG5DGPNUECSDBNBQV6RTBNMZV6RRRMFTX2===` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191829453.png)

[Base32编码解码](https://www.qqxiuzi.cn/bianma/base.php) 
使用在线网站进行解密，得到假的flag： `suctf{hAHaha_Fak3_F1ag}` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191831117.png)

2、转过方向，看看那张图片。发现存在LSB隐写，密文为： `U2FsdGVkX1+zHjSBeYPtWQVSwXzcVFZLu6Qm0To/KeuHg8vKAxFrVQ==` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191832778.png)

[Base64编码转换](https://www.qqxiuzi.cn/bianma/base64.htm) 
密文与Base64编码非常相似，并且Base64解码后头部是"Salted"，猜测加密方式为AES或3DES。（根据U2FsdGVkX1开头，也有同样效果）

> **3DES** (Triple DES): PKCS#5 的早期实现通常使用 3DES 加密算法。当使用 3DES 时，加密的数据块可能会以 “Salted” 开头，后面跟着一个随机生成的盐值，用于派生密钥。
> **AES** (Advanced Encryption Standard): 虽然 AES 通常不需要特定的前缀，但在某些实现中，如果使用 PKCS#5 或者类似的密码派生标准，也可能看到类似的前缀。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191834495.png)

3、加密方式是3DES，密钥为之前的假flag： `suctf{hAHaha_Fak3_F1ag}` ，解密得到flag
[TripleDes加密/解密](https://www.sojson.com/encrypt_triple_des.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191836631.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191838200.png)

### flag：

```bash
suctf{U_F0und_1t}
flag{U_F0und_1t}
```