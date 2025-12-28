---
title: "BUUCTF 大白 1"
date: 2024-09-24 22:40:30
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "密码学"
- "CTF"
- "Crypto"
- "安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192907272.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192909259.png)

### 题目描述：

看不到图？ 是不是屏幕太小了 。

### 密文：

下载附件后解压，发现一张名为dabai.png的图片。
（似乎因为文件被修改过，原图片无法放在这里，这张图片是我的截图。）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192911388.png)

### 解题思路：

1、根据题目信息和图片呈现出来的异常（图片只有一半），推测出图片被修改了图片高度。(从图片的属性中，也可以看出异常)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192913282.png)

2、将图片放在010Editor，修改图片宽高。我们来分析png文件格式，
首先，“89 50 4E 47 0D 0A 1A 0A”为标识png文件的八个字节的文件头标志。
然后是IHDR数据块，
“00 00 00 0D”说明IHDR头块长为13
”49 48 44 52“为IHDR标识（ASCII码为IHDR）

“00 00 02 A7”为图像的宽，24像素
”00 00 01 00“为图像的高，24像素
…

我们要修改的就是这串数字，将图像的高设置为与图像的宽一致，也就是将下图的”00 00 01 00“修改为“00 00 02 A7”。

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192914606.png)

修改后是这样：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192916407.png)

3、最后，将修改后的图片保存，查看图片拿到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192918194.png)

### flag：

```bash
flag{He1l0_d4_ba1}    //注意不要输错
```