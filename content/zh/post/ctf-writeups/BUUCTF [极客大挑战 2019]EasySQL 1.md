---
title: "BUUCTF [极客大挑战 2019]EasySQL 1"
date: 2024-09-21 20:16:44
category: "CTF Web"
categories: 
  - "CTF"
tags:
- "CTF"
- "网络安全"
- "web安全"
- "SQL注入"
- "BUUCTF"
- "WEB"
---



![](./assets/1_1.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 



---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](./assets/1_2.png)

### 题目描述：

[极客大挑战 2019]EasySQL 1

### 密文：

---

### 解题思路：

**1、根据题目提示，并且网站也存在输入框，尝试进行SQL注入。** 

首先，判断提交方式，随机输入数据
 ![在这里插入图片描述](./assets/1_3.png)

提交数据出现在URL中，确定为GET提交方式

**2、判断注入类型是字符型还是数字型** 

输入

```bash
1' 
```

 ![在这里插入图片描述](./assets/1_4.png)

查看是否有报错信息
 ![在这里插入图片描述](./assets/1_5.png)

```bash
You have an error in your SQL syntax; check the manual that corresponds to your MariaDB server version for the right syntax to use near '1'' at line 1
```

出现语法错误提示，确定为字符型注入

**3、判断注入点** 

使用

```bash
1' or 1=1#
```

如果结果返回了全部的内容，可以判断存在注入点
 ![在这里插入图片描述](./assets/1_6.png)

没想到这道题这么简单，仅仅判断注入点flag就出来了，连sqlmap都没用

### flag：

```bash
flag{60f94177-044e-40dd-8378-e49b803a8362}
```