---
title: "BUUCTF [BJDCTF2020]一叶障目 1"
date: 2024-09-21 10:58:04
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "安全"
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190651799.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190653815.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [https://github.com/BjdsecCA/BJDCTF2020](https://github.com/BjdsecCA/BJDCTF2020) 

### 密文：

下载附件，解压得到一张.png图片。
 ![这里显示不出来，暗示被修改了宽高参数。（放在Linux中也无法显示）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190655468.png)

---

### 解题思路：

1、在010Editor中打开，提示CRC校验错误，结合题目提示“一叶障目”，认为图片被修改了宽高。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190657303.png)

2、通过python脚本爆破宽高，得到正确的宽高，然后修改图片的宽高数据，得到正确的图片。爆破所用代码如下。

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

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190659146.png)

3、修改图片中的宽高参数，然后保存图片。（红框左边为宽度，右边为高度）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190700528.png)

打开图片，找到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190702273.png)

### flag：

```bash
flag{66666}
```