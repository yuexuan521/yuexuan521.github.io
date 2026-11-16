---
title: "BUUCTF 数据包中的线索 1"
date: 2024-01-24 16:33:26
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "BUUCTF"
- "Wireshark"
- "流量分析"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228193015587.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「数据包中的线索 1」，附件 94b9c18e-d5…](/images/blog/20251228193017648.png)

### 题目描述：

公安机关近期截获到某网络犯罪团伙在线交流的数据包，但无法分析出具体的交流内容，聪明的你能帮公安机关找到线索吗？

### 密文：

下载附件，解压得到一个.pcapng文件。

![流量中的线索.pcapng，143 KB，显示为 Wireshark capture](/images/blog/20251228193019508.png)

---

### 解题思路：

1、双击.pcapng文件，进入Wireshark中。我们首先过滤HTTP流量，然后追踪HTTP流，找到很多的特殊信息，似乎经过Base64编码加密。

![过滤 http 后只剩 4 条，GET /sdk/vimService?wsdl 与 GET /fenxi.php](/images/blog/20251228193020917.png)

```bash
base64的编码表是由（A-Z、a-z、0-9、+、/）64个可见字符构成，“=”符号用作后缀填充。

base64: aGVsbG8sd29ybGQuMTIzNDY1
特征：大小写字母（A-Z，a-z）和数字（0-9）以及特殊字符‘+’，‘/’，不满3的倍数，用‘=’补齐。
```

![跟踪 TCP 流，响应体是一大段 base64](/images/blog/20251228193022716.png)

2、我使用Base64编码工具进行解码，得到的都是乱码，没有有意义的文本。看了别人的题解，才知道解码出的内容是一个jpg图片的数据。我们使用这个Base64 在线网站，将得到的密文进行解码，网站提醒我们可以另存为jpg文件，点击另存为，我们会下载一张jpg图片。
[Base64 在线解码、编码](https://the-x.cn/encodings/Base64.aspx) 

![Base64 在线解码，右侧提示「该内容已经被插件识别为二进制数据」，并给出 500×480、JFIF](/images/blog/20251228193024887.png)

3、查看图片得到flag。

![解码后恢复出图片：路飞竖大拇指，顶部 flag{209acebf6324a09671abc31c869de72c}](/images/blog/20251228193027464.jpeg)

### flag：

```bash
flag{209acebf6324a09671abc31c869de72c}
```