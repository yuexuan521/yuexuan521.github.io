---
title: "BUUCTF [ACTF新生赛2020]music 1"
date: 2025-06-28 20:16:15
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
- "音频隐写"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190443925.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [m4a文件格式分析](https://blog.csdn.net/liuxunfei15/article/details/120441383)
- [异或运算 XOR 教程](https://www.ruanyifeng.com/blog/2021/01/_xor.html)
- [BUUCTF：［ACTF新生赛2020］music](https://blog.csdn.net/mochu7777777/article/details/109806994)


 ![题目页「［ACTF新生赛2020］music 1」，附件 attachment.tar](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190445904.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到tmp文件夹，内有vip.zip，解压得到vip.m4a文件。

 ![解出 vip.m4a（372 KB）与 vip.zip（368 KB）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190447569.png)

---

### 解题思路：

1、打开vip.m4a文件，发现该文件已损坏。

 ![播放器打不开 vip.m4a，提示文件类型不受支持，错误码 0xC00D36C4](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190448858.png)

在010Editor中，也可以观察到文件数据的混乱。

 ![十六进制从头到尾都是 A1 A1 A1 混着零星字节](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190450366.png)

2、通过观察，参考 [m4a文件格式分析](https://blog.csdn.net/liuxunfei15/article/details/120441383) 、 [异或运算 XOR 教程](https://www.ruanyifeng.com/blog/2021/01/_xor.html) 两篇文章，我终于明白，vip.m4a文件数据经过对“ `A1` ”的异或运算，才呈现上面的样子。

首先，通过对比m4a文件的ftyp（文件标识）和stsc（记录每个trunk的采样数），发现原来是“ `00` ”的数据变成了“ `A1` ”，ftyp：“ `66 74 79 70` ”变为“ `C7 D5 D8 D1` ”，stsc：“ `73 74 73 63` ”变为“ `D2 D5 D5 D2` ”。

（感谢CSDN半岛铁盒博主的分享，另外周的《半岛铁盒》也很好听）

**ftyp：** 
 ![红框 66 74 79 70 就是 ftyp，后面紧跟 4D 34 41，即 M4A](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190452337.png)

 ![0000h 处红框三组：A1 A1 A1、C7 D5 D8 D1、A1 A1 A1，说明每个字节都被异或过](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190453759.png)

**stsc：** 
 ![0094ebb0h 处 73 74 73 63 即 stsc，后面 00 01、00 02 被竖线标出](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190455552.png)

 ![5C1E0h 一带满屏 A1，右侧蓝色高亮 D2 D5，红框圈出非 A1 的字节](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190457661.png)

同时，我了解到异或运算的一条运算规律： **一个值与 0 的运算，总是等于其本身。** 

```bash
x ^ 0 = x
```

所以，当“ `A1` ”与 `0` 进行异或运算时，结果为“ `A1` ”。这就是为什么原来是“ `00` ”的数据变成了“ `A1` ”。

从而得出， **vip.m4a文件原数据经过了对“ `A1` ”的异或运算** 。

3、在010 Editor中，对整个文件进行对“ `A1` ”的异或运算，保存得到完好的vip.m4a文件。

 ![010 Editor 工具菜单，选「十六进制运算 → 二进制异或」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190459369.png)

 ![十六进制运算窗口：二进制异或，操作数填 A1，按位逐字节异或](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190501187.png)

播放vip.m4a文件，将听到的字母记下来，得到： `actfabcdfghijk` ，flag为： `abcdfghijk` 

### flag：

```bash
flag{abcdfghijk}
```