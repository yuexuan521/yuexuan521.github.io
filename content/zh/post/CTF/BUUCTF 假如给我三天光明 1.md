---
title: "BUUCTF 假如给我三天光明 1"
date: 2024-09-23 22:59:32
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "安全"
- "Misc"
- "图片隐写"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192638057.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192640137.png)

### 题目描述：

下载附件，解压得到一个zip压缩包和一张.jpg图片。

### 密文：

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192642131.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192643531.jpeg)

### 解题思路：

其实做CTF题时，一定要紧紧的盯着那些明显的事物，优先解决它们，而不是浪费时间对一些细枝末节的地方走流程，要抓重点。

1、由于我们从附件中拿到了一个zip压缩包和一张.jpg图片，尝试解压压缩包，发现需要密码。而观察题目所给的图片，发现其下方似乎有一种图案密码，尝试对密码进行破译。经过查找，这是一种盲文语言。
[盲文](https://baike.baidu.com/item/%E7%9B%B2%E6%96%87/440901) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192645094.png)

2、我们依照盲文对照表进行翻译，得到明文kmdonowg。使用该明文解压压缩包，得到一个.wav的音频文件。通过Audacity工具和人耳辨认，判断该音频使用莫尔斯电码的形式隐藏了信息。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192647464.png)

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192649566.png)

```bash
第一个红框代表“-.”，第二个红框代表“ ”（空格）。
-.-. - ..-. .-- .--. . .. ----- ---.. --... ...-- ..--- ..--.. ..--- ...-- -.. --..
```

3、将这些莫尔斯电码使用在线工具转换，得到flag值。（得到的flag转换为小写字母，才能提交成功）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192650967.png)

### flag：

```bash
flag{wpei08732?23dz}
```