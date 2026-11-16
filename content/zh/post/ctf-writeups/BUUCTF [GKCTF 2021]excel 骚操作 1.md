---
title: "BUUCTF [GKCTF 2021]excel 骚操作 1"
date: 2025-06-23 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "Office"
- "网络安全"
- "BUUCTF"
- "CTF"
- "MISC"
---

![题图：蓝发动漫女孩，配字「刷题嘛~？」「一起吖~」](/images/blog/20251228190938943.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [2021GKCTF Misc excel骚操作–详解](https://blog.csdn.net/qq_43871179/article/details/118310357)
- [中国编码APP下载](http://appserver.gs1cn.org/ancc2020h/)


 ![题目页「［GKCTF 2021］excel 骚操作 1」：你真的了解 excel 吗，附件 flag.xlsx](/images/blog/20251228190940931.png)

### 题目描述：

你真的了解excel吗（flag由flag头包裹

### 密文：

下载附件，得到flag.xlsx文件

 ![flag.xlsx，13 KB，显示为 Microsoft Excel 工作表](/images/blog/20251228190942599.png)

---

### 解题思路：

1、打开文件，hint： `我看见flag了，你呢？` 

 ![Excel 打开，A1 单元格里写着「我看见flag了，你呢？」](/images/blog/20251228190943700.png)

随便点击几个地方，发现有的单元格有数字 `1` ，而且不是均匀分布的。

 ![选中 E6，编辑栏显示 1，可单元格本身是空白](/images/blog/20251228190945457.png)

在这里，可以全选单元格，设置单元格格式中的数字类型为 `G/通用格式` ，就可以直观看到所有含数字 `1` 的单元格。

 ![设置单元格格式 → 数字 → 自定义，红框标出「自定义」与类型框里的 G/通用格式](/images/blog/20251228190946800.png)

 ![视图缩到 10%，满屏的 1 拼成一张二维码](/images/blog/20251228190948348.png)

再通过单元格 `条件格式` ，将单元格中所有数字等于 `1` 的单元格标黑，看它的图案。

 ![条件格式 → 突出显示单元格规则 → 等于(E)...](/images/blog/20251228190950045.png)

 ![「等于」对话框里填 1，格式选「自定义格式...」](/images/blog/20251228190951477.png)

最后调整单元格行高为27，可以发现这是个“码”。

 ![设好之后 1 变成黑块，二维码清晰浮现](/images/blog/20251228190953273.png)

2、这其实是一个叫 **汉信码** 的二维码，需要使用中国编码网的中国编码APP，扫码得到flag。

下载地址： [http://appserver.gs1cn.org/ancc2020h/](http://appserver.gs1cn.org/ancc2020h/) 

 ![手机扫码结果：smsto:1351100000:flag{9ee0cb62-f443-4a72-e9a3-43c0b910757e}，码制 HANXIN](/images/blog/20251228190954674.jpeg)

要想实现文中的隐藏效果，可以采取以下步骤：
1、单元格输入数字
2、单元格格式，数字选择自定义
3、类型中输入；；；

### flag：

```bash
flag{9ee0cb62-f443-4a72-e9a3-43c0b910757e}
```