---
title: "BUUCTF [XMAN2018排位赛]通行证 1"
date: 2024-09-21 20:10:52
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "密码学"
- "栅栏密码"
- "CTF"
- "BUUCTF"
- "MISC"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192311959.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192314003.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源：https://github.com/hebtuerror404/CTF_competition_warehouse_2018

### 密文：

```bash
a2FuYmJyZ2doamx7emJfX19ffXZ0bGFsbg==
```

---

### 解题思路：

1、两个等号结尾，“=”符号作后缀填充，base64的典型特征。解码得到 `kanbbrgghjl{zb____}vtlaln` 
[BASE64加密解密](https://base64.supfree.net/) 

```bash
kanbbrgghjl{zb____}vtlaln
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192316155.png)

2、发现一对花括号，一般用来包括flag值，在这里位置不同，应尝试转换位置，使用栅栏密码。（没想到，这里使用栅栏加密，而不是解密，并且是W型栅栏加密）
[栅栏加密/解密](https://ctf.bugku.com/tool/railfence) 

> 于篱笆密码法中， [明文](https://zh.wikipedia.org/wiki/%E6%98%8E%E6%96%87) 由上至下顺序写上，当到达最低部时，再回头向上，一直重复直至整篇明文写完为止。然后，再往右顺序抄写一次。
> 
> 此例子中，其包含了三条篱笆及一段明文：‘WE ARE DISCOVERED. FLEE AT ONCE’。然后再按法抄下：
> 
> ```
> W . . . E . . . C . . . R . . . L . . . T . . . E
> . E . R . D . S . O . E . E . F . E . A . O . C .
> . . A . . . I . . . V . . . D . . . E . . . N . .
> ```
> 
> 读取后再按 [传统](https://zh.wikipedia.org/wiki/%E6%9B%BF%E6%8D%A2%E5%BC%8F%E5%AF%86%E7%A0%81#%E7%B0%A1%E6%98%93%E6%9B%BF%E6%8F%9B%E5%AF%86%E7%A2%BC) 分组：
> 
> ```
> WECRL TEERD SOEEF EAOCA IVDEN
> ```

```bash
kzna{blnl_abj_lbh_trg_vg}
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192318503.png)

当栏数为7时，符合flag形式“xxxx{xxxxxxxxx}”

3、最后使用凯撒密码爆破脚本，移位数为13时，得到flag

```python
13  xman{oyay_now_you_get_it}
```

```python
# 编   者：玥轩
# 开发时间：2023/5/17 23:06

def decrypt(ciphertext, shift):
    """移位解密函数"""
    plaintext = ''
    for char in ciphertext:
        if char.isalpha(): # 如果是字母，进行移位解密
            if char.isupper():
                plaintext += chr((ord(char) - shift - 65) % 26 + 65) # 大写字母移位解密
            else:
                plaintext += chr((ord(char) - shift - 97) % 26 + 97) # 小写字母移位解密
        else: # 如果不是字母，直接输出
            plaintext += char
    return plaintext

# 加密密文和移位数
ciphertext = 'kzna{blnl_abj_lbh_trg_vg}'
shift = 3
ciphertext = ciphertext.lower()
# 小写易于观察
# 枚举所有可能的移位数，输出所有解密结果
for i in range(26):
    plaintext = decrypt(ciphertext, i)
    print("%d %s"% (i, plaintext))
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192320176.png)

4、另一种解题思路，通过对base64解出的密文进行凯撒加密，寻找“x”和“f”开头的字符串（因为是flag的形式），再进行栅栏加密。原理都是相同的，且不要求解密顺序。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192321751.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192323424.png)

### flag：

```bash
flag{oyay_now_you_get_it}
```