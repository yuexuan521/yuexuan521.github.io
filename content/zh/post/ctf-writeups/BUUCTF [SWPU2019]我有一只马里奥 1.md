---
title: "BUUCTF [SWPU2019]我有一只马里奥 1"
date: 2024-09-22 20:32:01
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

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228191939865.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「［SWPU2019］我有一只马里奥 1」，附件 attachment.e…](/images/blog/20251228191941958.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到一个.exe文件。
 ![attachment.exe，224 KB，显示为应用程序，图标是马里奥](/images/blog/20251228191943635.png)

---

### 解题思路：

1、双击.exe文件运行，得到一个1.txt文本。打开，如下图。

 ![解压后目录里是 ntf s 与 flag.txt](/images/blog/20251228191945445.png)

2、提示我们这是一个NTFS交换数据流的宿主文件，内部可能隐藏一个flag.txt文件。有两个方法可以得到flag.txt文件的内容。
[NTFS 交换数据流 实现隐藏文件](https://zhuanlan.zhihu.com/p/654643812) 

方法一：
在1.txt文件所在的文件夹，右键选择“在终端中打开”，打开命令行输入以下命令，回车打开flag.txt文件。

```bash
notepad 1.txt:flag.txt
```

![命令行里执行 notepad 1.txt:flag.txt](/images/blog/20251228191947280.png)

![记事本打开 1.txt:flag.txt，内容 swupctf{ddg_is_cute}](/images/blog/20251228191948669.png)

方法二：
使用工具NtfsStreamsEditor或AlternateStreamView打开存放1.txt文件的文件夹，扫描出现隐藏文件文件，导出后打开，得到flag。

[AlternateStreamView(跳转页面后，向下滑动，下载对应的32或64位软件)](https://www.nirsoft.net/utils/alternate_data_streams.html) 

 ![AlternateStreamView 列出 .:flag.txt:$DATA 这条流，大小 20 字节](/images/blog/20251228191950540.png)

[NtfsStreamsEditor下载](http://soft.onlinedown.net/soft/47981.htm) 

 ![NtfsStreamsEditor2 搜出 1.txt:flag.txt 数据流，20 字节、可疑度 1](/images/blog/20251228191952063.png)

 ![打开该数据流，内容同样是 swupctf{ddg_is_cute}](/images/blog/20251228191954156.png)

### flag：

```bash
flag{ddg_is_cute}
```