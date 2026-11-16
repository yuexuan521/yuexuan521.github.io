---
title: "BUUCTF [ACTF新生赛2020]剑龙 1"
date: 2025-01-13 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "数据库"
- "网络安全"
- "CTF"
- "BUUCTF"
- "MISC"
- "隐写"
---

![题图同样是那位蓝发女孩](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190557074.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [Hello CTF](https://hello-ctf.com/HC_Start/)
- [NewStar CTF](https://ns.openctf.net/learn/)
- [BuuCTF难题详解| Misc | ［ACTF新生赛2020］剑龙](https://blog.csdn.net/pone2233/article/details/108601733)


 ![题目页「［ACTF新生赛2020］剑龙 1」，附件 attachment.tar](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190559563.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到
 ![解出 hint.zip（64 KB）与一个叫 O_O 的文件（3 KB）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190601129.png)

---

### 解题思路：

1、先看一下hint.zip压缩包，解压得到

 ![hint.zip 里是 hh.jpg（柠檬水彩画）与 pwd.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190602214.png)

pwd.txt内容如下：

> ﾟωﾟﾉ= /｀ｍ´）ﾉ ~┻━┻ // *´∇｀* / [‘ *']; o=(ﾟｰﾟ) =* =3; c=(ﾟΘﾟ) =(ﾟｰﾟ)-(ﾟｰﾟ); (ﾟДﾟ) =(ﾟΘﾟ)= (o<sup>_</sup>o)/ (o<sup>_</sup>o);(ﾟДﾟ)={ﾟΘﾟ: ‘ *’ ,ﾟωﾟﾉ : ((ﾟωﾟﾉ==3) +'* ’) [ﾟΘﾟ] ,ﾟｰﾟﾉ :(ﾟωﾟﾉ+ ‘ *‘)[o<sup>_</sup>o -(ﾟΘﾟ)] ,ﾟДﾟﾉ:((ﾟｰﾟ==3) +’* ’)[ﾟｰﾟ] }; (ﾟДﾟ) [ﾟΘﾟ] =((ﾟωﾟﾉ <mark>3) +‘ *‘) [c<sup>_</sup>o];(ﾟДﾟ) [‘c’] = ((ﾟДﾟ)+’* ’) [ (ﾟｰﾟ)+(ﾟｰﾟ)-(ﾟΘﾟ) ];(ﾟДﾟ) [‘o’] = ((ﾟДﾟ)+‘ *‘) [ﾟΘﾟ];(ﾟoﾟ)=(ﾟДﾟ) [‘c’]+(ﾟДﾟ) [‘o’]+(ﾟωﾟﾉ +’* ’)[ﾟΘﾟ]+ ((ﾟωﾟﾉ</mark> 3) +’ *‘) [ﾟｰﾟ] + ((ﾟДﾟ) +’* ‘) [(ﾟｰﾟ)+(ﾟｰﾟ)]+ ((ﾟｰﾟ <mark>3) +‘_’) [ﾟΘﾟ]+((ﾟｰﾟ</mark> 3) +’ *‘) [(ﾟｰﾟ) - (ﾟΘﾟ)]+(ﾟДﾟ) [‘c’]+((ﾟДﾟ)+’* ‘) [(ﾟｰﾟ)+(ﾟｰﾟ)]+ (ﾟДﾟ) [‘o’]+((ﾟｰﾟ <mark>3) +‘ *‘) [ﾟΘﾟ];(ﾟДﾟ) [’* ’] =(o<sup>_</sup>o) [ﾟoﾟ] [ﾟoﾟ];(ﾟεﾟ)=((ﾟｰﾟ</mark> 3) +’ *‘) [ﾟΘﾟ]+ (ﾟДﾟ) .ﾟДﾟﾉ+((ﾟДﾟ)+’* ‘) [(ﾟｰﾟ) + (ﾟｰﾟ)]+((ﾟｰﾟ <mark>3) +‘_’) [o<sup>_</sup>o -ﾟΘﾟ]+((ﾟｰﾟ</mark> 3) +’ *‘) [ﾟΘﾟ]+ (ﾟωﾟﾉ +’* ‘) [ﾟΘﾟ]; (ﾟｰﾟ)+=(ﾟΘﾟ); (ﾟДﾟ)[ﾟεﾟ]=’\‘; (ﾟДﾟ).ﾟΘﾟﾉ=(ﾟДﾟ+ ﾟｰﾟ)[o<sup>_</sup>o -(ﾟΘﾟ)];(oﾟｰﾟo)=(ﾟωﾟﾉ +’ *‘)[c<sup>_</sup>o];(ﾟДﾟ) [ﾟoﾟ]=’"‘;(ﾟДﾟ) [’* ‘] ( (ﾟДﾟ) [’ *‘] (ﾟεﾟ+(ﾟДﾟ)[ﾟoﾟ]+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((o<sup>_</sup>o) +(o<sup>_</sup>o))+ ((ﾟｰﾟ) + (o<sup>_</sup>o))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟｰﾟ)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ (ﾟｰﾟ)+ (o<sup>_</sup>o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ ((ﾟｰﾟ) + (o<sup>_</sup>o))+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟΘﾟ)+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ ((ﾟｰﾟ) + (ﾟΘﾟ))+ (ﾟДﾟ)[ﾟεﾟ]+((o<sup>_</sup>o) +(o<sup>_</sup>o))+ (o<sup>_</sup>o)+ (ﾟДﾟ)[ﾟεﾟ]+(ﾟｰﾟ)+ (ﾟΘﾟ)+ (ﾟДﾟ)[ﾟoﾟ]) (ﾟΘﾟ)) (’* ');

确认为aaEncode编码，使用在线工具解得 `welcom3!` 

[在线工具：https://toolwa.com/aaencode/](https://toolwa.com/aaencode/) 

 ![aaEncode 加解密页：把 JavaScript 转成颜文字，输入框里是 welcom3!](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190603819.png)

2、剩下一张图片，再加上一个密码，确认使用steghide工具加密。

steghide下载地址： [https://sourceforge.net/projects/steghide/](https://sourceforge.net/projects/steghide/) 

使用如下命令，得到隐藏信息。

```shell
steghide extract -sf hh.jpg -p welcom3!
```

 ![命令行 steg hide.exe extract -sf hh.jpg -p welcom3! → wrote extracted data to "secret.txt"](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190605825.png)

 ![secret.txt 写着「想要flag吗？解出我的密文吧~」，下面是 U2FsdGVkX1/7KeHVI5984OsGUVSanPfPednHpK9lKvp0kdrxO4Tj/Q==](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190606909.png)

在hh.jpg的属性找到密钥： `@#$%^&%%$)` 

 ![hh.jpg 属性 → 详细信息：标题「这里有密钥」、主题「@#$$%^&&%%$)」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190608265.png)

解得如下信息：

```python
think about stegosaurus
```

DES加解密： [https://www.sojson.com/encrypt_des.html](https://www.sojson.com/encrypt_des.html) 
 ![DES 解密页：明文 think about stegosaurus、密码 @#$$%^&&%%$)、密文那串，显示解密成功](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190609623.png)

3、搜索发现对应题目“剑龙”，但其实指的是stegosaurus pyc隐写工具。

 ![搜 stegosaurus 才明白题目为什么叫剑龙，也顺带指到了同名工具](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190611323.png)

O_O文件的确是一个pyc文件。

 ![kali 里 file 0_0 → python 3.6 byte-compiled](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190613551.png)

stegosaurus下载地址： [https://github.com/AngelKitty/stegosaurus](https://github.com/AngelKitty/stegosaurus) 

 ![GitHub 上的 AngelKitty/stegosaurus：把 payload 藏进 Python 字节码的隐写工具](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190614895.png)

运行脚本加上 `-x` 参数，得到flag： `flag{3teg0Sauru3_!1}` 

 ![python stegosaurus.py -x 0_0.pyc → Extracted payload: flag{3teg0Sauru3_!1}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190617000.png)

### flag：

```bash
 flag{3teg0Sauru3_!1}
```