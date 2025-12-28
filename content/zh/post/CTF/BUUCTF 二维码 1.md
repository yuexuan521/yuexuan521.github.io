---
title: "BUUCTF 二维码 1"
date: 2024-09-24 22:39:08
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "密码学"
- "CTF"
- "Crypto"
- "BUUCTF"
- "笔记"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192523291.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192525403.png)

### 题目描述：

下载附件后解压，发现一张名为QR_code.png的二维码图片。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192526985.png)

### 解题思路：

1、扫描二维码，得到信息“secret is here”，提交发现这个不是flag，但提示我们flag就在图片中。（可以使用QR research或者手机扫码）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192528433.png)

2、使用010 Editor查看图片，发现可能有pk开头的zip压缩包，zip的创始人名字简写为PK。（也可以通过修改后缀名为.txt，查看到隐藏文件4number.txt。或者，通过Linux的cat命令查看）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192529976.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192532302.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192533953.png)

（对第二点的补充：在使用010 Editer查看图片时，可通过分析png文件的格式，找到隐藏的zip压缩文件。）

```bash
00 00 00 00 49 45 4E 44 AE 42 60 82      //为png文件的结尾数据块（IEND）
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192536316.png)

（我们发现在png文件的结尾后，还有一部分的数据，分析它的文件头，得到zip文件的信息。）

```bash
50 4B 03 04     //是 ZIP 的文件头的重要标志
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192538500.png)

3、使用Kali中的binwalk工具进行检测，确实存在zip压缩包和4number.txt文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192540294.png)

4、使用Kali中的foremost工具，分离出QR_code.png中的压缩文件，使用ls命令查看，得到一个output目录，查看output目录下的文件，找到zip文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192542424.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192543989.png)

5、解压zip文件需要密码，尝试几个常用的密码后还是不行。使用工具fcrackzip进行暴力破解，得到密码7639，正好对应之前4number.txt的文件名。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192546257.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192548609.png)

```bash
参数解释：
-b：			使用暴力破解
-c 1：			使用字符集，1指数字集合
-l 4-4：		指定密码长度，最小长度-最大长度
-u：			不显示错误密码，仅显示最终正确密码
```

5、使用密码解压zip压缩包，得到4number.txt文件，打开文件得到flag。（注意：得到的 flag 请包上 flag{} 提交）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192550528.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192552480.png)

### flag：

```bash
flag{vjpw_wnoei} 
```

还有一种Windows平台下的破解流程，跟上面的流程几乎一样，有兴趣的朋友可以去看看。 [https://blog.csdn.net/weixin_45728231/article/details/120988424](https://blog.csdn.net/weixin_45728231/article/details/120988424) 