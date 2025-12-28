---
title: "Packet Tracer - Configuring Extended ACLs - Scenario 1（配置扩展访问控制列表 - 场景1）"
date: 2025-01-23 12:47:58
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "网络"
- "服务器"
- "cisco"
- "思科"
- "CCNA"
- "ACL"
---



## Packet Tracer - 配置扩展访问控制列表 - 场景1

### 地址表

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212710707.png)

### 目标

第一部分：配置、应用并验证一个编号扩展访问控制列表（Extended ACL）

第二部分：配置、应用并验证一个命名扩展访问控制列表（Extended Named ACL）

### 背景/场景

两位员工需要访问由服务器提供的服务。PC1只需要FTP访问权限，而PC2仅需Web访问权限。两台计算机都能ping通服务器，但彼此之间不能互相ping通。

---

### 第一部分：配置、应用并验证一个编号扩展访问控制列表

**步骤 1：配置ACL以允许FTP和ICMP流量** 

a. 在R1的全局配置模式下，输入以下命令确定扩展访问列表的第一个有效编号。

> R1(config)# access-list ?
> <1-99> IP标准访问列表
> <100-199> IP扩展访问列表

b. 向命令中添加数字100后跟问号。

> R1(config)# access-list 100 ?
> deny 拒绝指定的数据包
> permit 允许转发指定的数据包
> remark 访问列表条目注释

c. 为了允许FTP流量，在“permit”后面输入问号。

> R1(config)# access-list 100 permit ?
> ahp 认证报头协议
> eigrp 思科 EIGRP 路由协议
> esp 封装安全负载
> gre 思科 GRE 隧道
> icmp Internet 控制消息协议
> ip 任意 Internet 协议
> ospf OSPF 路由协议
> tcp 传输控制协议
> udp 用户数据报协议

d. 此ACL允许FTP和ICMP流量。虽然ICMP已列出，但FTP未列出，因为FTP使用TCP协议。因此，输入“tcp”进一步细化ACL帮助信息。

> R1(config)# access-list 100 permit tcp ?
> A.B.C.D 源地址
> any 任意源主机
> host 单个源主机

e. 注意可以通过使用“host”关键字仅过滤PC1的流量，或者允许任何主机。在本例中，允许任何属于172.22.34.64/27网络地址范围内的设备。输入网络地址后跟问号。

> R1(config)# access-list 100 permit tcp 172.22.34.64 ?
> A.B.C.D 源通配符位

f. 计算通配符掩码，通过计算子网掩码的二进制相反数。

255.255.255.224 = 11111111.11111111.11111111.11100000
0.0.0.31 = 00000000.00000000.00000000.00011111

g. 输入通配符掩码后跟问号。

> R1(config)# access-list 100 permit tcp 172.22.34.64 0.0.0.31 ?
> A.B.C.D 目的地址
> any 任意目的主机
> eq 仅匹配给定端口号上的数据包
> gt 仅匹配具有较大端口号的数据包
> host 单个目的主机
> lt 仅匹配具有较小端口号的数据包
> neq 仅匹配非给定端口号上的数据包
> range 仅匹配端口号范围内的数据包

h. 配置目标地址。在此场景中，我们正在为单个目标（即服务器）过滤流量。输入“host”关键字后跟服务器的IP地址。

> R1(config)# access-list 100 permit tcp 172.22.34.64 0.0.0.31 host 172.22.34.62 ?
> dscp 匹配具有给定 dscp 值的数据包
> eq 仅匹配给定端口号上的数据包
> established 已建立
> gt 仅匹配有更大端口号的数据包
> lt 仅匹配有更小端口号的数据包
> neq 仅匹配不具有给定端口号的数据包
> precedence 匹配具有给定优先级值的数据包
> range 仅匹配端口号范围内的数据包

i. 注意其中一个选项是（回车）。换句话说，您可以按Enter键，该语句将允许所有TCP流量。然而，我们只允许FTP流量；因此，输入“eq”关键字后跟问号以显示可用选项。然后输入“ftp”并按Enter。

