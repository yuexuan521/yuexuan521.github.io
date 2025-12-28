---
title: "BUUCTF 大帝的密码武器 1"
date: 2024-09-27 10:44:17
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "buuctf"
- "网络安全"
- "密码"
- "ctf"
- "Crypto"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205036313.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205038483.png)

### 题目描述：

（下载题目，然后修改后缀名为.zip打开：）

公元前一百年，在罗马出生了一位对世界影响巨大的人物，他生前是罗马三巨头之一。他率先使用了一种简单的加密函，因此这种加密方法以他的名字命名。
以下密文被解开后可以获得一个有意义的单词：FRPHEVGL
你可以用这个相同的加密向量加密附件中的密文，作为答案进行提交。

### 密文：

```
ComeChina
```

---

### 解题步骤：

1、对题目中给出的密文进行凯撒解密（可以使用在线网站），执行以下Python代码
在线网站： [凯撒密码加密解密](https://www.qqxiuzi.cn/bianma/kaisamima.php) 

```python
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
ciphertext = 'FRPHEVGL'
shift = 3
ciphertext = ciphertext.lower()
# 枚举所有可能的移位数，输出所有解密结果
for i in range(26):
    plaintext = decrypt(ciphertext, i)
    print("%d %s"% (i, plaintext))
```

2、得到26个结果。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205040570.png)

3、寻找有意义的单词，发现偏移量为13的结果为有意义的单词。可以通过翻译软件，快速找到有意义的单词。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205042425.png)

4、对密文进行偏移量为13的凯撒解密，得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205044350.png)

### flag：

```
PbzrPuvan
```

**结束** 