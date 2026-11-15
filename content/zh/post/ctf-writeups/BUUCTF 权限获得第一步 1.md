---
title: "BUUCTF 权限获得第一步 1"
date: 2024-09-25 21:43:30
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "密码学"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205053233.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![BUUCTF「权限获得第一步 1」题目页，附件 8789fff6-91…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205054876.png)

### 题目描述：

你猜这是什么东西，记得破解后把其中的密码给我。答案为非常规形式。 注意：得到的 flag 请包上 flag{} 提交

### 密文：

```
Administrator:500:806EDC27AA52E314AAD3B435B51404EE:F4AD50F57683D4260DFD48AA351A17A8:::
```

---

### 解题思路：

1、有没有很熟悉的感觉，观察密文，尝试md5，得到结果。

![NTLM 反查：F4AD50F57683D4260DFD48AA351A17A8 对应 3617656](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205056443.png)

---

### flag：

```
flag{3617656}
```