> R1(config)# access-list 100 permit tcp 172.22.34.64 0.0.0.31 host 172.22.34.62 eq ftp

j. 创建第二个访问列表语句以允许从PC1到Server的ICMP（ping等）流量。注意，访问列表编号保持不变，并且不需要指定特定类型的ICMP流量。

> R1(config)# access-list 100 permit icmp 172.22.34.64 0.0.0.31 host 172.22.34.62
> <0-65535> 端口号
> ftp 文件传输协议 (21)
> pop3 邮局协议 v3 (110)
> smtp 简单邮件传输协议 (25)
> telnet Telnet (23)
> www 万维网（HTTP，80）

k. 默认情况下，所有其他流量都将被拒绝。

**步骤 2：在正确接口上应用ACL以过滤流量** 

从R1的角度看，ACL 100应用于与Gigabit Ethernet 0/0接口相连的网络中的入站流量。进入接口配置模式并应用ACL。

> R1(config)# interface gigabitEthernet 0/0
> R1(config-if)# ip access-group 100 in

**步骤 3：验证ACL实现** 

a. 从PC1 ping Server。如果无法成功ping通，请先验证IP地址是否正确。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212712649.png)

b. 从PC1 FTP至Server。用户名和密码均为cisco。

> PC> ftp 172.22.34.62

c. 退出Server上的FTP服务。

> ftp> quit

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212715058.png)

d. 从PC1 ping PC2。由于未明确允许此流量，目标主机应无法到达。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212716887.png)

### 第二部分：配置、应用并验证一个命名扩展访问控制列表

**步骤 1：配置ACL以允许HTTP访问和ICMP** 

a. 命名ACL以“ip”关键字开始。在R1的全局配置模式下，输入以下命令后跟问号。

> R1(config)# ip access-list ?
> extended 扩展访问列表
> standard 标准访问列表

b. 您可以配置命名的标准和扩展ACL。由于此访问列表需要过滤源和目标IP地址，因此必须是扩展类型。将名称设为 **HTTP_ONLY** （请注意，在Packet Tracer中评分时，名称区分大小写）。

> R1(config)# ip access-list extended HTTP_ONLY

c. 提示符会改变。现在您处于扩展命名ACL配置模式。PC2 LAN上的所有设备都需要TCP访问权限。输入网络地址后跟问号。

> R1(config-ext-nacl)# permit tcp 172.22.34.96 ?
> A.B.C.D 源通配符位

d. 另一种计算通配符的方法是从255.255.255.255减去子网掩码：

| 255.255.255.255 – 255.255.255.240 |
|:---:|
| = 0. 0. 0. 15 |

> R1(config-ext-nacl)# permit tcp 172.22.34.96 0.0.0.15 ?

e. 完成语句，指定服务器地址，并筛选www流量，如同第一部分操作一样。

> R1(config-ext-nacl)# permit tcp 172.22.34.96 0.0.0.15 host 172.22.34.62 eq www

f. 创建第二个访问列表语句，允许从PC2到Server的ICMP（ping等）流量。注意：提示符保持不变，此处无需指定特定类型的ICMP流量。

> R1(config-ext-nacl)# permit icmp 172.22.34.96 0.0.0.15 host 172.22.34.62

g. 默认情况下，所有其他流量都将被拒绝。退出扩展命名ACL配置模式。

**步骤 2：在正确接口上应用ACL以过滤流量** 

从R1的角度看，访问列表HTTP_ONLY应用于与Gigabit Ethernet 0/1接口相连的网络中的入站流量。进入接口配置模式并应用ACL。

> R1(config)# interface gigabitEthernet 0/1
> R1(config-if)# ip access-group HTTP_ONLY in

**步骤 3：验证ACL实现** 

a. 从PC2 ping Server。如果ping成功，则继续进行下一步；如果不成功，请先验证IP地址是否正确。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212718693.png)

b. 从PC2通过FTP连接到Server。连接应该失败。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212720616.png)

c. 在PC2上打开网页浏览器，将Server的IP地址作为URL输入。连接应该成功建立。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212722197.png)

