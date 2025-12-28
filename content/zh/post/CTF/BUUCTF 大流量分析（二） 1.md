---
title: "BUUCTF 大流量分析（二） 1"
date: 2025-05-12 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "Wireshark"
- "Misc"
- "BUUCTF"
- "CTF"
- "网络安全"
- "安全"
- "大流量分析"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192851996.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：大流量分析（二）](https://blog.csdn.net/mochu7777777/article/details/110494041) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192854069.png)

### 题目描述：

黑客对A公司发动了攻击，以下是一段时间内获取到的流量包，那黑客使用了哪个邮箱给员工发送了钓鱼邮件?（答案加上flag{}）附件链接: https://pan.baidu.com/s/1EgLI37y6m9btzwIWZYDL9g 提取码: 9jva 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，有很多pcap流量包。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192856383.png)

---

### 解题思路：

1、看第一个流量包，钓鱼邮件一般出现在攻击前期，寻找SMTP流量。

> 邮件协议：POP、SMTP、IMAP

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192858934.png)

追踪TCP流，发现几个邮箱。（拥有同样后缀@t3sec.cc的邮箱，应该是公司员工的邮箱）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192901470.png)

往下查看流量，发现一段Base64编码的数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192903711.png)

解码后，发现疑似钓鱼邮件，确认黑客使用的邮箱是： `xsser@live.cn` 。

```python
tPO80rrDoaMNCiAgICAgvPjT2rmry77N+MLnvNy5ubjEtq+jrLK/t9bTptPD0OjSqsn9vLajrL7J
sOaxvm1haWyhom9hoaJjcm21yM+1zbPW8LK9vavM5ru7o6zH67TzvNK1x8K8aHR0cDovLzExOC4x
OTQuMTk2LjIzMjo4MDg0L2dldC5waHAgzO7QtNfUvLq1xNXKusXS1LHjxeS6z8+1zbPJ/by2oaMN
CiAgICDQu9C7tPO80qOhDQoNCg==
```

```python
大家好。
     鉴于公司网络架构改动，部分应用需要升级，旧版本mail、oa、crm等系统逐步将替换，请大家登录http://118.194.196.232:8084/get.php 填写自己的帐号以便配合系统升级。
    谢谢大家！
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192905527.png)

### flag：

```bash
flag{xsser@live.cn}
```