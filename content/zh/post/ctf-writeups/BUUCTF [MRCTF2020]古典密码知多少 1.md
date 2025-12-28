---
title: "BUUCTF [MRCTF2020]古典密码知多少 1"
date: 2024-09-25 20:36:30
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "CRYPTO"
- "密码学"
- "安全"
- "古典密码"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216093033670.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216093033672.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢天璇战队供题。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216093033673.jpeg)

### 解题思路：

1、观察图片密文，推测为猪圈密码等图形密码。密码种类参考这篇文章： [CTF中的一些图形密码](https://www.cnblogs.com/Nuy0ah/p/16138118.html#1%E4%BC%A0%E7%BB%9F%E7%8C%AA%E5%9C%88%E5%AF%86%E7%A0%81) ，一共使用了三种加密方式：传统猪圈密码、圣堂武士密码、标准银河字母加密。

2、通过对照密码表，手工解密，得到明文：FGCPFLIRTUASYON。 [字母大小写转换工具](https://charactercalculator.com/zh-cn/case-converter/) 

```
明文：FGCPFLIRTUASYON

小写：fgcpflirtuasyon
```

3、尝试提交失败。翻译图片下方的英文，得到栏栅加密和字符串全部大写的信息。通过栏栅加密得到明文：flagiscryptofun，去掉无用信息，转换成大写字母，得到flag：CRYPTOFUN。
[栅栏密码加密解密](https://www.qqxiuzi.cn/bianma/zhalanmima.php) 

### flag：

```
flag:CRYPTOFUN
```

**密码对照表：** 

***1.传统猪圈密码*** 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216093033674.png)

***2.圣堂武士密码*** 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216093033675.png)

***3.标准银河字母加密*** 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216093033676.png)

