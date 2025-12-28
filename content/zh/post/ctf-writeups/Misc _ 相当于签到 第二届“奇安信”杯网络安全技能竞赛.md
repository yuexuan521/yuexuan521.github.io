---
title: "Misc | 相当于签到 第二届“奇安信”杯网络安全技能竞赛"
date: 2024-09-24 22:33:24
category: "“奇安信”杯网络安全技能竞赛"
categories: 
  - "CTF"
tags:
- "web安全"
- "安全"
- "MISC"
- "网络安全"
- "笔记"
- "CTF"
- "奇安信"
---



---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459175.png)

### 题目描述：

图片似乎经过了什么处理，你能否将其复原呢？

### 密文：

下载附件，解压得到一张.jpg图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459176.jpeg)

---

### 解题思路：

1、一张图片，典型的图片隐写。放到Kali中，使用binwalk检测，确认图片中隐藏zip压缩包。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459177.png)

使用foremost分离图片中的压缩包，在output目录中找到隐藏的zip压缩包。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459178.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459179.png)

2、尝试解压得到的压缩包，需要密码。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459180.png)

因为没有关于密码的提示，尝试用4位纯数字进行爆破，出现提示“no usable files found”（未找到可用文件）。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459181.png)

[zip伪加密原理](https://blog.csdn.net/qq_26187985/article/details/83654197) 
猜测该zip压缩包为伪加密，通过010Editor修改压缩源文件数据区和目录区的全局方式位标记（下图红色标识），将伪压缩文件恢复到未加密的状态。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459182.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459183.png)

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

ps:也不一定要09 00或00 00，只要是奇数都视为加密，而偶数则视为未加密
```

3、修改后，解压压缩包不需要密码，解压成功，得到一张.png图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459184.png)

从这里也可以看出图片经过修改，无法正常显示。在010 Editor中打开，提示CRC校验错误，结合题目提示“图片似乎经过了什么处理”，认为图片被修改了宽高。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459185.png)

通过爆破宽高，得到正确的宽高，然后修改图片的宽高数据，得到正确的图片。爆破所用代码如下。

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

得到正确的宽高值。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459186.png)

修改图片中的宽高参数，然后保存图片查看。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459187.png)

获得flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251210144459188.png)

### flag：

```bash
flag{bdbace45-506b-f530-aa4d-57884b2025e}(”-“可能为“_“)
```