---
title: "BUUCTF 金三 1"
date: 2025-12-01
draft: false
tags: ["CTF", "MISC"]
categories: ["比赛"]
author: "yuexuan521"
---

# BUUCTF 金三 1

![](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030226.png)

**BUUCTF:[https://buuoj.cn/challenges](https://buuoj.cn/challenges)**

---

@[TOC]

---
相关阅读
[CTF Wiki](https://ctf-wiki.org/)




![aaa](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030227.gif)

## 题目描述：
只有一个附件，下载下来有一张GIF图片。


## 解题思路：
本题一共有2种解法（本人找到的）
### 方法一：
1、打开这张GIF图片，观察到不正常闪动，似乎有东西藏在图片中。
2、使用StegSolve工具，对图片进行逐帧查看。

在StegSolve中打开GIF图片
![在这里插入图片描述](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030228.png)
打开逐帧查看
![在这里插入图片描述](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030230.png)
在这里可以逐帧查看图片（理论上可以在这里看到逐帧的图片，但我这显示不出来）
![在这里插入图片描述](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030231.png)
3、找到三张带有flag的图片，拼在一起得到flag。(中间的那张图片“he11o”，中间是两个数字1)
![43aff79438b2561c701cc8efaa1204bf](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030232.bmp)![0aeb23246a3ca6ec9ede79542f797edd](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030233.bmp)![2d4481e6dac0ba900021823fad7df144](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030234.bmp)

### 方法二：
1、使用Photoshop打开GIF图片，也可以逐帧查看图片。
![在这里插入图片描述](https://raw.githubusercontent.com/yuexuan521/image/main/202512021030235.png)


## flag：

```bash
flag{he11ohongke}
```