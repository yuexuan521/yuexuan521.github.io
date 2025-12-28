---
title: "BUUCTF [WUSTCTF2020]girlfriend 1"
date: 2024-11-18 12:58:09
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "dtmf2num"
- "CTF"
- "misc"
- "BUUCTF"
- "web安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192235955.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：[WUSTCTF2020]girlfriend](https://blog.csdn.net/mochu7777777/article/details/105412940) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192238168.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢 Iven Huang 师傅供题。
比赛平台：https://ctfgame.w-ais.cn/

### 密文：

下载附件，得到attachment .zip压缩包，解压后得到girlfriend.zip压缩包。最后材料是girlfriend.wav和题目描述.txt。

题目描述.txt内容如下：
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192239778.png)

---

### 解题思路：

1、尝试听girlfriend.wav音频文件，刚开始没有听出是手机键盘拨号声。后面看题解，得知是DTMF拨号音识别。
**Audacity显示** 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192240867.png)

使用dtmf2num程序识别拨号内容
[dtmf2num下载地址](https://link.csdn.net/?from_id=105412940&target=http://aluigi.altervista.org/mytoolz/dtmf2num.zip) 

```shell
dtmf2num.exe ..\资料\girlfriend.wav
dtmf2num.exe 音频文件位置
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192242495.png)

得到如下的按键顺序、次数

```bash
999*666*88*2*777*33*6*999*4*4444*777*555*333*777*444*33*66*3*7777
```

2、对应手机上九宫格输入，得到最后的字符串为 `YOUAREMYGIRLFRIENDS` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192244079.png)

```bash
YOUAREMYGIRLFRIENDS

999     --->   y
666     --->   o
88      --->   u
2       --->   a
777     --->   r
33      --->   e
6       --->   m
999     --->   y
4       --->   g
4444    --->   i
777     --->   r
555     --->   l
333     --->   f
777     --->   r
444     --->   i
33      --->   e
66      --->   n
3       --->   d
7777    --->   s

youaremygirlfriends
```

### flag：

大写提交不成功，切换小写

```bash
flag{youaremygirlfriends}
```