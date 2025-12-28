---
title: "BUUCTF [BJDCTF2020]纳尼 1"
date: 2024-07-23 10:54:24
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190717973.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190720047.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [https://github.com/BjdsecCA/BJDCTF2020](https://github.com/BjdsecCA/BJDCTF2020) 

### 密文：

下载附件，解压得到6.gif和题目.txt文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190721666.png)

---

### 解题思路：

1、查看题目.txt文件，得到提示“咦！这个文件怎么打不开？”，再看6.gif文件，果然无法打开。

 ![无法打开，初步猜测没有gif文件头](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190723032.gif)

用010Editor或WinHex打开，可以发现gif文件缺少相应的gif文件头。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190724804.png)

```bash
gif的文件头: 47 49 46 38
```

2、使用WinHex给gif文件加上gif的文件头，复制gif的文件头: “47 49 46 38“，将光标移动到数据最前端粘贴即可，然后保存文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190726599.png)

查看6.gif文件，已经可以正常打开啦。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190728401.gif)

3、可以看到正常的gif图片不断闪动某些字符（疑似Base64加密过的密文），可以使用StegSolve或者Photoshop查看，我的StegSolve无法显示，这里使用Photoshop为例。
[StegSolve的使用方法可以看这篇文章](https://blog.csdn.net/YueXuan_521/article/details/133816746?spm=1001.2014.3001.5502) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190730213.png)

查看不同图层的字符，记录下来，得到完整的密文。（是Base编码）

```bash
Q1RGe3dhbmdfYmFvX3FpYW5nX2lzX3NhZH0=
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190731817.png)

使用在线工具对密文进行解密，得到flag。
[BASE64加密解密](https://base64.supfree.net/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190733071.png)

### flag：

```bash
flag{wang_bao_qiang_is_sad}
```