---
title: "BUUCTF sqltest 1"
date: 2024-03-21 20:47:49
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

![文章头图：蓝发少女插画，手写「刷题嘛~？」「一起听~」](/images/blog/20251228190241254.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


 ![题目页「sqltest 1」：网站遭攻击，附网盘链接与提取码 34uu](/images/blog/20251228190243292.png)

### 题目描述：

网站遭受到攻击了，还好我们获取到了全部网络流量。 链接: [https://pan.baidu.com/s/1AdQXVGKb6rkzqMLkSnGGBQ](https://pan.baidu.com/s/1AdQXVGKb6rkzqMLkSnGGBQ) 提取码: 34uu 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，得到一个.pcapng文件，双击在Wireshark中打开，内容如下。

 ![Wireshark 里 No.4 是 GET /index.php?act=news&id=1%20and%20length((select%20count(*)%20from…](/images/blog/20251228190244996.png)

---

### 解题思路：

1、根据题目提示，认为网络攻击类型为SQL注入，过滤出http的流量。

 ![过滤 http 后是一长串注入请求，id 参数里全是 length((select count(*) from…](/images/blog/20251228190247214.png)

随便看一条流量的追踪流，确认为SQL注入。

 ![跟踪 TCP 流能看到完整盲注语句：ascii(substr((select concat_ws(char(94), flag) from db_flag.tb_flag limit 0,1)))>50](/images/blog/20251228190249265.png)

按如下步骤，将所有HTTP请求导出为CSV文件。

 ![Wireshark 菜单：文件 → 导出分组解析结果 → As CSV...](/images/blog/20251228190251526.png)

2、查看该CSV文件，可以更直观得看到SQL注入攻击。在文件的末尾，发现已经爆破出数据库名、表名、字段名，并且开始使用ascii码来判断值。我们可以从中推断出正确的ascii值，在对一个字符进行bool判断时，被重复判断的ASCII值就是正确的字符，最后提取到：

```bash
102
108
97
103
123
52
55
101
100
98
56
51
48
48
101
100
53
102
57
98
50
56
102
99
53
52
98
48
100
48
57
101
99
100
101
102
55
125
```

![导出 CSV 后按响应排序，红框把 125、102、108、108、97 逐个叉出来](/images/blog/20251228190253477.png)

使用在线网站将ASCII码转换为字符串，得到flag。

 ![拼出 flag{47edb8300ed5f9b28fc54b0d09ecdef7}，下面是各进制对照](/images/blog/20251228190255802.png)

### flag：

```bash
flag{47edb8300ed5f9b28fc54b0d09ecdef7}
```