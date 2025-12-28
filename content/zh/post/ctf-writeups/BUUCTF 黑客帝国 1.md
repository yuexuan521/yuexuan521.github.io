---
title: "BUUCTF 黑客帝国 1"
date: 2024-09-21 22:58:48
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "笔记"
- "BUUCTF"
- "网络安全"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193704544.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193706664.png)

### 题目描述：

Jack很喜欢看黑客帝国电影，一天他正在上网时突然发现屏幕不受控制，出现了很多数据再滚屏，结束后留下了一份神秘的数据文件，难道这是另一个世界给Jack留下的信息？聪明的你能帮Jack破解这份数据的意义吗？ 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.txt文本，内容如下。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193708533.png)

---

### 解题思路：

1、查看.txt文本的内容，似乎全部是十六进制的数据，在010 Editor看一下。
在010Editor中，使用“文件”选项卡的“导入16进制文件”选项，导入刚才的txt文件。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193711257.png)

确认这是rar压缩包的文件头，保存文件为rar压缩包。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193713122.png)

2、尝试解压得到的rar压缩包，需要密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193715301.png)

因为没有关于密码的提示，所以使用常用的4位纯数字进行破解。使用ARCHPR工具，选定参数，破解得到密码为3690。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193717157.png)

使用密码解压rar压缩包，得到.png文件，打开如下图。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193719250.png)

3、使用010 Editor打开.png文件，弹出错误。观察到.png文件的文件头与文件尾不相符，可能被修改了文件头。（）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193721183.png)

(PNG（png)文件头：89 50 4E 47　文件尾：AE 42 60 82）
(JPEG (jpg)文件头：FF D8 FF　　文件尾：FF D9　)

这是png文件头，但后面接的“JFIF”是jpg图片的文件头。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193722539.png)

这是jpg文件尾

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193724390.png)

将文件头修改为jpg的文件头，保存文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193726153.png)

4、打开修改过的jpg图片，得到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193727989.png)

### flag:

```bash
flag{57cd4cfd4e07505b98048ca106132125}
```