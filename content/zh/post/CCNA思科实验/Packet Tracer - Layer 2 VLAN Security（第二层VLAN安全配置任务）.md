---
title: "Packet Tracer - Layer 2 VLAN Security（第二层VLAN安全配置任务）"
date: 2025-01-23 12:47:44
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "网络"
- "智能路由器"
- "服务器"
- "运维"
- "笔记"
- "网络安全"
- "CCNA"
---



## PacketTracer - 第二层VLAN安全配置任务

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212730148.png)

### 目标

- 在SW-1和SW-2之间建立新的冗余链路。

- 在新连接的SW-1和SW-2之间的干线链路上启用中继并配置安全措施。

- 创建一个新的管理VLAN（VLAN 20）并将一台管理PC连接到该VLAN。

- 实施ACL以防止外部用户访问管理VLAN。

### 背景/场景

一家公司的网络当前使用两个独立的VLAN：VLAN 5和VLAN 10。此外，所有干线端口都已配置为本征VLAN 15。网络管理员希望在交换机SW-1和SW-2之间添加一条冗余链路。这条链路必须启用中继功能，并确保所有必要的安全设置到位。

此外，网络管理员还希望将一台管理PC连接到交换机SW-A。管理员希望这台管理PC能够连接到所有交换机及路由器，但不希望任何其他设备能够连接到管理PC或这些交换机上。因此，管理员计划创建一个新的VLAN 20用于管理目的。

所有设备已经预先配置了以下信息：

- 启用密码： **ciscoenpa55**

- 控制台密码： **ciscoconpa55**

- SSH用户名及密码： **SSHadmin / ciscosshpa55**

### 第一部分：验证连通性

**步骤1：验证C2（VLAN 10）与C3（VLAN 10）之间的连通性。** 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212732555.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212734673.png)

**步骤2：验证C2（VLAN 10）与D1（VLAN 5）之间的连通性。** 
注：如果使用简易PDU GUI包，请确保ping两次以允许ARP过程完成。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212736073.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212738694.png)

### 第二部分：在SW-1和SW-2之间创建冗余链路

**步骤1：连接SW-1和SW-2。** 

使用交叉线缆将SW-1的F0/23端口与SW-2的F0/23端口相连。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212740831.png)

**步骤2：在SW-1和SW-2之间的链路上启用干线功能，包括所有干线安全机制。** 

已预先配置了所有现存干线接口的干线功能。新链接必须设置为干线，并包括所有干线安全机制。在SW-1和SW-2上，将端口设置为干线模式，将本征VLAN 15分配给干线端口，并禁用自动协商功能。

> SW-1(config)#interface f0/23
> SW-1(config-if)#switchport mode trunk
> SW-1(config-if)#switchport trunk native vlan 15
> SW-1(config-if)#switchport nonegotiate
> SW-1(config-if)#no shutdown

> SW-2(config)#interface f0/23
> SW-2(config-if)#switchport mode trunk
> SW-2(config-if)#switchport trunk native vlan 15
> SW-2(config-if)#switchport nonegotiate
> SW-2(config-if)#no shutdown

### 第三部分：启用VLAN 20作为管理VLAN

网络管理员希望通过管理PC访问所有交换机和路由设备。出于安全原因，管理员希望确保所有受管设备都在一个独立的VLAN中。

**步骤1：在SW-A上启用管理VLAN（VLAN 20）。** 

a. 在SW-A上启用VLAN 20。

> SW-A(config)#vlan 20
> SW-A(config-vlan)#exit

b. 创建VLAN 20接口并在192.168.20.0/24网络内分配一个IP地址。

> SW-A(config)#interface vlan 20
> SW-A(config-if)#ip address 192.168.20.1 255.255.255.0

**步骤2：在所有其他交换机上启用相同的管理VLAN。** 

a. 在SW-B、SW-1、SW-2和中央交换机上创建管理VLAN。

> Central(config)#vlan 20
> Central(config-vlan)#exit

> SW-1(config)#vlan 20
> SW-1(config-vlan)#exit

> SW-2(config)#vlan 20
> SW-2(config-vlan)#exit

> SW-B(config)#vlan 20
> SW-B(config-vlan)#exit

b. 在所有交换机上创建VLAN 20接口，并在192.168.20.0/24网络内分配一个IP地址。

> Central(config)#int vlan 20
> Central(config-if)#ip address 192.168.20.2 255.255.255.0

> SW-1(config)#int vlan 20
> SW-1(config-if)#ip address 192.168.20.3 255.255.255.0

> SW-2(config)#int vlan 20
> SW-2(config-if)#ip address 192.168.20.4 255.255.255.0

