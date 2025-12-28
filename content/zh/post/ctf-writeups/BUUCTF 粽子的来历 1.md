---
title: "BUUCTF 粽子的来历 1"
date: 2025-05-30 11:22:04
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "BUUCTF"
- "MISC"
- "网络安全"
- "安全"
- "DBAPP"
- "MD5"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193243698.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[CTF比赛中，你见过哪些奇怪的或者脑洞大开的misc?](https://www.zhihu.com/question/345032936) 
[ctf 粽子的来历](https://blog.csdn.net/weixin_43790779/article/details/104160841) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193246202.png)

### 题目描述：

曹操的私生子曹小明因为爸爸活着的时候得罪太多人，怕死后被抄家，所以把财富保存在一个谁也不知道的地方。曹小明比较喜欢屈原，于是把地点藏在他的诗中。三千年后，小明破译了这个密码，然而却因为担心世界因此掀起战争又亲手封印了这个财富并仿造当年曹小明设下四个可疑文件，找到小明喜欢的DBAPP标记，重现战国辉煌。(答案为正确值(不包括数字之间的空格)的小写32位md5值) 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到四个DOC文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193248131.png)

---

### 解题思路：

1、在010 Editor中打开文件，每个文件的相近位置都出现类似信息，将这些数据修改成“ `FF` ”，文件就可以正常浏览。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193249498.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193251686.png)

打开文件，里面是一首离骚，四个文件内容相同。但是通过对比四个文件，发现有些段落的间距不同。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193253068.png)

2、文本的段间距有两种格式：1.5倍行距和单倍行距。1.5倍行距代表“ `1` ”，单倍行距代表“ `0` ”，得到字符串： `100100100001` 。（我是C.doc文件的数据是正确的）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193255012.png)

将这串字符串进行MD5加密，得到flag： `d473ee3def34bd022f8e5233036b3345` 。(答案为正确值(不包括数字之间的空格)的小写32位md5值)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193256611.png)

附一个脚本：

```python
import hashlib

def md5_encrypt(data):
    md5 = hashlib.md5()
    md5.update(data.encode('utf-8'))
    return md5.hexdigest()

# 测试示例
data = "100100100001"
encrypted_data = md5_encrypt(data)
print("加密前的数据：", data)
print("加密后的数据：", encrypted_data)
```

### flag：

```bash
flag{d473ee3def34bd022f8e5233036b3345}
```