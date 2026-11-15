---
title: "BUUCTF [SUCTF2018]followme 1"
date: 2024-12-16 09:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "MISC"
- "Wireshark"
- "BUUCTF"
- "Web安全"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191840322.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [Hello CTF](https://hello-ctf.com/HC_Start/)
- [NewStar CTF](https://ns.openctf.net/learn/)


 ![题目页「［SUCTF2018］followme 1」，附 GitHub 来源与 attachment.p…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191842332.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [https://github.com/hebtuerror404/CTF_competition_warehouse_2018](https://github.com/hebtuerror404/CTF_competition_warehouse_2018) 

### 密文：

下载附件得到attachment.pcapng文件

 ![attachment.pcapng，17,838 KB，显示为 Wireshark capture](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191844026.png)

---

### 解题思路：

1、有两种方法，先讲讲我的解题经过。我首先简单浏览了一下流量，发现有大量HTTP流量，并且似乎存在爆破行为。过滤出HTTP流量，查看到爆破密码的步骤，随便看一个就捡到了flag。

 ![Wireshark 过滤 http，多条 POST /build/index.php?action=login&_=…，下方表单里 password 值在 ASCII 区红框标出 SUCTF{password_is_not_weak}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191845264.png)

#### 方法一：

使用Wireshark，在分组字节流中查找包含 `ctf` 的内容，可以找到flag

 ![搜索框里输入 ctf，命中 100408 条，红框高亮 SUCTF{password_is_not_weak}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191847456.png)

#### 方法二：

将全部HTTP对象导出

 ![文件 → 导出 → HTTP 对象列表，列出 robots.txt、jquery.js 等，红框是「Save All」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191849628.png)

发现大量的文件如下：

 ![记事本打开 login%3f_=6975b9a9f7a359d322e…，内容是 name=admin&password=asdasdasd&referer=…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191851559.png)

搜索文件中的关键词，使用grep命令，查找到flag

```bash
grep -r -i  "ctf"
```

 ![kali 里 grep -r -i "ctf" 命中的结果，红框标出 SUCTF{password_is_not_weak}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191853095.png)

### flag：

```bash
SUCTF{password_is_not_weak}
flag{password_is_not_weak}
```