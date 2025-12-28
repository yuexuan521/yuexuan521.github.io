---
title: "BUUCTF [SUCTF2018]followme 1"
date: 2024-12-16 09:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "CTF"
- "misc"
- "wireshark"
- "BUUCTF"
- "web安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191840322.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[Hello CTF](https://hello-ctf.com/HC_Start/) 
[NewStar CTF](https://ns.openctf.net/learn/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191842332.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。来源： [https://github.com/hebtuerror404/CTF_competition_warehouse_2018](https://github.com/hebtuerror404/CTF_competition_warehouse_2018) 

### 密文：

下载附件得到attachment.pcapng文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191844026.png)

---

### 解题思路：

1、有两种方法，先讲讲我的解题经过。我首先简单浏览了一下流量，发现有大量HTTP流量，并且似乎存在爆破行为。过滤出HTTP流量，查看到爆破密码的步骤，随便看一个就捡到了flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191845264.png)

#### 方法一：

使用Wireshark，在分组字节流中查找包含 `ctf` 的内容，可以找到flag

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191847456.png)

#### 方法二：

将全部HTTP对象导出

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191849628.png)

发现大量的文件如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191851559.png)

搜索文件中的关键词，使用grep命令，查找到flag

```bash
grep -r -i  "ctf"
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191853095.png)

### flag：

```bash
SUCTF{password_is_not_weak}
flag{password_is_not_weak}
```