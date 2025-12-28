---
title: "BUUCTF 百里挑一 1"
date: 2024-12-16 07:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "CTF"
- "misc"
- "BUUCTF"
- "EXIF"
- "wireshark"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193134440.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[Hello CTF](https://hello-ctf.com/HC_Start/) 
[NewStar CTF](https://ns.openctf.net/learn/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193136490.png)

### 题目描述：

好多漂亮的壁纸，赶快挑一张吧！ 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件解压，得到pacap流量文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193138130.png)

---

### 解题思路：

1、双击打开文件，简单浏览发现存在大量的图片，将全部图片导出分析

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193139593.png)

得到的图片如下：

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193141386.jpeg)

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193143006.jpeg)

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193144782.jpeg)

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193146178.jpeg)

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193147584.jpeg)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193149409.jpeg)

简单看了几张，没有什么有用的信息

2、使用Kali上的exiftool工具，批量分析图片，寻找flag

> ExifTool是一款免费开源的图像信息查看工具，一个命令行应用程序。可用于读写和编辑图像（主要）、音视频和PDF等文件的元数据（metadata）。元数据是由一系列参数（下文为了与命令行参数做区别将称为标签）组成，如快门速度、光圈、白平衡、相机品牌和型号、镜头、焦距等等。而ExifTool可以帮助用户读取和处理这些数据， 支持许多不同的元数据格式，包括 EXIF，GPS，IPTC，XMP，JFIF，GeoTIFF，ICC 配置文件等等。支持多种输出格式设置选项（包括制表符分隔，HTML，XML 和 JSON），还可以多语言输出（cs，de，en，en-ca，en-gb，es，fi，fr，it，ja，ko，nl，pl，ru，sv，tr，zh-cn 或 zh-tw）。可以读取和写入许多数码相机的制造商说明。

得到一半的flag， `flag{ae58d0408e26e8f` 

```shell
exiftool * | grep flag
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193150869.png)

这一步可以使用其他的方法，按字符串分组字节流方式查找flag，也可以找到前半部分flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193152693.png)

3、经过以上的解题过程，猜测接下来的后半部分flag，也与exif信息有关。

> EXIF信息，是可交换图像文件的缩写，是专门为数码相机的照片设定的，可以记录数码照片的属性信息和拍摄数据。EXIF可以附加于JPEG、TIFF、RIFF等文件之中，为其增加有关数码相机拍摄信息的内容和索引图或图像处理软件的版本信息。

回到Wireshark，查找exif字符，追踪TCP流，就可以找到后半部分flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193154758.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193156836.png)

### flag：

```bash
flag{ae58d0408e26e8f26a3c0589d23edeec}
```