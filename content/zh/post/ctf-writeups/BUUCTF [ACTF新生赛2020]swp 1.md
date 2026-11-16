---
title: "BUUCTF [ACTF新生赛2020]swp 1"
date: 2024-06-21 20:42:51
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190528652.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［ACTF新生赛2020］swp 1」，附件 attachment.tar](/images/blog/20251228190530629.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到一个.tar文件。
 ![一个 tar 压缩包图标，文件名 attachment.tar](/images/blog/20251228190532595.png)

---

### 解题思路：

1、使用WinRAR解压.tar文件，得到两个.zip文件。

 ![解出两个 zip：._wget.zip（1 KB）与 wget.zip（6,885 KB）](/images/blog/20251228190534506.png)

解压wget.zip文件，得到.pcapng文件。

 ![还有 wget.pcapng，2020/1/30 23:47，7,878 KB](/images/blog/20251228190535869.png)

双击在Wireshark中打开，内容如下。

 ![包列表里混着 DNS、MDNS、LLMNR 的查询，域名都是 wpad.localdomain](/images/blog/20251228190537232.png)

根据wget.zip和wget.pcapng的文件名“wget”，上网搜一下，是一个在网络上下载文件的软件。既然如此，将http的流量过滤出来，导出文件看一下有什么文件。

 ![搜了下 wget：GNU Wget 是支持 HTTP、HTTPS、FTP 的下载工具](/images/blog/20251228190539255.png)

 ![文件 → 导出对象 → HTTP...](/images/blog/20251228190541542.png)

导出得到的文件如下。

 ![解压出的网页目录：一堆 js/css/图片，末尾还有 secret.zip、style.css、top.gif、webdig.js](/images/blog/20251228190543357.png)

2、找到一个secret.zip文件，尝试解压，需要密码。

 ![双击 secret.zip 里的 .flag.swp 时提示输入密码](/images/blog/20251228190545756.png)

使用Ziperello打开.zip压缩包，提示错误，猜测为ZIP伪加密。

 ![Ziperello 打开 secret.zip 直接报错「请打开包含加密文件的ZIP压缩包」](/images/blog/20251228190547155.png)

[zip伪加密原理](https://blog.csdn.net/qq_26187985/article/details/83654197) 
通过010 Editor修改压缩源文件数据区和目录区的全局方式位标记（下图红色标识），将伪压缩文件恢复到未加密的状态。

![十六进制 12D0h 处红框 00 00，可见 .flag.swp，后面还有第二个 PK 01 02 与 flag](/images/blog/20251228190549664.png)

```bash
未加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为00 00

伪加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为09 00

真加密：

文件头中的全局方式位标记为09 00

目录中源文件的全局方式位标记为09 00

ps:也不一定要09 00或00 00，只要是奇数都视为加密，而偶数则视为未加密
```

修改后，解压压缩包不需要密码，解压成功，得到两个文件。

 ![解出 .flag.swp（16 KB）与 flag 两个文件](/images/blog/20251228190551805.png)

3、使用Notepad++打开任意一个文件，都可以发现flag。

![Vim 打开 swp 恢复出未保存的内容，第 28 行红框里是 actf{…}，满屏 ^N](/images/blog/20251228190553556.png)

 ![切到恢复后的文件，第 63 行完整写着 actf{c5558bcf-26da-4f8b-b181-b61f3850b9e5}](/images/blog/20251228190555397.png)

### flag：

```bash
flag{c5558bcf-26da-4f8b-b181-b61f3850b9e5}
```