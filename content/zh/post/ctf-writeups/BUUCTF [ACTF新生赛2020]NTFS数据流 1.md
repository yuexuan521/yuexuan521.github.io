---
title: "BUUCTF [ACTF新生赛2020]NTFS数据流 1"
date: 2024-03-21 20:49:16
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "CTF"
- "网络安全"
- "笔记"
- "BUUCTF"
- "MISC"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190502742.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［ACTF新生赛2020］NTFS数据流 1」，附件 attachment.tar](/images/blog/20251228190505306.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到一个.tar文件。
 ![attachment.tar 32 KB，显示为 WinRAR 压缩文件](/images/blog/20251228190507010.png)

---

### 解题思路：

1、解压.tar文件，再解压tmp文件夹下的flag.rar文件，得到500个.txt文件。随便看一个.txt文件，内容如下。

 ![WinRAR 打开 flag.rar，里面是 0.txt 到 18.txt，每个 16 字节](/images/blog/20251228190508429.png)

 ![用记事本打开 3.txt，内容只有一句 flag is not here](/images/blog/20251228190509977.png)

2、结合题目提示，这是一道关于NTFS交换数据流的题目。
[NTFS 交换数据流 实现隐藏文件](https://zhuanlan.zhihu.com/p/654643812) 

使用工具NtfsStreamsEditor或AlternateStreamView打开存放tmp文件夹，扫描出现NTFS交换数据流文件，导出后打开，得到flag（或双击查看）。

[AlternateStreamView(跳转页面后，向下滑动，下载对应的32或64位软件)](https://www.nirsoft.net/utils/alternate_data_streams.html) 

这里使用NtfsStreamsEditor
 ![NtfsStreamsEditor2 搜出 293.txt:flag.txt 数据流，27 字节，可疑度 1](/images/blog/20251228190511060.png)

双击查看，得到flag。

 ![打开该数据流，内容是 ACTF{AAAds_nntfs_ffunn?}](/images/blog/20251228190513128.png)

### flag：

```bash
flag{AAAds_nntfs_ffunn?} 
```