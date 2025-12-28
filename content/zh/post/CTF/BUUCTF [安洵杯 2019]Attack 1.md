---
title: "BUUCTF [安洵杯 2019]Attack 1"
date: 2024-12-30 08:45:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "网络安全"
- "CTF"
- "misc"
- "wireshark"
- "Mimikatz"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192325019.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[Hello CTF](https://hello-ctf.com/HC_Start/) 
[NewStar CTF](https://ns.openctf.net/learn/) 
[安洵杯2019 官方Writeup(Web/Misc) - D0g3](https://xz.aliyun.com/t/6911?time__1311=n4%2bxnD0DgDuAG=DOzNDsA3xCTWk8DcAgBmoD&u_atoken=125653dce1d42cc643b337d1c883f99f&u_asig=0a472f9017274948088853311e0043#toc-24) 
[[安洵杯 2019]Attack （详细解析）](https://blog.csdn.net/weixin_66146598/article/details/125129282) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192327104.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

下载附件解压，得到Attack.pcap文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192329001.png)

---

### 解题思路：

1、打开流量包，根据题目提示，寻找攻击流量。首先，发现攻击者进行了目录扫描，在靠后位置发现上传一句话木马

**目录扫描** 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192330113.png)

**上传一句话木马** 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192332602.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192334567.png)

往下分析，发现一组TCP流量疑似执行命令，请求流量经过base64混淆，返回流量使用ROT13加密

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192337801.png)

继续分析其他TCP流，发现目录下多出一个s3cret.zip文件。(据说，可以通过文件大小异常，推测文件中包含其他文件，使用foremost分离文件)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192339966.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192341804.png)

在下一组流量中，找到zip压缩包的“PK”文件头，以及一个flag.txt文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192343482.png)

2、将以“50 4B 03 04”开头的zip文件数据，拿出来保存为zip文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192345626.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192348223.png)

尝试解压，发现需要密码。根据压缩包hint提示，密码可能与administrator用户有关

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192350434.png)

继续分析流量，发现使用了procdump.exe这个工具，产生lsass.dmp文件

> Procdump工具一般用来抓取windows的lsass进程中的用户明文密码
> lsass是windows系统的一个进程，用于本地安全和登陆策略。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192352711.png)

接下来，攻击者下载了lsass.dmp文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192354293.png)

我们将lsass.dmp文件下载下来

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192356375.png)

> Mimikatz 是一款功能强大的轻量级调试神器，通过它你可以提升进程权限注入进程读取进程内存，当然他最大的亮点就是他可以直接从 lsass.exe 进程中获取当前登录系统用户名的密码， lsass是微软Windows系统的安全机制它主要用于本地安全和登陆策略，通常我们在登陆系统时输入密码之后，密码便会储存在 lsass内存中，经过其 wdigest 和 tspkg 两个模块调用后，对其使用可逆的算法进行加密并存储在内存之中， 而 mimikatz 正是通过对lsass逆算获取到明文密码！也就是说只要你不重启电脑，就可以通过他获取到登陆密码，只限当前登陆系统！

使用mimikatz获得该文件中administrator的密码，得到 `W3lc0meToD0g3` 
mimikatz下载地址： [https://github.com/gentilkiwi/mimikatz/](https://github.com/gentilkiwi/mimikatz/) 

```bash
将lsass.dmp文件放到mimikatz.exe下目录
privilege::debug
sekurlsa::minidump lsass.dmp
sekurlsa::logonpasswords full
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192358227.png)

3、使用密码解压压缩包，得到flag.txt文件。（flag在文件底部，向下翻翻）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192400016.png)

### flag：

```bash
D0g3{3466b11de8894198af3636c5bd1efce2}
flag{3466b11de8894198af3636c5bd1efce2}
```