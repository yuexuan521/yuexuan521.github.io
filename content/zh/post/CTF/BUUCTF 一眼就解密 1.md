---
title: "BUUCTF 一眼就解密 1"
date: 2023-06-05 13:46:02
category: "BUUCTF Crypto"
tags:
- "ctf"
- "crypto"
- "Base64"
- "密码"
- "网络安全"
---

```cobol
flag：ZmxhZ3tUSEVfRkxBR19PRl9USElTX1NUUklOR30= 
```

观察发现，密文字符串末尾有“=”，猜测为Base32/64加密。又因为密文中含有小写字母，排除Base32，使用Base64在线解密工具解密，获得flag。

```cobol
flag{THE_FLAG_OF_THIS_STRING}
```

<div style="text-align:center;"><img alt="" src="https://i-blog.csdnimg.cn/blog_migrate/77d05a5c47745bdcbd9dbaa753166f97.png"></div>

**简述：** 

**base32的编码表是由（A-Z、2-7）32个可见字符构成，“=”符号用作后缀填充。** 

**base64的编码表是由（A-Z、a-z、0-9、+、/）64个可见字符构成，“=”符号用作后缀填充。** 

**在线解密工具： [https://www.qqxiuzi.cn/bianma/base64.htm](https://www.qqxiuzi.cn/bianma/base64.htm)** 