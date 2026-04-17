---
title: "BUUCTF hashcat 1"
date: 2025-04-28 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "Misc"
- "hashcat"
- "office2john"
- "Office"
- "网络安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185908081.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

[CTF Wiki](https://ctf-wiki.org/) 

[HashCat 恢复Excel、Word、PPT密码保姆教程](https://blog.csdn.net/qq_39150356/article/details/135980077) 

[buuctf MISC - hashcat](https://blog.csdn.net/c868954104/article/details/134599989) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185910079.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，解压得到What kind of document is this_文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185911867.png)

---

### 解题思路：

1、文件名提示，需要确定文件类型。在010 Editor中打开，根据文件头 `D0 CF 11 E0` 确定是MS Office文件。

```python
MS Office文件      文件头：D0 CF 11 E0
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185912963.png)

但是MS Office文件包括MS Word/Excel/PPT三种文件，随便尝试一种文件，发现有密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185914958.png)

2、结合题目 `hashcat` ，我们使用HashCat恢复MS Office文件（Excel、Word、PPT）密码。

首先，使用office2john.py获取文件的hash值。

office2john.py文件下载地址： [https://github.com/magnumripper/JohnTheRipper/blob/bleeding-jumbo/run/office2john.py](https://github.com/magnumripper/JohnTheRipper/blob/bleeding-jumbo/run/office2john.py) 

CMD中打开，命令如下：

```python
python office2john.py "What kind of document is this_" > hash
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185916686.png)

得到文件的hash值如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185917937.png)

去掉文件名 `What kind of document is this_:` ，保留 `$office$` 及之后的数据。（从 `$office$*2010*` 可以得到该文件使用的是 **Office2010** 版本，后面要用到！）

```python
$office$*2010*100000*128*16*265c4784621f00ddb85cc3a7227dade7*f0cc4179b2fcc2a2c5fa417566806249*b5ad5b66c0b84ba6e5d01f27ad8cffbdb409a4eddc4a87cd4dfbc46ad60160c9
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185919283.png)

最后，使用HashCat根据hash值去解析密码。

HashCat官方网站： [https://hashcat.net/hashcat/](https://hashcat.net/hashcat/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185920623.png)

将hash文件放到hashcat文件夹下，CMD打开，命令如下：

```python
hashcat -m 9500 hash -a 3 ?d?d?d?d -w 3 -O
```

> 破解 Office 加密 Offcie 版本对应哈希类型
> Office97-03(MD5+RC4,oldoffice$0,oldoffice$1)：-m 9700
> Office97-03($0/$1, MD5 + RC4, collider #1)：-m 9710
> Office97-03($0/$1, MD5 + RC4, collider #2)：-m 9720
> Office97-03($3/$4, SHA1 + RC4)：-m 9800
> Office97-03($3, SHA1 + RC4, collider #1)：-m9810
> Office97-03($3, SHA1 + RC4, collider #2)：-m9820
> Office2007：-m 9400
> **Office2010：-m 9500** 
> Office2013：-m 9600

> Hashcat 中自定义破解含义值：
> ?l = abcdefghijklmnopqrstuvwxyz，代表小写字母。
> ?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ，代表大写字母。
> ?d = 0123456789，代表数字。
> ?s = !"#$%&'()*+,-./:;<=>?@[]^_`{|}~，代表特殊字符。
> ?a = ?l?u?d?s，大小写数字及特殊字符的组合。
> ?b = 0x00 - 0xff

出现下图所示内容，表示成功运行。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185922895.png)

在上条命令的后面加上 `--show` 参数，显示密码为 `9919` 。

```python
hashcat -m 9500 hash -a 3 ?d?d?d?d -w 3 -O --show
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185925153.png)

3、使用密码尝试Excel、Word、PPT文件，直到后缀为 `.ppt` 的PPT文件，才正常显示内容。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185926521.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185927923.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185928981.png)

存在7张空白的幻灯片，全选之后，到“设计”选项卡，选择“设置背景格式”，选择黑色纯色填充。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185931258.png)

在第七张幻灯片中，得到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228185933091.png)

### flag：

```bash
flag{okYOUWIN}
```

### HashCat常用实例

```python
8 位数字破解

Hashcat64 -m 9700 hash -a 3 ?d?d?d?d?d?d?d?d -w 3 –O

1-8 位数字破解

Hashcat -m 9700 hash -a 3 --increment --increment-min 1–increment-max 8 ?d?d?d?d?d?d?d?d

1 到 8 位小写字母破解

Hashcat -m 9700 hash -a 3 --increment --increment-min 1–increment-max 8 ?l?l?l?l?l?l?l?l

8 位小写字母破解

Hashcat -m 9700 hash -a 3 ?l?l?l?l?l?l?l?l -w 3 –O

1-8 位大写字母破解

Hashcat -m 9700 hash -a 3 --increment --increment-min 1–increment-max 8 ?u?u?u?u?u?u?u?u

8 位大写字母破解

Hashcat -m 9700 hash -a 3 ?u?u?u?u?u?u?u?u -w 3 –O

5 位小写+大写+数字+特殊字符破解

Hashcat -m 9700 hash -a 3 ?b?b?b?b?b -w 3
```

> 破解时采取先易后难的原则，建议如下：
> 
> - 使用 1-8 位数字进行破解。
> 
> - 使用 1-8 位小写字母进行破解。
> 
> - 使用 1-8 位大写字母进行破解。
> 
> - 使用 1-8位混合大小写+数字+特殊字符进行破解。
> 
> - 利用收集的公开字典进行破解。
> 
> 

