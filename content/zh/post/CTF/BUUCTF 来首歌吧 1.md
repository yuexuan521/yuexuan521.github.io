---
title: "BUUCTF 来首歌吧 1"
date: 2024-09-23 22:56:49
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "安全"
- "Misc"
- "音频隐写"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193038727.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193040915.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.wav音频文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193042589.png)

---

### 解题思路：

1、得到一个音频文件，放到Audacity看看。看到有两条音轨，放大上面的那条音轨，看到这一串音频，有很多分组，分组内由粗的音块和细的音块组成，类似莫尔斯电码的“-”和“.”。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193043783.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193045628.png)

2、举例如下图。按照这样的对应将音轨上的分组全部转译为莫尔斯电码，转换为“… -… -.-. ----. …— … -… …- ----. -.-. -… ----- .---- —… —… …-. … …— . -… .---- --… -… --… ----- ----. …— ----. .---- ----. .---- -.-. ”。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193047437.png)

```bash
第一个红框代表“-.”，第二个红框代表“ ”（空格）。
..... -... -.-. ----. ..--- ..... -.... ....- ----. -.-. -... ----- .---- ---.. ---.. ..-. ..... ..--- . -.... .---- --... -.. --... ----- ----. ..--- ----. .---- ----. .---- -.-. 
```

3、使用在线网站，将莫尔斯电码转换为明文字符，得到flag值。
[在线网站](https://rumkin.com/tools/cipher/morse-code/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193048806.png)

### flag：

```bash
flag{5BC925649CB0188F52E617D70929191C}
```