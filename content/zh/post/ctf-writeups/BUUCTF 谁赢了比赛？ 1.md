---
title: "BUUCTF 谁赢了比赛？ 1"
date: 2024-09-22 20:28:22
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "安全"
- "网络安全"
- "CTF"
- "笔记"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193518927.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193521476.png)

### 题目描述：

小光非常喜欢下围棋。一天，他找到了一张棋谱，但是看不出到底是谁赢了。你能帮他看看到底是谁赢了么？ 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一个.png图片。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193523194.png)

---

### 解题思路：

1、放到010Editor中看一下，找到“Rar”文件头，猜测隐藏了rar压缩包。使用Kali中的binwalk工具进行检测，发现rar压缩包。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193526177.png)

使用binwalk加上“-e”参数，直接分离rar压缩包。

```bash
binwalk -e BitcoinPay.png
#如果出现报错，可以尝试在命令后加上“--run-as=root”参数
binwalk -e BitcoinPay.png --run-as=root
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193528028.png)

2、尝试解压rar压缩包，但是需要密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193529867.png)

使用ARCHPR工具，猜测是常见的4位纯数字进行破解，选定参数，破解得到密码为1020。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193531225.png)

用得到的密码解压rar压缩包，得到一张hehe.gif图片和一个flag.txt文本（这个里面当然没有flag，只有“where do you think the flag is? 你认为flag在哪里？”）。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193532894.png)

3、因为是.gif文件，可以使用StegSolve或者Photoshop分页查看，我的StegSolve无法显示，这里使用Photoshop为例。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193534373.png)

依次浏览图层，在第310个图层，发现了关于flag的提示，不过和之前的flag.txt文本相同的内容“do_you_know_where_is_the_flag 您知道flag在哪里吗？”。不过，这暗示我们flag就在这里，将图层310导出为png。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193536651.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193538454.png)

4、使用StegSolve打开图片，查看Red 0等通道发现一张二维码。（这一步我真的没想到）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193539553.png)

使用QR research扫描二维码，得到flag。（我们也知道最后是谁赢得了比赛！----山下敬吾）
[山下敬吾](https://baike.baidu.com/item/%E5%B1%B1%E4%B8%8B%E6%95%AC%E5%90%BE/4823035#%E5%AE%8C%E6%88%90700%E8%83%9C) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193541517.png)

### flag：

```bash
flag{shanxiajingwu_won_the_game}
```