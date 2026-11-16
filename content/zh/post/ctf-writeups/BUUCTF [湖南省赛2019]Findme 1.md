---
title: "BUUCTF [湖南省赛2019]Findme 1"
date: 2025-01-13 09:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
- "MISC"
- "隐写"
---

![文章题图：蓝发动漫少女，配文「刷题嘛~？一起呀」](/images/blog/20251228192422078.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [Hello CTF](https://hello-ctf.com/HC_Start/)
- [NewStar CTF](https://ns.openctf.net/learn/)
- [BuuCTF难题详解| Misc | ［湖南省赛2019］Findme](https://blog.csdn.net/pone2233/article/details/107990787)
- [［湖南省赛2019］Findme](https://www.cnblogs.com/zhoujiaff/p/17071345.html)


 ![BUUCTF 题目「［湖南省赛2019］Findme 1」页面，附件为 attachment.zip](/images/blog/20251228192424087.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到1.png、2.png、3.png、4.png、5.png。

 ![解压出的 1.png 到 5.png，其中 1.png 尺寸明显异常](/images/blog/20251228192426123.png)

---

### 解题思路：

1、先看第一张图片1.png，显示出错，而且与其它四张图片不同，尝试修改宽高。

通过python脚本爆破宽高，得到正确的宽高，然后修改图片的宽高数据，得到正确的图片。爆破所用代码如下。

```python
import os
import binascii
import struct

crcbp = open("repair.png", "rb").read()    #打开图片（修改图片路径）
for i in range(2000):
    for j in range(2000):
        data = crcbp[12:16] + \
            struct.pack('>i', i)+struct.pack('>i', j)+crcbp[24:29]
        crc32 = binascii.crc32(data) & 0xffffffff
        if(crc32 == 0x9BF1293B):    #图片当前CRC（修改CRC）
            print(i, j)
            print('hex:', hex(i), hex(j))
```

> 宽为 227， 高为 453， 新CRC值为 806611， 宽为 e3， 高为 1c5

正确的图片显示如下，还是有问题。
 ![1.png 打开后只剩一条极窄的横条，说明高度被改过](/images/blog/20251228192428376.png)

用010Editor打开，发现struct PNG_CHUNK `chunk[2]` 、 `chunk[3]` 缺少 `IDAT` 标识。

 ![010 Editor 中 chunk［2］、chunk［3］ 缺少 IDAT 标识（红框处）](/images/blog/20251228192429862.png)

添加 `IDAT` 标识，在struct PNG_CHUNK `chunk[2]` 下的 `union CTYPE type` 中的 `uint32 crc` ，添加值 `49444154h` 

 ![在 chunk［2］ 的 CTYPE 处补回 IDAT（49444154）](/images/blog/20251228192431495.png)

图片正常显示如下：

 ![修复后的 1.png 正常显示，是一张套娃照片](/images/blog/20251228192433666.png)

使用Stegsolve打开，在 `blue 2` 通道发现二维码。

 ![原文配图已失效（源站返回占位图）](/images/blog/20251228192435675.png)

 ![原文配图已失效（源站返回占位图）](/images/blog/20251228192437073.png)

扫码二维码，得到 `ZmxhZ3s0X3` 。

2、第二张图片2.png，用010 Editor打开，在文件尾发现 `7z` 开头的数据，提取出来再处理。

 ![2.png 尾部 IEND 之后跟着 50 4B 03 04 与 0.txt 等文件名，说明附了 ZIP](/images/blog/20251228192438964.png)

保存为7z文件后，无法正常解压。将文件数据中全部的 `7z` 替换为 `PK` ，保存为zip文件，可以正常解压。

> 7z文件头： `37 7A BC AF 27 1C` 
> zip文件头： `50 4B 03 04` 

 ![解压后得到 0.txt 至 15.txt 等大量文本文件](/images/blog/20251228192441296.png)

解压得到1000个文件。

 ![十六进制中成片出现 50 4B 03 04（PK 头）](/images/blog/20251228192443393.png)

将文件按照文件大小，从大到小排列，发现618.txt

 ![Kali 终端按大小排序，618.txt 大小为 21 字节](/images/blog/20251228192445482.png)

 ![文件列表中 618.txt 被红框标出](/images/blog/20251228192446813.png)

618.txt内容如下：

 ![618.txt 的内容：You find it: 1RVcmVfc](/images/blog/20251228192448370.png)

得到 `1RVcmVfc` 

3、第三张图片3.png，用010 Editor打开，在struct PNG_CHUNK chunk[0、1、2、3、4、5、6、7]下的 `uint32 crc` ，全部隐藏了十六进制数据。

 ![010 Editor 中多处 CRC 字段被红框标出](/images/blog/20251228192449809.png)

16进制转换文本： [https://www.sojson.com/hexadecimal.html](https://www.sojson.com/hexadecimal.html) 
 ![在线进制转换：33526C5A33303D 转成 3RIZ30=](/images/blog/20251228192451695.png)

提取出来转换为ASCII字符，得到 `3RlZ30=` 

4、第四张图片4.png，使用Kali中的exiftool查看4.png的EXIF信息。

 ![exiftool 查看 4.png，Artist 字段写着 another part:cExlX1BsY](/images/blog/20251228192453102.png)

在Artist下发现another part： `cExlX1BsY` 

5、第五张图片5.png，用010 Editor打开，在最后发现 a gift。

 ![5.png 尾部 IEND 之后写着 a gift:Yzcllfc0lN](/images/blog/20251228192455621.png)

得到 `Yzcllfc0lN` 

6、按照1.png、5.png、4.png、2.png、3.png的顺序组合，得到可以解码的Base64字符串。

```python
ZmxhZ3s0X3Yzcllfc0lNcExlX1BsY1RVcmVfc3RlZ30=
```

解码得到flag： `flag{4_v3rY_sIMpLe_PlcTUre_steg}` 

 ![Base64 解码得到 flag{4_v3rY_sIMpLe_PlcTUre_steg}](/images/blog/20251228192457825.png)

### flag：

```bash
flag{4_v3rY_sIMpLe_PlcTUre_steg}
```