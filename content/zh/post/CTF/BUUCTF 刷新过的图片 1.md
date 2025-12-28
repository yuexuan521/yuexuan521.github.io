---
title: "BUUCTF 刷新过的图片 1"
date: 2024-09-22 20:29:39
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "安全"
- "笔记"
- "网络安全"
- "BUUCTF"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192652659.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192654710.png)

### 题目描述：

浏览图片的时候刷新键有没有用呢 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.jpg图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192656713.jpeg)

---

### 解题思路：

1、根据题目提示，结合键盘上的刷新键为F5，猜测本题使用了F5隐写，使用F5-steganography工具导出隐写内容。

安装F5-steganography
[https://gitcode.com/mirrors/matthewgao/f5-steganography/tree/master](https://gitcode.com/mirrors/matthewgao/f5-steganography/tree/master) 

```bash
Kali下载安装语句
git clone https://github.com/matthewgao/F5-steganography 
```

安装成功如下图：
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192658030.png)

出现错误参考这篇文章： [安装F5-steganography和使用，及解决java环境版本报错问题](https://blog.csdn.net/pythllc/article/details/128969843) 

2、将Misc.jpg文件放到F5-steganography目录下（也可以使用绝对路径），使用命令导出Misc.jpg图片的隐写内容到output.txt文件（在F5-steganography目录下）。

```bash
java Extract Misc.jpg 
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192700481.png)

查看output.txt文件，发现pk文件头，确认为zip压缩包，并且内部还有flag.txt文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192702635.png)

3、更改output.txt文件的后缀为.zip，尝试解压压缩包，需要密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192705006.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192706136.png)

因为题目没有关于密码的提示，猜测该zip压缩包为伪加密。
通过010Editor修改压缩源文件数据区和目录区的全局方式位标记（下图红色标识），将伪压缩文件恢复到未加密的状态。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192707781.png)

```bash
未加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为00 00

伪加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为09 00

真加密：

文件头中的全局方式位标记为09 00

目录中源文件的全局方式位标记为09 00

ps:也不一定要09 00或00 00，只要是奇数都视为加密，而偶数则视为未加密（有些题目需要全部修改为0）
```

保存文件，再次尝试解压，解压成功。打开flag.txt文件，获得flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192709798.png)

### flag：

```bash
flag{96efd0a2037d06f34199e921079778ee}
```