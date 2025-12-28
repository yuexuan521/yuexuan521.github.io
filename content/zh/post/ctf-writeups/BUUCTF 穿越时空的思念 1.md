---
title: "BUUCTF 穿越时空的思念 1"
date: 2024-09-22 20:33:04
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "安全"
- "笔记"
- "网络安全"
- "CTF"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193228483.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193230578.png)

### 题目描述：

嫦娥当年奔月后，非常后悔，因为月宫太冷清，她想：早知道让后羿自己上来了，带了只兔子真是不理智。于是她就写了一首曲子，诉说的是怀念后羿在的日子。无数年后，小明听到了这首曲子，毅然决定冒充后羿。然而小明从曲子中听不出啥来，咋办。。（该题目为小写的32位字符，提交即可） 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，得到一个.mp3文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193232523.png)

---

### 解题思路：

1、得到一个音频文件，放到Audacity看看。看到有两条音轨，放大下面的那条音轨，看到这一串音频，有很多分组，分组内由粗的音块和细的音块组成，类似莫尔斯电码的“-”和“.”。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193234322.png)

2、如下图，按照这样的对应将音轨上的分组全部转译为莫尔斯电码，一共有两段。

```bash
第一个红框代表“-.”，第二个红框代表“ ”（空格）。
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193235940.png)

```bash
第一段：..-. ----- ..--- ----. -... -.. -.... ..-. ..... ..... .---- .---- ...-- ----. . . -.. . -... ---.. . ....- ..... .- .---- --... ..... -... ----- --... ---.. -....
第二段：..-. ----- ..--- ----. -... -.. -.... ..-. .....
```

3、使用在线网站，将莫尔斯电码转换为明文字符。
[在线摩斯密码翻译器](https://rumkin.com/tools/cipher/morse-code/) 

第一段：

```bash
F029BD6F551139EEDEB8E45A175B0786
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193237650.png)

第二段：

```bash
F029BD6F5
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193239277.png)

4、两段明文字符有重复，将第一段转换为小写字母，作为flag值。（符合题目的要求：小写的32位字符）
[字母大小写转换工具](https://charactercalculator.com/zh-cn/case-converter/) 
[在线文本字符数统计工具](https://uutool.cn/str-count/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193240862.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193242311.png)

可怜我写错一个“-”，一直解不出来，真要命。（一定要细心！！！）

### flag：

```bash
flag{f029bd6f551139eedeb8e45a175b0786}
```