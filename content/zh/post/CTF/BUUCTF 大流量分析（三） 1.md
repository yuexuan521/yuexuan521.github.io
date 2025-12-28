---
title: "BUUCTF 大流量分析（三） 1"
date: 2025-05-12 09:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "Wireshark"
- "Misc"
- "BUUCTF"
- "CTF"
- "网络安全"
- "安全"
- "大流量分析"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192841294.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF | 大流量分析 （一）（二）（三）](https://www.cnblogs.com/yunqian2017/p/14298416.html) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192843830.png)

### 题目描述：

某黑客对A公司发动了攻击，以下是一段时间内我们获取到的流量包，那黑客预留的后门的文件名是什么？（答案加上flag{}）附件链接: https://pan.baidu.com/s/1EgLI37y6m9btzwIWZYDL9g 提取码: 9jva 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，有很多pcap流量包。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192846159.png)

---

### 解题思路：

1、寻找黑客留下的后门文件，对于php站点，通常成功上传木马后，会测试 `phpinfo()` 返回。

所以，在流量包中搜索 `phpinfo()` ，过滤语句如下：

```python
tcp contains "phpinfo()"
```

最终，在该流量包 `数据采集D_eth0_NS_20160809_172831.pcap` 发现 `phpinfo()` 。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192848528.png)

执行 `phpinfo()` 的文件是admin.bak.php，黑客预留的后门的文件名是 `admin.bak.php` 。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228192850193.png)

```python
GET /admin.bak.php?a=assert&b=phpinfo() HTTP/1.1\r\n
```

### flag：

```bash
flag{admin.bak.php}
```