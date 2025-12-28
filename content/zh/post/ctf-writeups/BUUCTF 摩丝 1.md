---
title: "BUUCTF 摩丝 1"
date: 2024-09-27 10:56:02
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "CTF"
- "Crypto"
- "BUUCTF"
- "网络安全"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205046211.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205048318.png)

### 密文：

```python
.. .-.. --- ...- . -.-- --- ..-
```

###  摩尔斯电码简述：

 摩尔斯电码(Morse Code)是由美国人萨缪尔·摩尔斯在1836年发明的一种时通时断的且通过不同的排列顺序来表达不同英文字母、数字和标点符号的信号代码，摩尔斯电码主要由以下5种它的代码组成：

1、点（.）
2、划（-）
3、每个字符间短的停顿（通常用空格表示停顿）
4、每个词之间中等的停顿（通常用 / 划分）
5、以及句子之间长的停顿

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205049682.jpeg)

观察密文特征，以及提示，确定为莫尔斯编码。用在线工具直接解码，得到flag。

### flag：

```python
ILOVEYOU
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205051816.png)

**在线工具：** [在线摩斯密码翻译器](https://www.lddgo.net/encrypt/morse) 