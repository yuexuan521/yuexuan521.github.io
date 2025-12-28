---
title: "Packet Tracer - Configuring ASA Basic Settings and Firewall Using CLI（使用CLI配置ASA基本设置和防火墙）"
date: 2025-01-23 12:46:18
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "网络"
- "服务器"
- "运维"
- "笔记"
- "智能路由器"
- "网络安全"
- "计算机网络"
---



## Packet Tracer - 使用CLI配置ASA基本设置和防火墙

### IP地址表

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212646524.png)

### 目标

- 验证连接并探索ASA设备

- 使用CLI配置ASA的基本设置和接口安全级别

- 使用CLI配置路由、地址转换和检查策略

- 配置DHCP、AAA和SSH服务

- 配置DMZ区域、静态NAT和访问控制列表（ACL）

### 场景

您的公司有一个地点通过ISP进行互联网接入。R1代表由ISP管理的CPE设备。R2代表一个互联网路由器中继节点。R3代表一个ISP，它连接着一家网络管理公司的管理员，该管理员受雇远程管理您的网络。ASA是一个边缘CPE安全设备，将内部企业网络和DMZ区域连接到ISP，并为内部主机提供NAT和DHCP服务。ASA将被配置以允许内部网络的管理员以及远程管理员对其进行管理。三层VLAN接口提供了对活动中创建的三个区域——Inside区域、Outside区域和DMZ区域的访问权限。ISP分配了公共IP地址空间209.165.200.224/29，将在ASA上用于地址转换。

所有路由器和交换机设备已预先配置以下信息：

- 启用密码： **ciscoenpa55**

- 控制台密码： **ciscoconpa55**

- 管理员用户名及密码： **admin/adminpa55**

注意：此Packet Tracer活动并不能替代ASA实验室练习。这个活动提供了额外的实践机会，模拟了大部分ASA 5505设备的配置过程。与真实的ASA 5505相比，在命令输出或部分尚未在Packet Tracer中支持的命令上可能存在细微差别。

### 第一部分：验证连接和探索ASA设备

注：此Packet Tracer活动开始时，有20%的评估项已被标记为已完成。这是为了确保您不会意外更改ASA的某些默认值。例如，默认情况下内部接口名称为“inside”，不应更改。点击“检查结果”查看哪些评估项已经被正确评分。

**步骤1：验证网络连接性。** 

目前ASA尚未配置，但所有路由器、PC以及DMZ服务器都已配置完毕。请确认PC-C可以ping通任何路由器接口。请注意，此时PC-C无法ping通ASA、PC-B或DMZ服务器。

**步骤2：确定ASA版本、接口及许可证信息。** 

使用 `show version` 命令来了解ASA设备的各种特性。

**步骤3：确定文件系统及其闪存内存内容。** 

a. 进入特权EXEC模式。当前未设置密码，当提示输入密码时直接按回车键。

b. 使用 `show file system` 命令显示ASA的文件系统，并确定支持哪些前缀。

c. 使用 `show flash:` 或 `show disk0:` 命令来显示闪存内存的内容。

### 第二部分：使用CLI配置ASA设置和接口安全

提示：许多ASA CLI命令与Cisco IOS CLI中的命令相似，甚至相同。此外，在不同配置模式及子模式之间切换的过程本质上是相同的。

**步骤1：配置主机名和域名。** 

a. 配置ASA主机名为 **CCNAS-ASA** 。

b. 配置域名为 **ccnasecurity.com** 。

```
ciscoasa(config)#hostname CCNAS-ASA
CCNAS-ASA(config)#domain-name ccnasecurity.com
```

**步骤2：配置启用模式密码。** 

使用 `enable password` 命令将特权EXEC模式密码更改为 **ciscoenpa55** 。

```
CCNAS-ASA(config)#enable password ciscoenpa55
```

**步骤3：设置日期和时间。** 

使用 `clock set` 命令手动设置日期和时间（此步骤不计入评分）。

```
CCNAS-ASA(config)#clock set 21:42:25 May 11 2023
```

**步骤4：配置内部和外部接口。** 

此时您只需配置VLAN 1（内部）和VLAN 2（外部）接口。VLAN 3（dmz）接口将在活动的第五部分进行配置。

