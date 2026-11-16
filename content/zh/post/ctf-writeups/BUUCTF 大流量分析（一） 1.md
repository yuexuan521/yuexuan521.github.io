---
title: "BUUCTF 大流量分析（一） 1"
date: 2025-05-05 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "Wireshark"
- "网络安全"
- "MISC"
- "BUUCTF"
- "CTF"
- "流量分析"
---

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228192825456.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [BUUCTF:大流量分析（一）](https://blog.csdn.net/wangjin7356/article/details/122525900)


 ![题目页「大流量分析（一）1」：问黑客攻击 IP，附网盘链接与提取码 9jva](/images/blog/20251228192827137.png)

### 题目描述：

某黑客对A公司发动了攻击，以下是一段时间内我们获取到的流量包，那黑客的攻击ip是多少？（答案加上flag{}）附件链接: https://pan.baidu.com/s/1EgLI37y6m9btzwIWZYDL9g 提取码: 9jva 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，有很多pcap流量包

 ![解压目录里 10 个 pcap 文件，每个约 488 MB](/images/blog/20251228192828717.png)

---

### 解题思路：

1、随便打开一个，一般黑客的攻击流量会很多，需要使用Wireshark统计功能。

先统计IP，统计 → IPv4 Statistics → All Addresses

 ![统计 → IPv4 Statistics → All Addresses，红框 183.129.152.140，共 196601 个包](/images/blog/20251228192830566.png)

将IP出现数量Count进行排序，发现除了 `183.129.152.140` ，其它全部是内网IP。

> 常见内网IP段：
> 10.0.0.0 – 10.255.255.255
> 172.16.0.0 – 172.31.255.255
> 192.168.0.0 – 192.168.255.255

 ![统计菜单里红框标出「会话(C)」与「端点(E)」](/images/blog/20251228192833055.png)

2、统计会话和端点，发现 `183.129.152.140` 的分组数，是除内网IP最多的。

 ![统计 → Conversations → IPv4，红框 183.129.152.140 与 172.16.61.206、.199 的大量会话](/images/blog/20251228192835290.png)

会话

 ![同一会话窗口，A→B 与 B→A 的包数、字节数对比](/images/blog/20251228192837106.png)

端点

 ![端点窗口里红框同样是 183.129.152.140](/images/blog/20251228192839423.png)

最后，确认黑客的攻击ip是 `183.129.152.140` 。

### flag：

```bash
flag{183.129.152.140}
```