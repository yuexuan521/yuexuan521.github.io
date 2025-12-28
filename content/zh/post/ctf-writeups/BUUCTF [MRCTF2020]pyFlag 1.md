---
title: "BUUCTF [MRCTF2020]pyFlag 1"
date: 2025-06-30 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "BUUCTF"
- "CTF"
- "misc"
- "MRCTF2020"
- "pyFlag"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191535093.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[CTF常见编码及加解密（超全）](https://www.cnblogs.com/ruoli-s/p/14206145.html#%E7%A4%BE%E4%BC%9A%E4%B8%BB%E4%B9%89%E7%BC%96%E7%A0%81) 
[MRCTF2020 pyFlag](https://www.cnblogs.com/WXjzc/p/16096233.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191537295.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢天璇战队供题。

### 密文：

下载附件，解压得到三张图片：Furan.jpg、Miku.jpg、Setsuna.jpg。（出题人真是个二次元(╯°□°）╯︵ ┻━┻）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191538874.jpeg)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191540913.jpeg)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191542929.jpeg)

---

### 解题思路：

1、先看第一张图片Furan.jpg，用010Editor找到文件末尾的ZIP压缩包数据，根据hint这只是第二部分的数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191544741.png)

以此类推，在另外两张图片找到剩下的两部分数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191547025.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191549099.png)

拼接在一起保存为ZIP文件，解压需要密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191550968.png)

2、回去找密码，找了一圈什么都没有。用Ziperello尝试暴力破解，得到密码 `1234` 。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191552293.png)

解压压缩包，得到flag.txt和.hint.txt，内容分别如下：

一个是密文：

```python
G&eOhGcq(ZG(t2*H8M3dG&wXiGcq(ZG&wXyG(j~tG&eOdGcq+aG(t5oG(j~qG&eIeGcq+aG)6Q<G(j~rG&eOdH9<5qG&eLvG(j~sG&nRdH9<8rG%++qG%__eG&eIeGc+|cG(t5oG(j~sG&eOlH9<8rH8C_qH9<8oG&eOhGc+_bG&eLvH9<8sG&eLgGcz?cG&3|sH8M3cG&eOtG%_?aG(t5oG(j~tG&wXxGcq+aH8V6sH9<8rG&eOhH9<5qG(<E-H8M3eG&wXiGcq(ZG)6Q<G(j~tG&eOtG%+<aG&wagG%__cG&eIeGcq+aG&M9uH8V6cG&eOlH9<8rG(<HrG(j~qG&eLcH9<8sG&wUwGek2)
```

一个是提示：

```python
我用各种baseXX编码把flag套娃加密了，你应该也有看出来。
但我只用了一些常用的base编码哦，毕竟我的智力水平你也知道...像什么base36base58听都没听过
提示：0x10,0x20,0x30,0x55
```

3、根据hint只使用了Base编码，（提示： `0x10,0x20,0x30,0x55` 转为十进制 `16 32 48 85` ，似乎是加密顺序，Base16->Base32->Base48->Base85，逆序解密即可。不过好像没有Base48编码啊！！！这是怎么回事？(╯°□°）╯︵ ┻━┻）

> Base16特征：大写字母(A-Z)和数字(0-9)，不用‘=’补齐。
> Base32特征：大写字母(A-Z)和数字(2-7)，不满5的倍数，用‘=’补齐。
> Base64特征：大小写字母（A-Z，a-z）和数字（0-9）以及特殊字符‘+’，‘/’，不满3的倍数，用‘=’补齐。
> Base85特征：特点是奇怪的字符比较多，但是很难出现等号

先解起来再说吧。首先，进行Base85解密，得到：

```python
475532444B4E525549453244494E4A57475132544B514A54473432544F4E4A5547515A44474D4A5648415A54414E4257473434544B514A5647595A54514D5A5147553444474D5A5547453355434E5254475A42444B514A57494D3254534D5A5447555A444D4E5256494532444F4E4A57475A41544952425547343254454E534447595A544D524A5447415A55493D3D3D
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191554548.png)

接下来进行Base16解密，得到

[Base16编码解码](https://www.qqxiuzi.cn/bianma/base.php?type=16) 

```python
GU2DKNRUIE2DINJWGQ2TKQJTG42TONJUGQZDGMJVHAZTANBWG44TKQJVGYZTQMZQGU4DGMZUGE3UCNRTGZBDKQJWIM2TSMZTGUZDMNRVIE2DONJWGZATIRBUG42TENSDGYZTMRJTGAZUI===
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191556564.png)

典型的Base32密文特征，进行Base32解密，得到

[Base32编码解码](https://www.qqxiuzi.cn/bianma/base.php) 

```python
54564A4456455A3757544231583046795A5638305833417A636B5A6C593352665A47566A4D47526C636E303D  
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191558565.png)

又是Base16，解密得到

```python
TVJDVEZ7WTB1X0FyZV80X3AzckZlY3RfZGVjMGRlcn0=
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191600106.png)

最后是Base64解密，得到flag： `MRCTF{Y0u_Are_4_p3rFect_dec0der}` 。

[BASE64加密解密](https://base64.supfree.net/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191601669.png)

官方WriteUP脚本:

```python
#!/usr/bin/env python

import base64
import re

def baseDec(text,type):
    if type == 1:
        return base64.b16decode(text)
    elif type == 2:
        return base64.b32decode(text)
    elif type == 3:
        return base64.b64decode(text)
    elif type == 4:
        return base64.b85decode(text)
    else:
        pass

def detect(text):
    try:
        if re.match("^[0-9A-F=]+$",text.decode()) is not None:
            return 1
    except:
        pass
    
    try:
        if re.match("^[A-Z2-7=]+$",text.decode()) is not None:
            return 2
    except:
        pass

    try:
        if re.match("^[A-Za-z0-9+/=]+$",text.decode()) is not None:
            return 3
    except:
        pass
    
    return 4

def autoDec(text):
    while True:
        if b"MRCTF{" in text:
            print("\n"+text.decode())
            break

        code = detect(text)
        text = baseDec(text,code)

with open("flag.txt",'rb') as f:
    flag = f.read()

autoDec(flag)
```

### flag：

```bash
flag{Y0u_Are_4_p3rFect_dec0der}
```