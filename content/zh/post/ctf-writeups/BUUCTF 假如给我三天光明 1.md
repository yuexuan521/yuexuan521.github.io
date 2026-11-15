---
title: "BUUCTF 假如给我三天光明 1"
date: 2024-09-23 22:59:32
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "MISC"
- "隐写"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192638057.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「假如给我三天光明 1」，附件 c103bd46-5a…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192640137.png)

### 题目描述：

下载附件，解压得到一个zip压缩包和一张.jpg图片。

### 密文：

![文件列表：music.zip（1 KB）与 pic.jpg（54 KB）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192642131.png)

![海伦·凯勒的图书封面，底部印着一行盲文点阵](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192643531.jpeg)

### 解题思路：

其实做CTF题时，一定要紧紧的盯着那些明显的事物，优先解决它们，而不是浪费时间对一些细枝末节的地方走流程，要抓重点。

1、由于我们从附件中拿到了一个zip压缩包和一张.jpg图片，尝试解压压缩包，发现需要密码。而观察题目所给的图片，发现其下方似乎有一种图案密码，尝试对密码进行破译。经过查找，这是一种盲文语言。
[盲文](https://baike.baidu.com/item/%E7%9B%B2%E6%96%87/440901) 

![盲文对照表，列出 a 到 z 的点位](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192645094.png)

2、我们依照盲文对照表进行翻译，得到明文kmdonowg。使用该明文解压压缩包，得到一个.wav的音频文件。通过Audacity工具和人耳辨认，判断该音频使用莫尔斯电码的形式隐藏了信息。

![Audacity 打开 music，8kHz 单声道 32 位浮点，波形是密集的通断块](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192647464.png)

![再放大，红框圈出短点与长划的宽度差异](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192649566.png)

```bash
第一个红框代表“-.”，第二个红框代表“ ”（空格）。
-.-. - ..-. .-- .--. . .. ----- ---.. --... ...-- ..--- ..--.. ..--- ...-- -.. --..
```

3、将这些莫尔斯电码使用在线工具转换，得到flag值。（得到的flag转换为小写字母，才能提交成功）

![摩尔斯解码（Decrypt）输出 CTFWPEIO8732?23DZ](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192650967.png)

### flag：

```bash
flag{wpei08732?23dz}
```