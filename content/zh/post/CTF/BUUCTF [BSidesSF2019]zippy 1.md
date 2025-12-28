---
title: "BUUCTF [BSidesSF2019]zippy 1"
date: 2025-10-20 08:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "misc"
- "BSidesSF2019"
- "流量分析"
- "zippy"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190843252.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190845318.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件，得到attachment.pcapng文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190846936.png)

---

### 解题思路：

1、打开attachment.pcapng文件，流量很少，简单浏览发现执行了两条命令。

```bash
nc -l -p 4445 > flag.zip
使用 netcat 工具监听端口 4445 并将接收到的数据重定向到 flag.zip 文件。
unzip -P supercomplexpassword flag.zip
使用 unzip 工具来解压 flag.zip 文件。-P 选项后面跟的是解压密码 supercomplexpassword。
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190848070.png)

其中，保存下来的flag.zip文件是解题的关键，解压密码 `supercomplexpassword` 也有用。

2、在Kali中，使用foremost工具分离流量包中的flag.zip文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190849813.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190851583.png)

使用解压密码 `supercomplexpassword` 解压flag.zip文件，得到flag.txt文件，得到flag

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190854676.png)

### flag：

```bash
CTF{this_flag_is_your_flag}
flag{this_flag_is_your_flag}
```