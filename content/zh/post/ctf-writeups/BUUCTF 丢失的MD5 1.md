---
title: "BUUCTF 丢失的MD5 1"
date: 2024-09-27 10:50:10
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "python"
- "buuctf"
- "网络安全"
- "密码"
- "ctf"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204941704.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204944216.png)

### 密文：

```python
import hashlib   
for i in range(32,127):
    for j in range(32,127):
        for k in range(32,127):
            m=hashlib.md5()
            m.update('TASC'+chr(i)+'O3RJMV'+chr(j)+'WDJKX'+chr(k)+'ZM')
            des=m.hexdigest()
            if 'e9032' in des and 'da' in des and '911513' in des:
                print des
```

### 解题思路：

密文部分给了一串Python代码，发现一个语法错误

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204945731.png)

修改后

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204947594.png)

运行代码，报错

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204949995.png)

报错原因是：必须在哈希之前Unicode对象进行编码
对代码进行修改，对字符进行（UTF-8）编码

```python
import hashlib   
for i in range(32,127):
    for j in range(32,127):
        for k in range(32,127):
            m=hashlib.md5()
            m.update('TASC'.encode('UTF-8')+chr(i).encode('UTF-8')+'O3RJMV'.encode('UTF-8')+chr(j).encode('UTF-8')+'WDJKX'.encode('UTF-8')+chr(k).encode('UTF-8')+'ZM'.encode('UTF-8'))
            des=m.hexdigest()
            if 'e9032' in des and 'da' in des and '911513' in des:
                print(des)
```

### flag：

```python
e9032994dabac08080091151380478a2
```