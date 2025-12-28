---
title: "BUUCTF [ACTF新生赛2020]music 1"
date: 2025-06-28 20:16:15
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "安全"
- "CTF"
- "misc"
- "music"
- "ACTF新生赛2020"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190443925.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[m4a文件格式分析](https://blog.csdn.net/liuxunfei15/article/details/120441383) 
[异或运算 XOR 教程](https://www.ruanyifeng.com/blog/2021/01/_xor.html) 
[BUUCTF：[ACTF新生赛2020]music](https://blog.csdn.net/mochu7777777/article/details/109806994) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190445904.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到tmp文件夹，内有vip.zip，解压得到vip.m4a文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190447569.png)

---

### 解题思路：

1、打开vip.m4a文件，发现该文件已损坏。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190448858.png)

在010Editor中，也可以观察到文件数据的混乱。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190450366.png)

2、通过观察，参考 [m4a文件格式分析](https://blog.csdn.net/liuxunfei15/article/details/120441383) 、 [异或运算 XOR 教程](https://www.ruanyifeng.com/blog/2021/01/_xor.html) 两篇文章，我终于明白，vip.m4a文件数据经过对“ `A1` ”的异或运算，才呈现上面的样子。

首先，通过对比m4a文件的ftyp（文件标识）和stsc（记录每个trunk的采样数），发现原来是“ `00` ”的数据变成了“ `A1` ”，ftyp：“ `66 74 79 70` ”变为“ `C7 D5 D8 D1` ”，stsc：“ `73 74 73 63` ”变为“ `D2 D5 D5 D2` ”。

（感谢CSDN半岛铁盒博主的分享，另外周的《半岛铁盒》也很好听）

**ftyp：** 
 ![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190452337.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190453759.png)

**stsc：** 
 ![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190455552.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190457661.png)

同时，我了解到异或运算的一条运算规律： **一个值与 0 的运算，总是等于其本身。** 

```bash
x ^ 0 = x
```

所以，当“ `A1` ”与 `0` 进行异或运算时，结果为“ `A1` ”。这就是为什么原来是“ `00` ”的数据变成了“ `A1` ”。

从而得出， **vip.m4a文件原数据经过了对“ `A1` ”的异或运算** 。

3、在010 Editor中，对整个文件进行对“ `A1` ”的异或运算，保存得到完好的vip.m4a文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190459369.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190501187.png)

播放vip.m4a文件，将听到的字母记下来，得到： `actfabcdfghijk` ，flag为： `abcdfghijk` 

### flag：

```bash
flag{abcdfghijk}
```