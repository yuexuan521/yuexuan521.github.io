---
title: "BUUCTF Windows系统密码 1"
date: 2024-09-27 10:39:47
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "Crypto"
- "密码"
- "md5"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204921015.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204923236.png)

### 题目描述：

```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
ctf:1002:06af9108f2e1fecf144e2e8adef09efd:a7fcb22a88038f35a8f39d503e7f0062:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SUPPORT_388945a0:1001:aad3b435b51404eeaad3b435b51404ee:bef14eee40dffbc345eeb3f58e290d56:::
```

### 解题过程：

1、统计密文长度，为32位，且符合md5加密特征，初步判断为md5加密。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204924898.png)

2、对密文依次进行解密，排除不正确的结果。
**在线解密网站：** [md5在线解密](https://www.cmd5.com/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204926232.png)

3、最终得到结果

### flag:

```
good-luck
```

### MD5 简述：

 一般MD5值是32位由数字“0-9”和字母“a-f”所组成的字符串，字母大小写统一；如果出现这个范围以外的字符说明这可能是个错误的md5值，就没必要再拿去解密了。

 16位值是取的是8~24位。

**​ 特征：** 

 有固定长度，一般是32位或者16位

 由数字“0-9”和字母“a-f”组成

**举例：** 

```java
明文：hello，world.123456
md5(hello，world.123456,32) = 5189503aae1b1c0a6fbf7ea9e3128ab0
md5(hello，world.123456,16) = ae1b1c0a6fbf7ea9
```