a. 为内部网络（192.168.1.0/24）配置逻辑VLAN 1接口，并将其安全级别设置为最高值 **100** 。

```
CCNAS-ASA(config)# interface vlan 1
CCNAS-ASA(config-if)# nameif inside
CCNAS-ASA(config-if)# ip address 192.168.1.1 255.255.255.0
CCNAS-ASA(config-if)# security-level 100
```

b. 为外部网络（209.165.200.224/29）创建逻辑VLAN 2接口，将其安全级别设置为最低值 **0** ，并启用VLAN 2接口。

```
CCNAS-ASA(config-if)# interface vlan 2
CCNAS-ASA(config-if)# nameif outside
CCNAS-ASA(config-if)# ip address 209.165.200.226 255.255.255.248
CCNAS-ASA(config-if)# security-level 0
```

c. 使用以下验证命令检查您的配置：

1. 使用 `show interface ip brief` 命令显示所有ASA接口的状态。注意：这个命令与IOS命令show ip interface brief不同。如果之前配置的任何物理或逻辑接口状态不是up/up，请根据需要排查问题后再继续。

提示：大多数ASA show命令，包括ping、copy等，无需do命令即可在任意配置模式提示符下执行。

1. 使用 `show ip address` 命令显示三层VLAN接口的信息。

2. 使用 `show switch vlan` 命令显示ASA上配置的内部和外部VLAN以及分配的端口。

**步骤5：测试到ASA的连接性。** 

a. 应该可以从PC-B成功ping通ASA内部接口地址（192.168.1.1）。如果无法ping通，请按需排查配置问题。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212649034.png)

b. 从PC-B尝试ping VLAN 2（外部）接口的IP地址209.165.200.226。理论上您不应该能ping通这个地址。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212651625.png)

### 第三部分：使用CLI配置路由、地址转换和检查策略

**步骤1：为ASA配置静态默认路由。** 

在ASA外部接口上配置默认静态路由，以便ASA能够访问外部网络。

a. 使用 `route` 命令创建一个“全零”默认路由，将其与ASA外部接口关联，并将R1 G0/0 IP地址（209.165.200.225）设置为最后手段网关。

```
CCNAS-ASA(config)# route outside 0.0.0.0 0.0.0.0 209.165.200.225
```

b. 发出 `show route` 命令以验证静态默认路由是否存在于ASA路由表中。

c. 验证ASA能否ping通R1 S0/0/0 IP地址10.1.1.1。如果无法ping通，请按需排查问题。

**步骤2：使用PAT和网络对象配置地址转换。** 

a. 创建名为 **inside-net** 的网络对象，并使用subnet和nat命令为其分配属性。

```
CCNAS-ASA(config)# object network inside-net
CCNAS-ASA(config-network-object)# subnet 192.168.1.0 255.255.255.0
CCNAS-ASA(config-network-object)# nat (inside,outside) dynamic interface
CCNAS-ASA(config-network-object)# end
```

b. ASA将配置拆分为定义要转换的网络的对象部分以及实际的nat命令参数。这些内容会在运行配置中的两个不同位置显示。使用 `show run` 命令显示NAT对象配置。

c. 从PC-B尝试ping R1 G0/0接口IP地址209.165.200.225。这些ping请求应失败。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212653563.png)

d. 在ASA上发出 `show nat` 命令查看已翻译和未翻译的命中次数。请注意，来自PC-B的ping请求中有四个被翻译，四个未被翻译。外出的ping（echo请求）已被翻译并发送至目标。返回的echo响应由于防火墙策略而被阻止。您将在本部分活动的第3步配置默认检查策略以允许ICMP流量。

**步骤3：修改默认MPF应用检查全局服务策略。** 

为了实现应用层检查和其他高级选项，Cisco ASA设备提供了MPF功能。

Packet Tracer ASA设备默认没有MPF策略映射。作为修改，我们可以创建一个默认策略映射，用于对内部到外部的流量进行检查。正确配置后，只有由内部发起的流量才被允许回传到外部接口。您需要将ICMP添加到检查列表中。

a. 使用以下命令创建类图、策略映射和服务策略，并在策略映射列表中添加ICMP流量的检查：

