---
title: "BUUCTF 荷兰宽带数据泄露 1"
date: 2024-09-23 22:54:27
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "安全"
- "Misc"
- "路由"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193257943.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193259978.png)

### 题目描述：

下载附件，解压得到一个.bin文件。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193301660.png)

---

### 解题思路：

1、刚开始没什么思路，看了别人的题解，了解到一个新工具RouterPassView。大多数现代路由器都可以让您备份一个文件路由器的配置文件，该软件可以读取这个路由配置文件。联系题目，conf.bin文件可能为一个路由器的配置文件。我们用RouterPassView打开conf.bin文件，如下：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193302710.png)

2、查看文件没有发现什么特殊信息，题目也没有提示。我们尝试将用户名（Username）和密码（Password）作为flag值提交，最后确认用户名为flag值。（如果找不到Username、Password，可以使用Ctrl+f查找）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193304870.png)

### flag：

```bash
flag{053700357621}
```