---
title: "BUUCTF 二维码 1"
date: 2024-05-24 22:39:08
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "密码学"
- "CTF"
- "BUUCTF"
- "笔记"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192523291.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「二维码 1」：注意：得到的 flag 请包上 flag{} 提交，附件 f4571698-e…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192525403.png)

### 题目描述：

下载附件后解压，发现一张名为QR_code.png的二维码图片。

### 密文：

![一张标准二维码，表面看不出异常](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192526985.png)

### 解题思路：

1、扫描二维码，得到信息“secret is here”，提交发现这个不是flag，但提示我们flag就在图片中。（可以使用QR research或者手机扫码）

![QR Research 解码：版本 2、纠错等级 H、掩码 7，内容只有一句 secret is here](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192528433.png)

2、使用010 Editor查看图片，发现可能有pk开头的zip压缩包，zip的创始人名字简写为PK。（也可以通过修改后缀名为.txt，查看到隐藏文件4number.txt。或者，通过Linux的cat命令查看）

![010 Editor：1D0h 处 45 4E 44 AE 42 60 82 之后紧跟 50 4B 03 04，Ascii 区可见 4number.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192529976.png)

![记事本打开整张图全是乱码，末尾也能看到 4number.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192532302.png)

![kali 里 cat QR_code.png，同样认得 IHDR、IEND 与 4number.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192533953.png)

（对第二点的补充：在使用010 Editer查看图片时，可通过分析png文件的格式，找到隐藏的zip压缩文件。）

```bash
00 00 00 00 49 45 4E 44 AE 42 60 82      //为png文件的结尾数据块（IEND）
```

![放大 1D0h 一带：IEND 的 CRC 之后就是 50 4B 03 04，红底标出 49 与 45 4E 44](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192536316.png)

（我们发现在png文件的结尾后，还有一部分的数据，分析它的文件头，得到zip文件的信息。）

```bash
50 4B 03 04     //是 ZIP 的文件头的重要标志
```

![再放大，50 4B 03 04 14 00 09 00 08 正好接在 42 60 82 后面](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192538500.png)

3、使用Kali中的binwalk工具进行检测，确实存在zip压缩包和4number.txt文件。

![binwalk：471（0x1D7）起是加密 Zip，compressed 29 / uncompressed 15，name: 4number.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192540294.png)

4、使用Kali中的foremost工具，分离出QR_code.png中的压缩文件，使用ls命令查看，得到一个output目录，查看output目录下的文件，找到zip文件。

![foremost QR_code.png，输出 foundat=4number.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192542424.png)

![kali 里 ls 有 4number.txt、flag.zip、Android.apk；ls output 是 audit.txt、png、zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192543989.png)

5、解压zip文件需要密码，尝试几个常用的密码后还是不行。使用工具fcrackzip进行暴力破解，得到密码7639，正好对应之前4number.txt的文件名。

![unzip 00000000.zip，4number.txt 要密码，输错就 skipping](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192546257.png)

![fcrackzip -b -c1 -l 4 -u 那个 zip，PASSWORD FOUND!!!!: pw == 7639](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192548609.png)

```bash
参数解释：
-b：			使用暴力破解
-c 1：			使用字符集，1指数字集合
-l 4-4：		指定密码长度，最小长度-最大长度
-u：			不显示错误密码，仅显示最终正确密码
```

5、使用密码解压zip压缩包，得到4number.txt文件，打开文件得到flag。（注意：得到的 flag 请包上 flag{} 提交）

![这次密码对了，正常 inflating: 4number.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192550528.png)

![cat 4number.txt → CTF{vjpw_wnoei}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192552480.png)

### flag：

```bash
flag{vjpw_wnoei} 
```

还有一种Windows平台下的破解流程，跟上面的流程几乎一样，有兴趣的朋友可以去看看。 [https://blog.csdn.net/weixin_45728231/article/details/120988424](https://blog.csdn.net/weixin_45728231/article/details/120988424) 