```
CCNAS-ASA(config)# class-map inspection_default
CCNAS-ASA(config-cmap)# match default-inspection-traffic
CCNAS-ASA(config-cmap)# exit
CCNAS-ASA(config)# policy-map global_policy
CCNAS-ASA(config-pmap)# class inspection_default
CCNAS-ASA(config-pmap-c)# inspect icmp
CCNAS-ASA(config-pmap-c)# exit
CCNAS-ASA(config)# service-policy global_policy global
```

b. 从PC-B再次尝试ping R1 G0/0接口IP地址209.165.200.225。这次ping应该成功，因为现在ICMP流量正在被检查，合法的返回流量被允许通过。若ping失败，请排查您的配置。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212655546.png)

### 第四部分：配置DHCP、AAA和SSH

**步骤1：配置ASA作为DHCP服务器。** 

a. 在ASA内部接口上配置DHCP地址池并启用它。

```
CCNAS-ASA(config)# dhcpd address 192.168.1.5-192.168.1.36 inside
```

b. （可选）指定给客户端提供的DNS服务器IP地址。

```
CCNAS-ASA(config)# dhcpd dns 209.165.201.2 interface inside
```

c. 在ASA内启用DHCP守护进程，使其监听内部接口上的DHCP客户端请求。

```
CCNAS-ASA(config)# dhcpd enable inside
```

d. 将PC-B从静态IP地址更改为DHCP客户端，并验证其是否接收到IP地址信息。如有必要，请解决任何问题。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212657871.png)

**步骤2：配置AAA以使用本地数据库进行身份验证。** 

a. 使用username命令定义一个名为admin的本地用户，并指定密码adminpa55。

```
CCNAS-ASA(config)# username admin password adminpa55
```

b. 配置AAA以使用本地ASA数据库进行SSH用户身份验证。

```
CCNAS-ASA(config)# aaa authentication ssh console LOCAL
```

**步骤3：配置远程访问ASA。** 

ASA可以配置为接受来自内部或外部网络的单个主机或范围内的主机连接。在此步骤中，外部网络的主机只能通过SSH与ASA通信。SSH会话可用于从内部网络访问ASA。

a. 生成RSA密钥对，这是支持SSH连接所必需的。由于ASA设备已经有RSA密钥存在，当提示替换它们时请输入no。

```
CCNAS-ASA(config)# crypto key generate rsa modulus 1024
```

b. 配置ASA以允许来自内部网络（192.168.1.0/24）和外部网络分支办公室远程管理主机（172.16.3.3）的任何主机通过SSH进行连接。设置SSH超时时间为10分钟（默认为5分钟）。

```
CCNAS-ASA(config)# ssh 192.168.1.0 255.255.255.0 inside
CCNAS-ASA(config)# ssh 172.16.3.3 255.255.255.255 outside
CCNAS-ASA(config)# ssh timeout 10
```

c. 从PC-C通过SSH建立到ASA（209.165.200.226）的会话。如不成功，请排查问题。

```
PC> ssh -l admin 209.165.200.226
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212700132.png)

d. 从PC-B通过SSH建立到ASA（192.168.1.1）的会话。如不成功，请排查问题。

```
PC> ssh -l admin 192.168.1.1
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212701975.png)

### 第五部分：配置DMZ、静态NAT和ACL

R1 G0/0接口与ASA的外部接口分别使用209.165.200.225和.226。您将使用公网地址209.165.200.227，并通过静态NAT提供对服务器的地址转换访问。

**步骤1：在ASA上配置DMZ接口VLAN 3。** 

a. 配置DMZ VLAN 3，该VLAN将是公共访问Web服务器所在的位置。为它分配IP地址192.168.2.1/24，并命名为 **dmz** ，同时为其设置安全级别为 **70** 。由于服务器无需主动与内部用户通信，因此禁用到接口VLAN 1的转发。

```shell
CCNAS-ASA(config)# interface vlan 3
CCNAS-ASA(config-if)# ip address 192.168.2.1 255.255.255.0
CCNAS-ASA(config-if)# no forward interface vlan 1
CCNAS-ASA(config-if)# nameif dmz
INFO: Security level for "dmz" set to 0 by default.
CCNAS-ASA(config-if)# security-level 70
```

b. 将ASA物理接口E0/2分配给DMZ VLAN 3并启用此接口。

