---
title: "BUUCTF 从娃娃抓起 1"
date: 2024-09-21 20:19:09
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "密码学"
- "BUUCTF"
- "MISC"
- "CTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192608476.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192610962.png)

### 题目描述：

伟人的一句话，标志着一个时代的开始。那句熟悉的话，改变了许多人的一生，为中国三十年来计算机产业发展铺垫了道路。两种不同的汉字编码分别代表了汉字信息化道路上的两座伟大里程碑。请将你得到的话转为md5提交，md5统一为32位小写。

### 密文：

```bash
0086 1562 2535 5174
bnhn s wwy vffg vffg rrhy fhnv
```

请将你得到的这句话转为md5提交，md5统一为32位小写。
提交格式：flag{md5}

---

### 解题思路：

1、根据题目描述，在浏览器搜索“伟人的一句话，标志着一个时代的开始。”，可以找到“计算机普及要从娃娃抓起。”，但这不是flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192612641.png)

2、根据题目“两种不同的汉字编码”的提示，结合两串密文，分别指中文电码和五笔编码。

```bash
0086 1562 2535 5174
人    工   智   能
bnhn s  wwy vffg vffg rrhy fhnv
也   要  从   娃   娃    抓   起
```

明文：

```bash
人工智能也要从娃娃抓起
```

[中文电码查询](https://dianma.bmcx.com/) 

> 中文电码表采用了四位阿拉伯数字作代号，从0001到9999按四位数顺序排列，用四位数字表示最多一万个汉字、字母和符号。汉字先按部首，后按笔划排列。字母和符号放到电码表的最尾。后来由于一万个汉字不足以应付户籍管理的要求，又有第二字面汉字的出现。在香港，两个字面都采用同一编码，由输入员人手选择字面；在台湾，第二字面的汉字会在开首补上“1”字，变成5个数字的编码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192614716.png)

[五笔编码查询](http://wubi.118cha.com/109728.html) 

> 五笔编码是一种汉字输入法的编码方式，由王永民在1983年发明，称为“五笔字型”，是中国大陆地区早期最流行的一种汉字输入法。它的基本原理是将汉字拆分成若干个字根（偏旁部首或笔画），然后根据这些字根在键盘上的分布位置进行编码。
> 五笔编码把汉字分为三个层次：字根、键名字和成字字根。它使用26个英文字母键作为基本编码单元，通过击打不同的字母组合来输入汉字。每个汉字都可以通过最多四码的方式输入，前两码为该汉字的主要字根，后两码则是其他字根或识别码，以确保唯一性。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192616372.gif)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192618534.png)

3、将得到的字符串进行32位小写MD5加密，得到flag。
[md5在线加密](http://www.ttmd5.com/hash.php?type=0) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192620148.png)

### flag：

```bash
flag{3b4b5dccd2c008fe7e2664bd1bc19292}
```