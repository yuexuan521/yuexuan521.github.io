---
title: "BUUCTF [watevrCTF 2019]Evil Cuteness 1"
date: 2025-06-16 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "BUUCTF"
- "CTF"
- "misc"
- "FLAG"
- "watevrCTF 2019"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192214992.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192217527.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件，得到attachment.jpg

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192219080.jpeg)

---

### 解题思路：

1、习惯先在010Editor看一下，然后就发现“ `PK` ”头，隐藏ZIP压缩包。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192220599.png)

另存为ZIP压缩包，解压得到abc文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192222407.png)

打开它，得到flag。

```python
watevr{7h475_4c7u4lly_r34lly_cu73_7h0u6h}
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192223473.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192224665.png)

### flag：

```bash
flag{7h475_4c7u4lly_r34lly_cu73_7h0u6h}
```