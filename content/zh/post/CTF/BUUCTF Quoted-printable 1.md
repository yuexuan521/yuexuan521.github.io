---
title: "BUUCTF Quoted-printable 1"
date: 2024-09-25 21:32:24
category: "BUUCTF Crypto"
tags:
- "网络安全"
- "CTF"
- "密码"
- "md5"
- "Crypto"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204846945.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204849145.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

```python
=E9=82=A3=E4=BD=A0=E4=B9=9F=E5=BE=88=E6=A3=92=E5=93=A6
```

### 解题思路：

1、观察密文，结合题目，直接确定为Quoted-printable编码。
2、使用在线工具进行解密。 [在线工具](http://www.mxcz.net/tools/QuotedPrintable.aspx) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204851018.png)

3、得到flag，那你也很棒哦。

### flag：

```
那你也很棒哦
```

### Quoted-printable编码：

Quoted-printable编码是一种二进制数据在Internet上传输时的一种编码方式。它将二进制数据转换成可打印的ASCII字符。这种编码方式将每个非可打印字符(ASCII值小于32或大于126)，如二进制数据的控制字符或扩展字符(如汉字)，转换为一个等号"=“加上它的ASCII值的16进制表示，如”\x0A"会变成"=0A"。这个编码方式的目的是确保数据可以安全地在网络上传输，尤其是当数据包含非ASCII字符时。它是多用途互联网邮件扩展（MIME) 一种实现方式。有时候我们可以邮件头里面能够看到这样的编码；

**​ 特征：** 

任何一个8位的字节值可编码为3个字符：一个等号”=”后跟随两个十六进制数字(0–9或A–F)表示该字节的数值.