---
title: "BUUCTF MD5"
date: 2023-06-05 14:06:56
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "密码学"
- "网络安全"
- "CTF"
---

密文：

```cobol
e00cf25ad42683b3df678c61f42c6bda
```

**简述：** 

一般MD5值是32位由数字“0-9”和字母“a-f”所组成的字符串，字母大小写统一；如果出现这个范围以外的字符说明这可能是个错误的md5值，就没必要再拿去解密了。

**特征：** 

 有固定长度，一般是32位或者16位

 由数字“0-9”和字母“a-f”组成

直接进行在线解密

<div style="text-align:center;"><img alt="MD5 在线查询页，密文 e00cf25ad42683b3df678c61f42c6bda 查询结果为 admin1" src="/images/blog/csdn_9e5e7bd7ff1e33d7a8ba26adc6f19271.png" loading="lazy" decoding="async" width="1102" height="432"></div>

在线解密工具： [https://www.cmd5.com/](https://www.cmd5.com/) 