```shell
CCNAS-ASA(config-if)# interface Ethernet0/2
CCNAS-ASA(config-if)# switchport access vlan 3
```

c. 使用以下验证命令检查您的配置：

1. 使用 `show interface ip brief` 命令显示所有ASA接口的状态。
    ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212704275.png)

2. 使用 `show ip address` 命令显示第3层VLAN接口的信息。
    ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212706400.png)

3. 使用 `show switch vlan` 命令显示ASA上的inside和outside VLAN配置以及分配的端口信息。
    ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212708670.png)

**步骤2：使用网络对象配置到DMZ服务器的静态NAT。** 

配置一个名为dmz-server的网络对象，并将其分配给DMZ服务器的静态IP地址（192.168.2.3）。在定义对象模式下，使用nat命令指定此对象用于使用静态NAT将DMZ地址翻译为外部地址，并指定公开翻译地址209.165.200.227。

```
CCNAS-ASA(config)# object network dmz-server
CCNAS-ASA(config-network-object)# host 192.168.2.3
CCNAS-ASA(config-network-object)# nat (dmz,outside) static 209.165.200.227
CCNAS-ASA(config-network-object)# exit
```

**步骤3：配置ACL以允许从互联网访问DMZ服务器。** 

配置一个名为OUTSIDE-DMZ的命名访问列表，允许来自任何外部主机到DMZ服务器内部IP地址的TCP协议在端口80上进行通信。将访问列表应用到ASA的外部接口的“IN”方向。

```
CCNAS-ASA(config)# access-list OUTSIDE-DMZ permit tcp any host 192.168.2.3 eq 80
CCNAS-ASA(config)# access-group OUTSIDE-DMZ in interface outside
```

注：与IOS ACL不同，ASA ACL的permit语句必须允许对内部私有DMZ地址的访问。外部主机通过服务器的公共静态NAT地址访问服务器，ASA将其翻译成内部主机IP地址，然后应用ACL。

**步骤4：测试对DMZ服务器的访问。** 

在创建Packet Tracer活动时，成功测试外部对DMZ Web服务器的访问功能并未实现；因此，不强制要求成功测试。

**步骤5：检查结果。** 

完成百分比应为100%。点击“Check Results”查看反馈和已完成所需组件的验证。

### 实验脚本：

**第一部分：验证连接和探索ASA设备** 

```bash
hostname CCNAS-ASA
domain-name ccnasecurity.com
enable password ciscoenpa55
clock set 10:38:00 22 dec 2020
```

**第二部分：使用CLI配置ASA设置和接口安全** 

```bash
interface vlan 1
nameif inside
ip address 192.168.1.1 255.255.255.0
security-level 100

interface vlan 2
nameif outside
ip address 209.165.200.226 255.255.255.248
security-level 0
interface Ethernet0/0
switchport access vlan 2

interface vlan 3
ip address 192.168.2.1 255.255.255.0
no forward interface vlan 1
nameif dmz
security-level 70
interface Ethernet0/2
switchport access vlan 3
```

**第三部分：使用CLI配置路由、地址转换和检查策略** 

```bash
route outside 0.0.0.0 0.0.0.0 209.165.200.225
class-map inspection_default
match default-inspection-traffic
exit
policy-map global_policy
class inspection_default
inspect icmp
exit
service-policy global_policy global
```

**第四部分：配置DHCP、AAA和SSH** 

```bash
dhcpd address 192.168.1.5-192.168.1.36 inside
dhcpd dns 209.165.201.2 interface inside
dhcpd enable inside

username admin password adminpa55
crypto key generate rsa modulus 1024 #no

aaa authentication ssh console LOCAL
ssh 192.168.1.0 255.255.255.0 inside
ssh 172.16.3.3 255.255.255.255 outside
ssh timeout 10
```

**第五部分：配置DMZ、静态NAT和ACL** 

```bash
object network dmz-server
host 192.168.2.3
nat (dmz,outside) static 209.165.200.227

object network inside-net
subnet 192.168.1.0 255.255.255.0
nat (inside,outside) dynamic interface

access-list OUTSIDE-DMZ permit icmp any host 192.168.2.3
access-list OUTSIDE-DMZ permit tcp any host 192.168.2.3 eq 80
access-group OUTSIDE-DMZ in interface outside
```