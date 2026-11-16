---
title: "BUUCTF 弱口令 1"
date: 2024-09-21 20:16:14
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "隐写"
- "CTF"
- "BUUCTF"
- "MISC"
- "密码学"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228192951217.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「弱口令 1」，附件 6ecf18ee-d0…](/images/blog/20251228192953740.png)

### 题目描述：

老菜鸡，伤了神，别灰心，莫放弃，试试弱口令注意：得到的 flag 请包上 flag{} 提交

### 密文：

---

### 解题思路：

得到一个zip压缩包，但是有密码。用Bandizip打开，得到一些空格和换行符组成的信息。如下
 ![压缩包里只有一个 女神.png，292,528 字节，预览是蓝色柱状图](/images/blog/20251228192955869.png)

```bash
    
 
 	  
 	  
					
  	 
			
 	 
  	
		
```

 ![打开图片，是一张蓝色柱状图，形状像一面旗](/images/blog/20251228192957671.png)

使用SublimeText打开，选中可以更清楚地看到，看着很像摩斯密码，实际上就是。

 ![加上行号 1–10，每行柱长明显不同](/images/blog/20251228192958859.png)

用记事本手抄一下

```bash
.... . .-.. .-.. ----- ..-. --- .-. ..- --
```

使用在线工具解码，得到zip压缩包的密码

[在线摩斯密码翻译器](https://www.lddgo.net/encrypt/morse) 
 ![摩尔斯解码工具把点划解成 HELL0FORUM](/images/blog/20251228193000203.png)

解压缩zip压缩包，得到一张名为“女神.png”的图片
 ![女神.png 真正的画面是一个女孩的插画](/images/blog/20251228193001786.png)

尝试了各种方法均无效，最后使用lsb隐写提取出flag，使用密钥123456，对应题目“弱口令”。（虽说是lsb隐写，但用Stegsolve没有发现）
 ![cloacked-pixel 里执行 lsb.py extract 女神.png 1.txt 123456，cat 出 flag{jsy09-wytg5-wius8}](/images/blog/20251228193004055.png)

这里使用工具cloacked-pixel，python2环境配置比较麻烦。 [https://github.com/livz/cloacked-pixel](https://github.com/livz/cloacked-pixel) 

### flag：

```bash
flag{jsy09-wytg5-wius8}
```