---
title: "BUUCTF 蜘蛛侠呀 1"
date: 2025-01-27 09:17:18
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "数据库"
- "java"
- "开发语言"
- "CTF"
- "BUUCTF"
- "misc"
- "网络安全"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193338867.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[Hello CTF](https://hello-ctf.com/HC_Start/) 
[NewStar CTF](https://ns.openctf.net/learn/) 
[buuctf-蜘蛛侠呀](https://blog.csdn.net/amber_o0k/article/details/124262757) 
[BUUCTF：蜘蛛侠呀](https://blog.csdn.net/mochu7777777/article/details/109645038) 
[MISC（时间隐写）蜘蛛侠呀](https://guokeya.github.io/post/Y7dlMvs3K/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193340873.png)

### 题目描述：

将你获得的明显信息md5加密之后以flag{xxx}的格式提交。 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到out.pcap

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193342598.png)

---

### 解题思路：

1、打开out.pcap，发现大量ICMP报文，携带以 `$$START$$` 开头的数据，似乎是Base64数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193343645.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193345675.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193346948.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193348529.png)

使用Python脚本，提取数据。

```python
import pyshark
import binascii

# 打开PCAP文件并设置过滤器
packets = pyshark.FileCapture('out.pcap', display_filter="icmp.type==0")

res = []

# 处理每个数据包
for each in packets:
    try:
        # 解码ICMP数据负载
        data = binascii.unhexlify(each.icmp.data).decode()
        
        # 去除头部的 $$START$$
        if data.startswith('$$START$$'):
            data = data[len('$$START$$'):]
        
        # 去除重复项
        if data not in res:
            res.append(data)
    except Exception as e:
        print(f"Error processing packet: {e}")

# 将结果写入文件
with open('out.txt', 'w') as f:
    f.write(''.join(res))

# 关闭数据包读取器
packets.close()

print('done')
```

得到的数据，删除头部和尾部的标识，复制到在线网站进行Base64解码。

[Base64 在线解码、编码](https://the-x.cn/encodings/Base64.aspx) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193353373.png)

解码后的数据提示是一个zip文件，另存为zip。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193355718.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193358417.png)

2、解压zip文件，得到flag.gif

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193400043.gif)

观察该GIF，发现播放非常缓慢，猜测帧间隔存在隐写数据。这似乎是 `时间隐写` 。

使用Kali下的identify工具提取数据，命令如下：

```shell
identify -format '%T' flag.gif
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193402664.png)

```bash
2050502050502050205020202050202020205050205020502050205050505050202050502020205020505050205020206666
```

得到的数据由 `20` 和 `50` 组成，猜想二进制。将 `20` 替换为 `0` ， `50` 替换为 `1` ，去掉尾部的 `6666` 。

```bash
011011010100010000110101010111110011000101110100
```

将二进制数据转换为ASCII，得到

[2进制到ASCII字符串在线转换工具](https://coding.tools/cn/binary-to-text) 

```bash
mD5_1t
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193404437.png)

将 `mD5_1t` 进行md5加密，得到flag： `f0f1003afe4ae8ce4aa8e8487a8ab3b6` 。

[MD5加解密工具](https://www.sojson.com/md5/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193406125.png)

---

在Kali中使用tshark也可以提取数据，只需要将十六进制数据转换为字符即可。

```shell
tshark -r out.pcap -T fields -e data > data.txt
```

Python脚本如下：

```python
import binascii

with open('data1.txt','r') as file:
    with open('data2.txt','wb') as data:
        for i in file.readlines():
            data.write(binascii.unhexlify(i[:-1]))
```

### flag：

```bash
flag{f0f1003afe4ae8ce4aa8e8487a8ab3b6}
```

$$
\min (\text{Difficulty}_{\text{User}}) = \min \left( \sum_{i=1}^{n} (\text{Query}_i - \text{Answer}_i)^2 \right)
$$

