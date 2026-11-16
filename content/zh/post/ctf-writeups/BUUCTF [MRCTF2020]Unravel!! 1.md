---
title: "BUUCTF [MRCTF2020]Unravel!! 1"
date: 2025-06-30 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191603745.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [BUUCTF：［MRCTF2020］Unravel!!](https://blog.csdn.net/mochu7777777/article/details/109671882)


 ![题目页「［MRCTF2020］Unravel!! 1」，感谢天璇战队供题，附件 attachment.rar](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191605778.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢天璇战队供题。

### 密文：

下载附件，解压得到Unravel文件夹，文件如下：

 ![三个文件：JM.png（399 KB）、Look_at_the_file_ending.wav（5,937 KB）、win-win.zip（4,842 KB）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191607940.png)

---

### 解题思路：

1、先看Look_at_the_file_ending.wav文件，既然已经提示我们看文件结尾，就用010Editor打开，得到一串密文。

```python
key=U2FsdGVkX1/nSQN+hoHL8OwV9iJB/mSdKk5dmusulz4=
```

 ![十六进制 C000h 处，红框记下一串 U2FsdGVkX1/nSQ…z4=](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191609317.png)

密文有点像Base64，尝试Base64解密，发现“ `Salted` ”头部，判断为AES加密，寻找密钥。

[Base64编码转换](https://www.qqxiuzi.cn/bianma/base64.htm) 

 ![Base64 解码（UTF-16）只得到 Salted__ 开头的乱码，说明还要密码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191611066.png)

2、接下来处理JM.png，在StegSolve看到“ `PK` ”头，存在隐藏ZIP压缩包。

 ![StegSolve 打开 JM.png，右侧 File Format Analysis 里红框 PK](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191612856.png)

用010 Editor打开，将压缩包的数据，提取保存为ZIP文件。

 ![十六进制 6340h 处 IEND 之后紧跟 PK 03 04，后面是 .png、IHDR、IDAT](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191615583.png)

解压得到aes.png文件，这应该就是AES的密钥。

```python
Tokyo
```

 ![提取出的图片上写着 Tokyo](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191617747.png)

3、进行AES解密，得到win-win.zip的解压密码： `CCGandGulu` 。

[在线AES加密/解密](https://www.sojson.com/encrypt_aes.html) 

 ![AES 加解密页：密文 CCGandGulu、密钥 Tokyo，上面是那串 U2FsdGVk…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191618884.png)

解压win-win.zip，得到Ending.wav。这里要使用SilentEye进行解密，下载地址： [https://achorein.github.io/silenteye/](https://achorein.github.io/silenteye/) 

 ![DeepSound 解码 wav，Decoded message 得到 MRCTF{Th1s_is_the_3nd1n9}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191622063.png)

得到flag： `MRCTF{Th1s_is_the_3nd1n9}` 

### flag：

```bash
flag{Th1s_is_the_3nd1n9}
```