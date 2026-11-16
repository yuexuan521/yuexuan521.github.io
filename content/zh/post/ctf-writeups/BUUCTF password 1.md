---
title: "BUUCTF password 1"
date: 2024-09-27 10:54:09
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "CTF"
- "密码学"
- "网络安全"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228204843053.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![BUUCTF「password 1」题目页，附件 005f93e8-64…](/images/blog/20251228204845519.png)

### 密文：

```python
姓名：张三 
生日：19900315

key格式为key{xxxxxxxxxx}
```

### 解题思路：

flag的长度为十位，猜测为姓名的缩写“zs”，加上生日“19900315”，构成flag。

### flag：

```python
flag{zs1990315}
```