---
title: "BUUCTF 权限获得第一步 1"
date: 2024-09-25 21:43:30
category: "BUUCTF Crypto"
tags:
- "网络安全"
- "CTF"
- "Crypto"
- "密码"
- "md5"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205053233.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205054876.png)

### 题目描述：

你猜这是什么东西，记得破解后把其中的密码给我。答案为非常规形式。 注意：得到的 flag 请包上 flag{} 提交

### 密文：

```
Administrator:500:806EDC27AA52E314AAD3B435B51404EE:F4AD50F57683D4260DFD48AA351A17A8:::
```

---

### 解题思路：

1、有没有很熟悉的感觉，观察密文，尝试md5，得到结果。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205056443.png)

---

### flag：

```
flag{3617656}
```