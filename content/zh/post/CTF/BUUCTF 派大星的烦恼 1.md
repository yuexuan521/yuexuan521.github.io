---
title: "BUUCTF 派大星的烦恼 1"
date: 2025-05-31 11:44:09
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "数据库"
- "缓存"
- "BUUCTF"
- "MISC"
- "CTF"
- "网络安全"
- "安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193122674.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：派大星的烦恼](https://blog.csdn.net/mochu7777777/article/details/109678243) 
[buuctf 派大星的烦恼](https://www.cnblogs.com/WXjzc/p/16095984.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193124793.png)

### 题目描述：

派大星最近很苦恼，因为它的屁股上出现了一道疤痕！我们拍下了它屁股一张16位位图，0x22，0x44代表伤疤两种细胞，0xf0则是派大星的赘肉。还原伤疤，知道是谁打的派大星！(答案为32位的一串字符串) 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到派大星的烦恼.bmp

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193126769.bmp)

---

### 解题思路：

1、用010Editor打开bmp文件，搜索 `0x22、0x44` 两种伤疤细胞。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193129039.png)

只有两种格式，可以尝试转换为二进制数据。

```python
"DD"DD""""D"DD""""""DD"""DD"DD""D""DDD""D"D"DD""""""DD""D""""DD"D"D"DD""""D"DD""D"""DD"""""DDD""""D"DD"""D"""DD"""D""DD"D"D"DD"""DD""DD"D"D""DD""DD"DD"""D"""DD""DD"DD""D"D""DD"D"D"DD"""D"""DD"""D"DD""DD"""DD"D"D""DD"""D"DD""DD""DD"""""DDD""DD""DD"""D""DD""
```

2、将 `"` 对应0、 `D` 对应1，转换为

```python
0110110000101100000011000110110010011100101011000000110010000110101011000010110010001100000111000010110001000110001001101010110001100110101001100110110001000110011011001010011010101100010001100010110011000110101001100010110011001100000111001100110001001100
```

将二进制数据转换为ASCII文字，转换出一堆乱码，尝试逆序反转二进制数据再转ASCII文字。
[ASCII，十六进制，二进制，十进制，Base64转换器](https://www.rapidtables.org/zh-CN/convert/number/ascii-hex-bin-dec-converter.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193130850.png)

逆序反转数据：
[文本逆序翻转工具
](https://uutool.cn/txt-reverse/) 

```python
0011001000110011001110000011001100110100011001010110001100110100011000100011010101100101001101100110001000110110011001010110011000110101011001000110001000110100001110000011000100110100001101010110000100110000001101010011100100110110001100000011010000110110
```

成功转换为ASCII： `23834ec4b5e6b6ef5db48145a0596046` 。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193132315.png)

3、用得到的ASCII文本提交flag(答案为32位的一串字符串) ，失败了。再将得到的ASCII文本进行逆序反转，得到flag： `6406950a54184bd5fe6b6e5b4ce43832` 。

最后，贴个网上的脚本：

```python
with open("E:\Download\misc\派大星的烦恼.bmp","rb") as fr:
    res = fr.read()[4000:4256]
    tmp = []
    for v in res:
        if v == 34:
            tmp.append(0)
        else:
            tmp.append(1)
    fr.close()
for i in range(len(tmp)):
    tmp[i] = str(tmp[i])
a = "".join(tmp)
print(a)
b = []
for i in range(0,len(a),8):
    t = a[i:i+8]
    t = t[::-1]
    b.append(int(t,2))
w = ""
for v in b:
    w+=str(hex(v))[2:]
print(w)
```

### flag：

```bash
flag{6406950a54184bd5fe6b6e5b4ce43832}
```