---
title: "BUUCTF old-fashion 1"
date: 2024-09-25 20:43:44
category: "BUUCTF Crypto"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "密码学"
- "网络安全"
- "CTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204835730.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「old-fashion 1」，附件 ec8bd2cf-5fa…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204837870.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

```c
Os drnuzearyuwn, y jtkjzoztzoes douwlr oj y ilzwex eq lsdexosa kn pwodw tsozj eq ufyoszlbz yrl rlufydlx pozw douwlrzlbz, ydderxosa ze y rlatfyr jnjzli; mjy gfbmw vla xy wbfnsy symmyew (mjy vrwm qrvvrf), hlbew rd symmyew, mebhsymw rd symmyew, vbomgeyw rd mjy lxrzy, lfk wr dremj. Mjy eyqybzye kyqbhjyew mjy myom xa hyedrevbfn lf bfzyewy wgxwmbmgmbrf. Wr mjy dsln bw f1_2jyf-k3_jg1-vb-vl_l
```

![一段替换密码密文，以 Os drnuzearyuwn 开头](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204839308.png)

---

### 解题思路：

1、观察密文特征，词频分析，使用爆破工具，工具 [quipqiup](https://quipqiup.com/) 。

![quipqiup 网站：Puzzle 里粘密文，Clues 填 G=R QWW=THE 后点 Solve](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204840895.png)

2、得到flag。（注意：得到的 flag 请包上 flag{} 提交）

### flag：

```c
n1_2hen-d3_hu1-mi-ma_a
```