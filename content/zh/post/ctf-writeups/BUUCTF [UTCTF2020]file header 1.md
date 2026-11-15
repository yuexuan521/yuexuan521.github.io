---
title: "BUUCTF [UTCTF2020]file header 1"
date: 2024-01-21 20:12:50
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "Web安全"
- "CTF"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192112889.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［UTCTF2020］file header 1」，附件 attachment.p…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192114943.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。(最后提交形式为flag{xxx})

### 密文：

---

### 解题思路：

1、下载文件，我这里直接在网页打开了，选择“另存为”保存文件。
 ![浏览器打开 attachment.png 只显示破损图标，说明文件头被破坏](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192116520.png)

图片无法正常显示，再加上题目的提示“fileheader”，初步猜测文件头损坏。使用010 Editor打开，发现文件头缺失，文件尾对应png文件尾（AE 42 60 82），确认为png文件。

```bash
PNG (png)　　 文件头：89 50 4E 47　　　　　文件尾：AE 42 60 82
```

 ![十六进制视图，红框标出文件末尾的 AE 42 60 82，即 PNG 的 IEND](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192118149.png)

2、给文件添加缺失的png文件头（89 50 4E 47），新的文件头替换掉原来的空文件头（00 00 00 00）
 ![Notepad++ 里 attachment2.png 的开头，红框标出 89 50 4E 47，右侧显示 ‰PNG....IHDR](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192120099.png)

保存并查看，得到flag。
 ![HACKERMAN 迷因图，左上角写着 utflag{3lit3_h4ck3r}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192122051.png)

### flag：

```bash
flag{3lit3_h4ck3r}
```