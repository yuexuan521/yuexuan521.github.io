---
title: "BUUCTF [ACTF新生赛2020]剑龙 1"
date: 2025-01-13 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "数据库"
- "安全"
- "网络安全"
- "CTF"
- "BUUCTF"
- "misc"
- "stegosaurus"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190557074.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[Hello CTF](https://hello-ctf.com/HC_Start/) 
[NewStar CTF](https://ns.openctf.net/learn/) 
[BuuCTF难题详解| Misc | [ACTF新生赛2020]剑龙](https://blog.csdn.net/pone2233/article/details/108601733) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190559563.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190601129.png)

---

### 解题思路：

1、先看一下hint.zip压缩包，解压得到

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190602214.png)

pwd.txt内容如下：

> ﾟωﾟﾉ= /｀ｍ´）ﾉ ~┻━┻ // *´∇｀* / [‘ *']; o=(ﾟｰﾟ) =* =3; c=(ﾟΘﾟ) =(ﾟｰﾟ)-(ﾟｰﾟ); (ﾟДﾟ) =(ﾟΘﾟ)= (o<sup>_</sup>o)/ (o<sup>_</sup>o);(ﾟДﾟ)={ﾟΘﾟ: ‘ *’ ,ﾟωﾟﾉ : ((ﾟωﾟﾉ==3) +'* ’) [ﾟΘﾟ] ,ﾟｰﾟﾉ :(ﾟωﾟﾉ+ ‘ *‘)[o<sup>_</sup>o -(ﾟΘﾟ)] ,ﾟДﾟﾉ:((ﾟｰﾟ==3) +’* ’)[ﾟｰﾟ] }; (ﾟДﾟ) [ﾟΘﾟ] =((ﾟωﾟﾉ <mark>3) +‘ *‘) [c<sup>_</sup>o];(ﾟДﾟ) [‘c’] = ((ﾟДﾟ)+’* ’) [ (ﾟｰﾟ)+(ﾟｰﾟ)-(ﾟΘﾟ) ];(ﾟДﾟ) [‘o’] = ((ﾟДﾟ)+‘ *‘) [ﾟΘﾟ];(ﾟoﾟ)=(ﾟДﾟ) [‘c’]+(ﾟДﾟ) [‘o’]+(ﾟωﾟﾉ +’* ’)[ﾟΘﾟ]+ ((ﾟωﾟﾉ</mark> 3) +’ *‘) [ﾟｰﾟ] + ((ﾟДﾟ) +’* ‘) [(ﾟｰﾟ)+(ﾟｰﾟ)]+ ((ﾟｰﾟ <mark>3) +‘_’) [ﾟΘﾟ]+((ﾟｰﾟ</mark> 3) +’ *‘) [(ﾟｰﾟ) - (ﾟΘﾟ)]+(ﾟДﾟ) [‘c’]+((ﾟДﾟ)+’* ‘) [(ﾟｰﾟ)+(ﾟｰﾟ)]+ (ﾟДﾟ) [‘o’]+((ﾟｰﾟ <mark>3) +‘ *‘) [ﾟΘﾟ];(ﾟДﾟ) [’* ’] =(o<sup>_</sup>o) [ﾟoﾟ] [ﾟoﾟ];(ﾟεﾟ)=((ﾟｰﾟ</mark> 3) +’ *‘) [ﾟΘﾟ]+ (ﾟДﾟ) .ﾟДﾟﾉ+((ﾟДﾟ)+’* ‘) [(ﾟｰﾟ) + (ﾟｰﾟ)]+((ﾟｰﾟ <mark>3) +‘_’) [o<sup>_</sup>o -ﾟΘﾟ]+((ﾟｰﾟ</mark> 3) +’ *‘) [ﾟΘﾟ]+ (ﾟωﾟﾉ +’* ‘) [ﾟΘﾟ]; (ﾟｰﾟ)+=(ﾟΘﾟ); (ﾟДﾟ)[ﾟεﾟ]=’\‘; (ﾟДﾟ).ﾟΘﾟﾉ=(ﾟДﾟ+ ﾟｰﾟ)[o<sup>_</sup>o -(ﾟΘﾟ)];(oﾟｰﾟo)=(ﾟωﾟﾉ +’ *‘)[c<sup>_</sup>o];(ﾟДﾟ) [ﾟoﾟ]=’"‘;(ﾟДﾟ) [’* ‘] ( (ﾟДﾟ) [’ *‘] (ﾟεﾟ+(ﾟДﾟ)[ﾟoﾟ]+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o<sup>_</sup>o) +(o<sup>_</sup>o))+ ((ﾟｰﾟ) + (o<sup>_</sup>o))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (o<sup>_</sup>o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ ((ﾟｰﾟ) + (o<sup>_</sup>o))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+((o<sup>_</sup>o) +(o<sup>_</sup>o))+ (o<sup>_</sup>o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟoﾟ]) (ﾟΘﾟ)) (’* ');

确认为aaEncode编码，使用在线工具解得 `welcom3!` 

[在线工具：https://toolwa.com/aaencode/](https://toolwa.com/aaencode/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190603819.png)

2、剩下一张图片，再加上一个密码，确认使用steghide工具加密。

steghide下载地址： [https://sourceforge.net/projects/steghide/](https://sourceforge.net/projects/steghide/) 

使用如下命令，得到隐藏信息。

```shell
steghide extract -sf hh.jpg -p welcom3!
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190605825.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190606909.png)

在hh.jpg的属性找到密钥： `@#$%^&%%$)` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190608265.png)

解得如下信息：

```python
think about stegosaurus
```

DES加解密： [https://www.sojson.com/encrypt_des.html](https://www.sojson.com/encrypt_des.html) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190609623.png)

3、搜索发现对应题目“剑龙”，但其实指的是stegosaurus pyc隐写工具。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190611323.png)

O_O文件的确是一个pyc文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190613551.png)

stegosaurus下载地址： [https://github.com/AngelKitty/stegosaurus](https://github.com/AngelKitty/stegosaurus) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190614895.png)

运行脚本加上 `-x` 参数，得到flag： `flag{3teg0Sauru3_!1}` 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190617000.png)

### flag：

```bash
 flag{3teg0Sauru3_!1}
```