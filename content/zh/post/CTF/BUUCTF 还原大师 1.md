---
title: "BUUCTF 还原大师 1"
date: 2024-09-25 20:41:34
category: "BUUCTF Crypto"
tags:
- "python"
- "开发语言"
- "CTF"
- "网络安全"
- "密码"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205125529.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205127717.png)

### 题目描述：

我们得到了一串神秘字符串：TASC?O3RJMV?WDJKX?ZM,问号部分是未知大写字母，为了确定这个神秘字符串，我们通过了其他途径获得了这个字串的32位MD5码。但是我们获得它的32位MD5码也是残缺不全，E903???4DAB???08???51?80??8A?,请猜出神秘字符串的原本模样，并且提交这个字串的32位MD5码作为答案。 注意：得到的 flag 请包上 flag{} 提交

---

### 解题思路：

1、仔细阅读题目，明白我们需要还原完整的MD5码，作为flag提交。

2、缺失的字符为大写字母，可以通过枚举来筛选出正确的MD5码。

设计程序：

```python
import hashlib

Cipertext = "TASC?O3RJMV?WDJKX?ZM"

for i in range(26):
    temp1 = Cipertext.replace("?", chr(65 + i), 1)
    for j in range(26):
        temp2 = temp1.replace("?", chr(65 + j), 1)
        for z in range(26):
            temp3 = temp2.replace("?", chr(65 + z), 1)

            Plaintext = hashlib.md5(temp3.encode("UTF-8")).hexdigest().upper()
            if Plaintext[0:4] == "E903":
                print(Plaintext)
```

3、执行代码，得到正确的MD5码作为flag提交。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205129653.png)

### flag：

```c
E9032994DABAC08080091151380478A2
```