> SW-B(config)#int vlan 20
> SW-B(config-if)#ip address 192.168.20.5 255.255.255.0

**步骤3：连接并配置管理PC。** 

将管理PC连接到SW-A的F0/1端口，并确保为其分配192.168.20.0/24网络内的可用IP地址。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212742442.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212744001.png)

**步骤4：在SW-A上确保管理PC属于VLAN 20。** 

接口F0/1必须是VLAN 20的一部分。

> SW-A(config)#int f0/1
> SW-A(config-if)#switchport access vlan 20
> SW-A(config-if)#no shutdown

**步骤5：验证管理PC与所有交换机之间的连通性** 。

管理PC应能成功ping通SW-A、SW-B、SW-1、SW-2和中央交换机。

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212745938.png)

 ![请添加图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212748325.png)

### 第四部分：使管理PC能够访问路由器R1

**步骤1：在路由器R1上启用新的子接口。** 

a. 创建子接口 **g0/0.3** ，并设置封装类型为 **dot1q 20** ，以便支持VLAN 20。

> R1(config)#int g0/0.3
> R1(config-subif)#encapsulation dot1Q 20

b. 分配192.168.20.0/24网络内的IP地址。

> R1(config)#int g0/0.3
> R1(config-subif)#ip address 192.168.20.100 255.255.255.0

步骤2：验证管理PC与R1之间的连通性。

务必在管理PC上配置默认网关以实现连通性。

**步骤3：启用安全性。** 

虽然管理PC必须能够访问路由器，但其他任何PC都不应能够访问管理VLAN。

a. 创建只允许管理PC访问路由器的ACL。

> R1(config)#access-list 101 deny ip any 192.168.20.0 0.0.0.255
> R1(config)#access-list 101 permit ip any any
> R1(config)#access-list 102 permit ip host 192.168.20.6 any

b. 将ACL应用到适当的接口上。

> R1(config)#int g0/0.1
> R1(config-subif)#ip access-group 101 in
> R1(config-subif)#int g0/0.2
> R1(config-subif)#ip access-group 101 in

> R1(config)#line vty 0 4
> R1(config-line)#access-class 102 in

注：可以有多种方式创建ACL来满足必要的安全要求。因此，该活动这一部分的评分基于正确的连通性需求。管理PC必须能够连接到所有交换机和路由器，而所有其他PC则不能连接到管理VLAN内的任何设备。

**步骤4：验证安全性。** 

a. 验证只有管理PC可以访问路由器。使用SSH从管理PC通过用户名SSHadmin和密码ciscosshpa55登录R1。

> PC> ssh -l SSHadmin 192.168.20.100

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212750446.png)

b. 从管理PC尝试ping SW-A、SW-B和R1，是否成功？请解释结果。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212751788.png)

> VLAN20 中的设备不需要通过路由器进行路由，不受ACL的影响。

c. 从D1尝试ping管理PC，是否成功？请解释结果。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212754164.png)

> 不同 VLAN 中的设备 ping VLAN20 中的设备，必须进行路由，而路由器具有阻止所有数据包访问 192.168.20.0 目标网络的 ACL。

**步骤5：检查结果。** 

您的完成度应该为100%。点击“检查结果”查看反馈信息以及已完成的必要组件验证。

如果所有组件都看似正确，但活动仍显示未完成，则可能是由于验证ACL操作的连通性测试出现问题。

### 实验脚本：

**Part 2:** 

SW-1、SW-2

```powershell
连接SW-1、SW-2，使用交叉线路，要开端口
连接SW-A、PC，要开端口
interface f0/23
switchport mode trunk
switchport trunk native vlan 15
switchport nonegotiate
no shutdown
```

**Part 3：** 

SW-A：

```powershell
int f0/1
switchport access vlan 20
no shutdown
```

SW-1：

```powershell
int f0/23
switchport trunk native vlan 15
switchport mode trunk 
switchport nonegotiate
```

SW-2:

```powershell
int f0/23
switchport trunk native vlan 15
switchport mode trunk 
switchport nonegotiate
```

SW-A、B、1、2、Central：

```powershell
vlan 20
int vlan 20
ip address 192.168.20.XXX 255.255.255.0
```

**Part 4:** 

R1：

```powershell
int g0/0.3
encapsulation dot1Q 20
ip address 192.168.20.20 255.255.255.0
```

R1：

```powershell
access-list 101 deny ip any 192.168.20.0 255.255.255.0
access-list 101 permit ip any any 
access-list 102 permit ip host 192.168.20.6 any 

int g0/0.1
ip access-group 101 in 

int g0/0.2
ip access-group 101 in 

line vty 0 4
access-class 102 in
```

PC：

```powershell
ssh -l SSHadmin 192.168.20.20
```