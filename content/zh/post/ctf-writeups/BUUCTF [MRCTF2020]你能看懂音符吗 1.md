---
title: "BUUCTF [MRCTF2020]你能看懂音符吗 1"
date: 2024-05-21 22:56:29
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MISC"
- "网络安全"
- "CTF"
- "笔记"
- "BUUCTF"
---

![文章题图：蓝发动漫少女，配文「刷题嘛~？一起呀」](/images/blog/20251228191624009.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![BUUCTF 题目「［MRCTF2020］你能看懂音符吗 1」页面，附件 attachment.rar](/images/blog/20251228191626117.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢Galaxy师傅供题。

### 密文：

下载附件，得到一个rar压缩包。

 ![解压得到的 attachment_2.rar，仅 10 KB](/images/blog/20251228191628228.png)

---

### 解题思路：

1、尝试解压rar压缩包，出现错误无法解压。

![WinRAR 诊断：压缩文件格式未知或数据已被损坏](/images/blog/20251228191629610.png)

使用010Editor打开rar压缩包，发现该文件的rar文件头格式错误，修改为“52 61 72 21”，也可以使用WinHex修改。
（RARArchive(rar)， 文件头：52 61 72 21）

![十六进制开头是 61 52 72 21（aRr!），文件头被写坏（红框）](/images/blog/20251228191631038.png)

修改为

![改回 52 61 72 21（Rar!）后，右侧已能识别出 Rar! 标识（红框）](/images/blog/20251228191632839.png)

保存文件，再次解压得到.docx文件。

 ![修复后解出「你能看懂音符吗.docx」](/images/blog/20251228191634555.png)

2、查看.docx文件，内容如下。

 ![文档打开后只有一句「呀！一不小心把文档里的东西弄没了……」](/images/blog/20251228191638429.png)

使用010 Editor打开.docx文件，发现存在PK文件的文件头，确认为zip文件。
（ZIP Archive (zip)，文件头：50 4B 03 04）

 ![十六进制中可见 50 4B 03 04，说明 docx 其实是 ZIP](/images/blog/20251228191639777.png)

将文件另存为zip文件或修改后缀为.zip，解压成功。得到的文件如下图：

 ![解压 docx 得到 _rels、customXml、docProps、word 等目录](/images/blog/20251228191641631.png)

3、选择word文件夹的docunment.xml文件，右键在浏览器打开，找到与题目相符合的音符。

 ![document.xml 里藏着一串音符字符与「这都让你发现了，可是你能看懂吗？」](/images/blog/20251228191643377.png)

复制下来，使用在线工具将音符转换为明文，得到flag。
[文本加密为音乐符号](https://www.qqxiuzi.cn/bianma/wenbenjiami.php?s=yinyue) 

 ![文本加密为音乐符号工具，解密得到 MRCTF{thEse_n0tes_ArE_am@zing~}](/images/blog/20251228191645278.png)

---

另一种方法，可以查看到word中的隐藏文字。

选择文件

![Word 的「文件」菜单（红框）](/images/blog/20251228191647172.png)

选择选项

![文件菜单里点「选项」（红框）](/images/blog/20251228191648742.png)

选择显示，勾选“隐藏文字”，文档中就显示出了音符。

![Word 选项 → 显示，勾选「隐藏文字」（红框）](/images/blog/20251228191650666.png)

 ![勾选后正文里显出隐藏的音符串与那句提示](/images/blog/20251228191652518.png)

如果有的同学复制出现问题，可以尝试将全部文字选中，右键选择字体，把“隐藏”选项取消选中，就可以复制下啦。

![字体对话框的效果区勾选「隐藏」，用于取消隐藏后复制](/images/blog/20251228191654705.png)

### flag：

```bash
flag{thEse_n0tes_ArE_am@zing~}
```