---
title: "BUUCTF 菜刀666 1"
date: 2024-02-23 10:59:54
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "网络安全"
- "笔记"
- "CTF"
- "BUUCTF"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193306916.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「菜刀666 1」：流量分析，你能找到flag吗](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193308775.png)

### 题目描述：

流量分析，你能找到flag吗 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.pcapng文件。
 ![666666.pcapng，2017/12/8 19:43，2,310 KB](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193310376.png)

---

### 解题思路：

1、双击文件，打开wireshark。根据题目提示，我们过滤出POST数据包。（菜刀一般使用POST上传）

```bash
http.request.method==POST
```

 ![过滤 http.request.method==POST，清一色是 192.168.25.128 → 192.168.43.83 的 POST /upload/1.php](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193311843.png)

简单看了几个数据包之后，发现有个数据包不太正常。

![同一列表往下翻，第 1068 号包长达 5033 字节（红框），明显比别的大一截](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193313943.png)

追踪流看一下，发现和前面几个数据包不同，有很长一串16进制字符串。
 ![追踪 HTTP 流 eq 7：请求体是 aa=@eval.(base64_decode($_POST［action］));&action=…，后面一大串 base64 拼十六进制](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193316282.png)

2、找到z1后面的字符串，通过Base64编码转换为明文信息，猜测z2对应的这串16进制字符串是一张jpg文件的数据。

 ![这段代码里能看到 z1= 与 z2= 两个参数](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193319320.png)

 ![Base64 工具里输入 D:\wamp64\www\upload\6666.jpg，就是 z1 解出来的路径](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193321625.png)

通过z2的文件头“FF D8”和文件结尾“FF D9”也可以确定这是一张jpg图片。

```bash
JPEG (jpg)
文件头：FF D8 FF　　　　　　　　　　　　　　　　　　　　　　　 
文件尾：FF D9
```

新建一个txt文件，将z2的16进制数据复制到txt文本中，保存。在010Editor中，使用“文件”选项卡的“导入16进制文件”选项，导入刚才新建的txt文件。

 ![z2 那一段粘到记事本，是完整的 JPEG 十六进制，FF D8 FF E0 开头](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193323272.png)

![010 Editor 的「文件 → 导入十六进制」（红框）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193325509.png)

保存为.jpg文件，打开图片，得到一个字符串，但不是flag。（提示我们这是密码）

![Kali 默认壁纸（蓝底白龙）上写着红字 Th1s_1s_p4sswd_!!!](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193327342.jpeg)

```bash
Th1s_1s_p4sswd_!!!
```

3、回Wireshark接着看之前的数据包，在下面找到一个zip压缩包，又在倒数第二个数据包的追踪流中，找到pk标识，确认该pcapng文件内有一个zip压缩包。（wireshark 截取的流量中，会截取文件传输对应的流量，也就是说，这个pcapng文件将包括 zip压缩包）

![响应里是目录列表：1.php、6666.jpg、flag.txt，还有红框框出的 hello.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193329696.png)

![同一响应末尾接着 PK 开头的 zip 内容，最后一行写 well, you need passwd!](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193331310.png)

使用Kali中的foremost工具，将zip压缩包从pcapng文件中提取出来。

 ![binwalk 666666.pcapng：一堆 xz compressed data，1422689 处是加密 Zip、name: flag.txt；foremost 后 tree 得到 audit.txt 与 zip/00002778.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193333103.png)

尝试解压压缩包，需要密码，想起来之前拿到的密码，使用密码解压，得到flag.txt文件。打开拿到flag。

 ![unzip 00002778.zip 先打印 well,you need passwd!，输密码后 extracting: flag.txt](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193335505.png)

 ![cat flag.txt → flag{30pWdJ-JP6FzK-koCMAK-VkfWBq-75Un2z}](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193337337.png)

### flag：

```bash
flag{3OpWdJ-JP6FzK-koCMAK-VkfWBq-75Un2z} 
```