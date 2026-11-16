---
title: "BUUCTF key不在这里 1"
date: 2025-05-30 11:22:38
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "MISC"
- "网络安全"
- "密码学"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228185953208.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [buuctf-misc-key不在这里1](https://blog.csdn.net/qq_29977871/article/details/126004017)


 ![题目页「key不在这里 1」，附件 attachment.zip](/images/blog/20251228185955261.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到1564386056.png

 ![一张黑白二维码，纹路非常密](/images/blog/20251228185956619.png)

---

### 解题思路：

1、扫码得到如下链接：

```python
https://cn.bing.com/search?q=key%E4%B8%8D%E5%9C%A8%E8%BF%99%E9%87%8C&m=10210897103375566531005253102975053545155505050521025256555254995410298561015151985150375568&qs=n&form=QBRE&sp=-1&sc=0-38&sk=&cvid=2CE15329C18147CBA4C1CA97C8E1BB8C
```

 ![QR Research 解码，内容是一个 cn.bing.com 搜索链接，版本 13、纠错 H](/images/blog/20251228185958216.png)

访问链接，没有找到flag。

 ![浏览器打开该链接，搜索词是「key不在这里」，结果定位到 CSDN 那篇文章](/images/blog/20251228190000841.png)

2、注意到URL中，有一长串十进制数字： `10210897103375566531005253102975053545155505050521025256555254995410298561015151985150375568` ，将其转为ASCII

```python
def decode_mixed(s):
    """
    解码混合长度的数字序列到ASCII字符。
    
    :param s: 密文字符串
    :return: 解码后的ASCII文本
    """
    temp = ''
    while s:
        if int(s[:3]) < 127:
            temp += chr(int(s[:3]))
            s = s[3:]
        else:
            temp += chr(int(s[:2]))
            s = s[2:]
    return temp

# 给定的密文
ciphertext = '10210897103375566531005253102975053545155505050521025256555254995410298561015151985150375568'

decoded_text = decode_mixed(ciphertext)
print(decoded_text)
```

得到ASCII字符串，如下

```python
flag%7B5d45fa256372224f48746c6fb8e33b32%7D
```

 ![终端输出 flag%7B5d45fa256372224f48746c6fb8e33b32%7D](/images/blog/20251228190002634.png)

3、再进行一次URL解码，得到flag。

[在线urlencode编码、urldecode解码、url编码解码、百分号编码](http://web.chacuo.net/charseturlencode) 
 ![URL 解码工具解出 flag{5d45fa256372224f48746c6fb8e33b32}](/images/blog/20251228190003935.png)

### flag：

```bash
flag{5d45fa256372224f48746c6fb8e33b32}
```