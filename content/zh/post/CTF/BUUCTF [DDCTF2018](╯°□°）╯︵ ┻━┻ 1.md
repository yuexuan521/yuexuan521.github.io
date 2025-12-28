---
title: "BUUCTF [DDCTF2018](╯°□°）╯︵ ┻━┻ 1"
date: 2025-08-11 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "misc"
- "DDCTF2018"
- "网络安全"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190907404.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：[DDCTF2018](╯°□°）╯︵ ┻━┻](https://blog.csdn.net/mochu7777777/article/details/105324802#:~:text=DDCTF2) 
[[DDCTF2018](╯°□°）╯︵ ┻━┻](https://www.cnblogs.com/fishjumpriver/p/17833337.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190909389.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件为attachment.txt

```bash
(╯°□°）╯︵ ┻━┻
50pt

(╯°□°）╯︵ ┻━┻

d4e8e1f4a0f7e1f3a0e6e1f3f4a1a0d4e8e5a0e6ece1e7a0e9f3baa0c4c4c3d4c6fbb9b2b2e1e2b9b9b7b4e1b4b7e3e4b3b2b2e3e6b4b3e2b5b0b6b1b0e6e1e5e1b5fd
```

---

### 解题思路：

1、观察密文，134个字符，猜测为十六进制的数据，先两位截取。

```bash
hex列表：['d4', 'e8', 'e1', 'f4', 'a0', 'f7', 'e1', 'f3', 'a0', 'e6', 'e1', 'f3', 'f4', 'a1', 'a0', 'd4', 'e8', 'e5', 'a0', 'e6', 'ec', 'e1', 'e7', 'a0', 'e9', 'f3', 'ba', 'a0', 'c4', 'c4', 'c3', 'd4', 'c6', 'fb', 'b9', 'b2', 'b2', 'e1', 'e2', 'b9', 'b9', 'b7', 'b4', 'e1', 'b4', 'b7', 'e3', 'e4', 'b3', 'b2', 'b2', 'e3', 'e6', 'b4', 'b3', 'e2', 'b5', 'b0', 'b6', 'b1', 'b0', 'e6', 'e1', 'e5', 'e1', 'b5', 'fd']
```

将十六进制数据转换为十进制。

```bash
十进制列表：[212, 232, 225, 244, 160, 247, 225, 243, 160, 230, 225, 243, 244, 161, 160, 212, 232, 229, 160, 230, 236, 225, 231, 160, 233, 243, 186, 160, 196, 196, 195, 212, 198, 251, 185, 178, 178, 225, 226, 185, 185, 183, 180, 225, 180, 183, 227, 228, 179, 178, 178, 227, 230, 180, 179, 226, 181, 176, 182, 177, 176, 230, 225, 229, 225, 181, 253]
```

发现十进制数据都大于128，减去128，再转为字符串。

```bash
-128得到ASCII十进制dec列表：[84, 104, 97, 116, 32, 119, 97, 115, 32, 102, 97, 115, 116, 33, 32, 84, 104, 101, 32, 102, 108, 97, 103, 32, 105, 115, 58, 32, 68, 68, 67, 84, 70, 123, 57, 50, 50, 97, 98, 57, 57, 55, 52, 97, 52, 55, 99, 100, 51, 50, 50, 99, 102, 52, 51, 98, 53, 48, 54, 49, 48, 102, 97, 101, 97, 53, 125]
```

转为字符串，得到flag。

```bash
最终答案：That was fast! The flag is: DDCTF{922ab9974a47cd322cf43b50610faea5}
```

解密脚本如下：

```python
# @Author：YueXuan
# @Date  ：2024/10/8 22:00

def split_into_hex_pairs(s):
    """将输入字符串切片成每两个字符一组的列表"""
    return [s[i:i+2] for i in range(0, len(s), 2)]

def convert_hex_to_int(hex_pairs):
    """将十六进制列表转换为十进制整数列表"""
    return [int(pair, 16) for pair in hex_pairs]

def adjust_for_ascii(int_values):
    """将整数列表中的值减去128以获取ASCII值"""
    return [value - 128 for value in int_values]

def convert_int_to_str(int_values):
    """将整数列表转换为字符串"""
    return ''.join(chr(value) for value in int_values)

def main(hex_string):
    """主函数，调用上述函数并打印结果"""
    print("字符串长度：%s" % len(hex_string))

    hex_pairs = split_into_hex_pairs(hex_string)
    print("hex列表：%s" % hex_pairs)

    int_values = convert_hex_to_int(hex_pairs)
    print("转化为十进制int列表：%s" % int_values)

    ascii_values = adjust_for_ascii(int_values)
    print("-128得到ASCII十进制dec列表：%s" % ascii_values)

    result_str = convert_int_to_str(ascii_values)
    print('最终答案：%s' % result_str)

if __name__ == '__main__':
    hex_str = 'd4e8e1f4a0f7e1f3a0e6e1f3f4a1a0d4e8e5a0e6ece1e7a0e9f3baa0c4c4c3d4c6fbb9b2b2e1e2b9b9b7b4e1b4b7e3e4b3b2b2e3e6b4b3e2b5b0b6b1b0e6e1e5e1b5fd'
    main(hex_str)
```

---

网上的其他脚本：

```python
# -*- coding:utf-8 -*-
# author: mochu7
def hex_str(str):#对字符串进行切片操作，每两位截取
    hex_str_list=[]
    for i in range(0,len(str)-1,2):
        hex_str=str[i:i+2]
        hex_str_list.append(hex_str)
    print("hex列表：%s\n"%hex_str_list)
    hex_to_str(hex_str_list)

def hex_to_str(hex_str_list):
    int_list=[]
    dec_list=[]
    flag=''
    for i in range(0,len(hex_str_list)):#把16进制转化为10进制
        int_str=int('0x%s'%hex_str_list[i],16)
        int_list.append(int_str)
        dec_list.append(int_str-128)#-128得到正确的ascii码
    for i in range(0,len(dec_list)):#ascii码转化为字符串
        flag += chr(dec_list[i])
    print("转化为十进制int列表：%s\n"%int_list)
    print("-128得到ASCII十进制dec列表：%s\n"%dec_list)
    print('最终答案：%s'%flag)

if __name__=='__main__':
    str='d4e8e1f4a0f7e1f3a0e6e1f3f4a1a0d4e8e5a0e6ece1e7a0e9f3baa0c4c4c3d4c6fbb9b2b2e1e2b9b9b7b4e1b4b7e3e4b3b2b2e3e6b4b3e2b5b0b6b1b0e6e1e5e1b5fd'
    print("字符串长度：%s"%len(str))
    hex_str(str)
```

```python
key='d4e8e1f4a0f7e1f3a0e6e1f3f4a1a0d4e8e5a0e6ece1e7a0e9f3baa0c4c4c3d4c6fbb9b2b2e1e2b9b9b7b4e1b4b7e3e4b3b2b2e3e6b4b3e2b5b0b6b1b0e6e1e5e1b5fd'
hex=[]
flag=''
for i in range(0,len(key)-1,2):
    zdh=key[i:i+2]
    hex.append(zdh)
for i in range(len(hex)):
    zdh1=hex[i]
    flag+=chr(int(int(zdh1,16))-128)
print(flag)
```

### flag：

```bash
flag{922ab9974a47cd322cf43b50610faea5}
```