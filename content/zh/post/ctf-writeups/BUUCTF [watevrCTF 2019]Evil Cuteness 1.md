---
title: "BUUCTF [watevrCTF 2019]Evil Cuteness 1"
date: 2025-06-16 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192214992.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［watevrCTF 2019］Evil Cuteness 1」，附件 attachment.jpg](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192217527.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件，得到attachment.jpg

 ![一张虎斑小猫的照片](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192219080.jpeg)

---

### 解题思路：

1、习惯先在010Editor看一下，然后就发现“ `PK` ”头，隐藏ZIP压缩包。

 ![十六进制视图，高亮 FF D9 之后紧跟 50 4B 03 04](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192220599.png)

另存为ZIP压缩包，解压得到abc文件

 ![分离出一个名为 abc 的文件，大小 1 KB](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192222407.png)

打开它，得到flag。

```python
watevr{7h475_4c7u4lly_r34lly_cu73_7h0u6h}
```

 ![记事本打开 abc：watevr{7h475_4c7u4lly_r34lly_cu73_7h0u6h}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192223473.png)

 ![010 Editor 里同样是这行 flag，整行被选中](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192224665.png)

### flag：

```bash
flag{7h475_4c7u4lly_r34lly_cu73_7h0u6h}
```