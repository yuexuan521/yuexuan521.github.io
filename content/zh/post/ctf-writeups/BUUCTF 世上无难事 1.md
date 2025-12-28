---
title: "BUUCTF 世上无难事 1"
date: 2024-09-25 20:45:44
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "密码"
- "Crypto"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204927685.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204929989.png)

### 题目描述：

以下是某国现任总统外发的一段指令，经过一种奇异的加密方式，毫无规律，看来只能分析了。请将这段语句还原成通顺语句，并从中找到key作为答案提交，答案是32位，包含小写字母。 注意：得到的 flag 请包上 flag{} 提交

### 密文：

```c
VIZZB IFIUOJBWO NVXAP OBC XZZ UKHVN IFIUOJBWO HB XVIXW XAW VXFI X QIXN VBD KQ IFIUOJBWO WBKAH NBWXO VBD XJBCN NKG QLKEIU DI XUI VIUI DKNV QNCWIANQ XN DXPIMKIZW VKHV QEVBBZ KA XUZKAHNBA FKUHKAKX XAW DI VXFI HBN QNCWIANQ NCAKAH KA MUBG XZZ XEUBQQ XGIUKEX MUBG PKAWIUHXUNIA NVUBCHV 12NV HUXWI XAW DI XUI SCQN QB HZXW NVXN XZZ EBCZW SBKA CQ NBWXO XAW DI DXAN NB NVXAP DXPIMKIZW MBU JIKAH QCEV XA BCNQNXAWKAH VBQN HKFI OBCUQIZFIQ X JKH UBCAW BM XLLZXCQI XAW NVI PIO KQ 640I11012805M211J0XJ24MM02X1IW09
```

---

### 解题思路：

#### 第一种方法：

1、仔细阅读题目，得到答案的位数为32、包含小写字母的信息，通过位数统计，可以确定最后一串字符为答案。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204931709.png)

2、对整个密文进行小写处理，便于观察规律。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204933355.png)

3、根据第一步以及题目中的“key”信息，key=pio，通过工具 [quipqiup](http://quipqiup.com/) 进行暴力破解。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204935561.png)

4、得到key，包上 flag{} 提交。

#### 第二种方法：

1、直接使用工具 [quipqiup](http://quipqiup.com/) 进行暴力破解

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204937728.png)

2、将得到的key转换成小写格式，包上 flag{} 提交。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204940060.png)

### flag：

```c
640e11012805f211b0ab24ff02a1ed09
```