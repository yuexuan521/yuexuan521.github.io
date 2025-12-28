---
title: "BUUCTF 菜刀666 1"
date: 2024-02-23 10:59:54
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "安全"
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193306916.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193308775.png)

### 题目描述：

流量分析，你能找到flag吗 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.pcapng文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193310376.png)

---

### 解题思路：

1、双击文件，打开wireshark。根据题目提示，我们过滤出POST数据包。（菜刀一般使用POST上传）

```bash
http.request.method==POST
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193311843.png)

简单看了几个数据包之后，发现有个数据包不太正常。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193313943.png)

追踪流看一下，发现和前面几个数据包不同，有很长一串16进制字符串。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193316282.png)

2、找到z1后面的字符串，通过Base64编码转换为明文信息，猜测z2对应的这串16进制字符串是一张jpg文件的数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193319320.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193321625.png)

通过z2的文件头“FF D8”和文件结尾“FF D9”也可以确定这是一张jpg图片。

```bash
JPEG (jpg)
文件头：FF D8 FF　　　　　　　　　　　　　　　　　　　　　　　 
文件尾：FF D9
```

新建一个txt文件，将z2的16进制数据复制到txt文本中，保存。在010Editor中，使用“文件”选项卡的“导入16进制文件”选项，导入刚才新建的txt文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193323272.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193325509.png)

保存为.jpg文件，打开图片，得到一个字符串，但不是flag。（提示我们这是密码）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193327342.jpeg)

```bash
Th1s_1s_p4sswd_!!!
```

3、回Wireshark接着看之前的数据包，在下面找到一个zip压缩包，又在倒数第二个数据包的追踪流中，找到pk标识，确认该pcapng文件内有一个zip压缩包。（wireshark 截取的流量中，会截取文件传输对应的流量，也就是说，这个pcapng文件将包括 zip压缩包）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193329696.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193331310.png)

使用Kali中的foremost工具，将zip压缩包从pcapng文件中提取出来。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193333103.png)

尝试解压压缩包，需要密码，想起来之前拿到的密码，使用密码解压，得到flag.txt文件。打开拿到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193335505.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193337337.png)

### flag：

```bash
flag{3OpWdJ-JP6FzK-koCMAK-VkfWBq-75Un2z} 
```