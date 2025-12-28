---
title: "BUUCTF 被嗅探的流量 1"
date: 2024-09-24 22:49:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "笔记"
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
- "wireshark"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193502458.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193504562.png)

### 题目描述：

某黑客潜入到某公司内网通过嗅探抓取了一段文件传输的数据，该数据也被该公司截获，你能帮该公司分析他抓取的到底是什么文件的数据吗？

### 密文：

下载附件，解压得到一个.pcapng文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193506954.png)

### 解题思路：

1、双击.pcapng文件，在Wireshark中打开，开始分析流量。我首先大致浏览了一下流量，发现HTTP协议的流量有上传文件的痕迹。（upload上传）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193508795.png)

2、通过在顶部过滤器输入“http”语句，将HTTP流量过滤出来。（也可以使用“http.request.method==POST”语句实现更精确的过滤）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193511377.png)

3、我是将每一条上传的流量都追踪HTTP流，最后找到有flag的报文。其实，可以查看流量的提示信息更快的定位目标流量。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193513413.png)

4、在这条流量的提示信息中，我们看到包含JPEG图像，追踪这条流量的HTTP流，看到很多的数据，在数据的最下面找到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193515560.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193516983.png)

### flag：

```bash
flag{da73d88936010da1eeeb36e945ec4b97}
```