---
title: "BUUCTF [UTCTF2020]zero 1"
date: 2025-04-21 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "Misc"
- "网络安全"
- "安全"
- "零宽隐写"
- "zero"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192204670.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：[UTCTF2020]zero](https://blog.csdn.net/mochu7777777/article/details/109817723https://blog.csdn.net/mochu7777777/article/details/109817723) 
[零宽度字符隐写](https://lazzzaro.github.io/2020/05/24/misc-%E9%9B%B6%E5%AE%BD%E5%BA%A6%E5%AD%97%E7%AC%A6%E9%9A%90%E5%86%99/index.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192206718.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存附件，内容如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192208079.png)

---

### 解题思路：

1、密文如下，本来想尝试凯撒加密，但在PyCharm中看到存在大量“ZWJ”字符，联想到零宽隐写。

> `&zwj;` 
> 它叫 **零宽** 连字，全称是Zero Width Joiner，简称“ **ZWJ** ”，是一个不打印字符，放在某些需要复杂排版语言（如阿拉伯语、印地语）的两个字符之间，使得这两个本不会发生连字的字符产生了连字效果。零宽连字符的Unicode码位是U+200D (HTML: ‍ ‍）。

```python
​​​​​​​Lorem ipsum​​​​​​​ dolor ‌‌‌‌‍﻿‍‍sit​​​​​​​​ amet​​​​​​​​​‌‌‌‌‍﻿‍‌, consectetur ​​​​​​​adipiscing​​​​​​​‌‌‌‌‍‬‍‬ elit​​​​​​​.‌‌‌‌‍‬﻿‌​​​​​​​‌‌‌‌‍‬‌‍ Phasellus quis​​​​​​​ tempus​​​​​​ ante, ​​​​​​​​nec vehicula​​​​​​​​​​​​​​​​ mi​​​​​​​​. ​​​​​​​‌‌‌‌‍‬‍﻿Aliquam nec​​​​​​​​​‌‌‌‌‍﻿‬﻿ nisi ut neque​​​​​​​ interdum auctor​​​​​​​.‌‌‌‌‍﻿‍﻿ Aliquam felis ‌‌‌‌‍‬‬‌orci​​​​​​​, vestibulum ‌‌‌‌‍﻿‬‍sit ​​​​​​​amet​​​​​​​​​ ante‌‌‌‌‍‌﻿‬ at​​​​​​​, consectetur‌‌‌‌‍‌﻿﻿ lobortis eros​​​​​​​​​.‌‌‌‌‍‍‍‌ ‌‌‌‌‍‌‌‌​​​​​​​Orci varius​​​​​​​ ​​​​​​​natoque ‌‌‌‌‍﻿‌﻿penatibus et ‌‌‌‌‍‬‌﻿​​​​​​​magnis‌‌‌‌‌﻿‌‍‌‌‌‌‌﻿‌‍ dis ​​​​​​​‌‌‌‌‍‍﻿﻿parturient montes, ​​​​​​​nascetur ridiculus ‌‌‌‌‌﻿‍‌​​​​​​​​​​​​​​‌‌‌‌‌﻿‬‍mus. In finibus‌‌‌‌‌﻿‌‬ magna​​​​​​‌‌‌‌‌﻿‍﻿ mauris, quis‌‌‌‌‍‬‌‍ auctor ‌‌‌‌‍‬‌‍libero congue quis. ‌‌‌‌‍‬‬‬Duis‌‌‌‌‍‬‌‬ sagittis consequat urna non tristique. Pellentesque eu lorem ‌‌‌‌‍﻿‌‍id‌‌‌‌‍‬‬﻿ quam vestibulum ultricies vel ac purus‌‌‌‌‌﻿‌‍.‌‌‌‌‌﻿‍‌‌‌‌‌‍﻿﻿‍
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192209547.png)

（在Linux的vim编辑器中也可以看到）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192210963.png)

2、使用在线解密网站，得到flag： `utflag{whyNOT@sc11_4927aajbqk14}` 。

零宽度字符隐写： [https://330k.github.io/misc_tools/unicode_steganography.html](https://330k.github.io/misc_tools/unicode_steganography.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192213365.png)

### flag：

```bash
flag{whyNOT@sc11_4927aajbqk14}
```

> **零宽度字符隐写术** （Zero-Width Space Steganography）：
> 将隐藏消息编码和解码为不可打印/可读字符。
> **字符包括：** 
> 零宽度空格（\u200b）
> 零宽度非连接符（\u200c）
> 零宽度连接符（\u200d）
> 从左至右书写标记（\u200e）
> 从右至左书写标记（\u200f）

> **解密** 
> **在线工具** [https://www.mzy0.com/ctftools/zerowidth1/](https://www.mzy0.com/ctftools/zerowidth1/) [http://330k.github.io/misc_tools/unicode_steganography.html](http://330k.github.io/misc_tools/unicode_steganography.html) 
> [https://offdev.net/demos/zwsp-steg-js](https://offdev.net/demos/zwsp-steg-js) 
> [https://yuanfux.github.io/zero-width-web/](https://yuanfux.github.io/zero-width-web/) 
> [http://www.atoolbox.net/Tool.php?Id=829](http://www.atoolbox.net/Tool.php?Id=829) 
> **其他工具** zwsp-steg-py [https://github.com/enodari/zwsp-steg-py](https://github.com/enodari/zwsp-steg-py) 
> **转换** 转化为二进制的加密： [https://zhuanlan.zhihu.com/p/87919817](https://zhuanlan.zhihu.com/p/87919817) 
> 转化为Morse编码的加密： [https://zhuanlan.zhihu.com/p/75992161](https://zhuanlan.zhihu.com/p/75992161) 

---

**Tips** 

> 居然已经3年了，时间真是快啊。逝者如斯夫，不舍昼夜！

1. 您发布的文章将会展示至 [里程碑专区](https://blog.csdn.net/rank/list/milestone) ，您也可以在 [专区](https://blog.csdn.net/rank/list/milestone) 内查看其他创作者的纪念日文章

2. 优质的纪念文章将会获得神秘打赏哦

