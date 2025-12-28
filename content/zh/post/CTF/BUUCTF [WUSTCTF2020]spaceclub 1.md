---
title: "BUUCTF [WUSTCTF2020]spaceclub 1"
date: 2025-07-07 08:15:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "安全"
- "网络安全"
- "spaceclub"
- "WUSTCTF2020"
- "ASCII"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192245940.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：[WUSTCTF2020]spaceclub](https://blog.csdn.net/mochu7777777/article/details/107532402) 
[[WUSTCTF2020]spaceclub](https://www.cnblogs.com/fishjumpriver/p/18119169) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192248076.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢 Iven Huang 师傅供题。
比赛平台：https://ctfgame.w-ais.cn/

### 密文：

保存附件，得到attachment.txt

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192249826.png)

---

### 解题思路：

1、打开文件，什么都没有？欧~~，有猫腻。(╯°□°）╯︵ ┻━┻

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192251154.png)

放到SublimeText中，看得更清楚。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192252535.png)

2、接下来就简单了，长的一行对应“ `0` ”，短的一行对应“ `1` ”，得到二进制数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192254201.png)

合并数据，将二进制数据转换为ASCII字符串，得到flag： `wctf2020{h3re_1s_y0ur_fl@g_s1x_s1x_s1x}` 。

[在线多行合并一行工具](https://tool.hiofd.com/multi-to-one-line-online/) 

```python
011101110110001101110100011001100011001000110000001100100011000001111011011010000011001101110010011001010101111100110001011100110101111101111001001100000111010101110010010111110110011001101100010000000110011101011111011100110011000101111000010111110111001100110001011110000101111101110011001100010111100001111101
```

[2进制到ASCII字符串在线转换工具](https://coding.tools/cn/binary-to-text) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192255564.png)

贴一个网上找的脚本：

```python
import binascii

f = open("attachment.txt", "r")
content = f.readlines()
s = ""
for line in content:
    if len(line) == 7:
        s += "0"
    if len(line) == 13:
        s += "1"
print(s)
print(binascii.unhexlify(hex(int(s, 2))[2:]))
```

### flag：

```bash
flag{h3re_1s_y0ur_fl@g_s1x_s1x_s1x}
```