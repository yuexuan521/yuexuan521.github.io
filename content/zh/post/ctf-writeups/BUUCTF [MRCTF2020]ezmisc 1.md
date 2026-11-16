---
title: "BUUCTF [MRCTF2020]ezmisc 1"
date: 2024-06-21 23:02:42
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228191502878.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［MRCTF2020］ezmisc 1」，附件 attachment.zip](/images/blog/20251228191504883.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢Galaxy师傅供题。

### 密文：

下载附件，解压得到.png图片。

![一张「图片违规！」的占位图，笑脸 X 眼配一个对话框](/images/blog/20251228191506932.png)

从这里也可以看出图片经过修改，无法正常显示。

---

### 解题思路：

1、在010Editor中打开，提示CRC校验错误，认为图片被修改了宽高，不符合CRC校验。

 ![Notepad++ 查找结果报 *ERROR: CRC Mismatch @ chunk［0］; in data: 370c8f0b; expected: 3f9ba6aa](/images/blog/20251228191508692.png)

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

 ![程序输出 hex: 0x1f4 0x1c8，进程已结束，退出代码 0](/images/blog/20251228191510156.png)

2、修改图片中的宽高参数，然后保存图片查看。

![010 Editor 的 IHDR 里，红框标出 00 00 01 F4 与 00 00 01 C8，即宽 500、高 456](/images/blog/20251228191511514.png)

查看图片，得到flag。

![手写体图片「Where is the Flag???」，底部写着 MRCTF{1ts_vEryyyyyy_ez!}](/images/blog/20251228191513318.png)

### flag：

```bash
flag{1ts_vEryyyyyy_ez!}
```