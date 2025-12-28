---
title: "BUUCTF Alice与Bob 1"
date: 2024-09-27 10:46:07
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "python"
- "密码"
- "crypto"
- "ctf"
- "网络安全"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216120120899.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216120120900.png)

### 题目描述：

密码学历史中，有两位知名的杰出人物，Alice和Bob。他们的爱情经过置换和轮加密也难以混淆，即使是没有身份认证也可以知根知底。就像在数学王国中的素数一样，孤傲又热情。下面是一个大整数:98554799767,请分解为两个素数，分解后，小的放前面，大的放后面，合成一个新的数字，进行md5的32位小写哈希，提交答案。

---

### 解题过程：

1、将大整数98554799767，分解为两个素数。
请执行以下python代码：

```python
from random import randint
from math import gcd

def PollardRho(n, f):
    x, y = randint(1, n-1), 1
    while True:
        x = f(x, n)
        y = f(f(y, n), n)
        d = gcd(abs(x-y), n)
        if d != 1:
            return d
        elif x == y:
            return n

def factorize(n):
    if n == 1:
        return []
    elif is_prime(n):
        return [n]
    else:
        f = PollardRho(n, lambda x, n: (x**2 + 1) % n)
        return factorize(f) + factorize(n // f)

def is_prime(n):
    if n <= 1:
        return False
    elif n <= 3:
        return True
    elif n % 2 == 0 or n % 3 == 0:
        return False
    i = 5
    while i**2 <= n:
        if n % i == 0 or n % (i+2) == 0:
            return False
        i += 6
    return True

n = 98554799767
print(factorize(n))
```

得到结果：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216120120901.png)

2、小的放前面，大的放后面，合成一个新的数字。

```
[101999, 966233]
10999966233
```

3、对这个新数字，进行md5的32位小写哈希加密。
在线解密网站： [在线16位和32位大小写MD5加密](https://www.matools.com/md5) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251216120120902.png)

得到结果

### flag：

```
d450209323a847c8d01c6be47c81811a
```

**结束** 