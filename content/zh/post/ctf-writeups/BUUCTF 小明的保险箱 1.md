---
title: "BUUCTF 小明的保险箱 1"
date: 2024-11-01 11:37:26
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "笔记"
- "网络安全"
- "安全"
- "MISC"
- "图片隐写"
- "CTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192920231.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[Hello CTF](https://hello-ctf.com/HC_Start/) 
[NewStar CTF](https://ns.openctf.net/learn/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192922350.png)

### 题目描述：

小明有一个保险箱，里面珍藏了小明的日记本，他记录了什么秘密呢？。。。告诉你，其实保险箱的密码四位纯数字密码。

### 密文：

下载附件，得到一张.jpg图片。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192924269.jpeg)

---

### 解题思路：

1、读完题目，感觉这是一道图片隐藏文件的题目，另外还需要用到压缩包密码破解的知识。先使用010 Editor查看图片，没有找到PK（zip文件的标志），反而被一个域名和一些HTML源代码扰乱方向，StegSolve上也没有什么帮助。转换方向，使用Kali的binwalk工具，看到图片中隐藏了一个rar压缩包，确定目标。
[Windows平台参考思路](https://blog.csdn.net/weixin_45728231/article/details/120988424) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192925919.png)

2、我没有直接在Kali平台下直接分离文件，而是在Windows下，通过修改图片文件后缀名为.rar来实现。解压压缩包果然需要密码。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192928061.png)

3、根据题目的提示，密码为四位纯数字。使用RARP工具暴力破解密码，选定合适的约束条件可以大幅减少破解所需要的时间。破解得到的密码为7869。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192929495.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192931547.png)

3、使用密码来解压压缩包，得到2.txt文件，打开得到flag。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192933191.png)

### flag：

```bash
flag{75a3d68bf071ee188c418ea6cf0bb043}
```