---
title: "Packet Tracer - Layer 2 Security（第二层安全配置任务）"
date: 2025-01-23 12:48:13
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "服务器"
- "asp.net"
- "数据库"
- "运维"
- "思科"
- "Cisco"
- "CCNA"
---



## PacketTracer - 第二层安全配置任务

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212724150.png)

### 目标

- 确保将中心交换机（3560型号）设置为根桥。

- 保护生成树协议参数以防止对STP的操控攻击。

- 启用端口安全功能以防止CAM表溢出攻击。

### 背景/场景

最近网络遭受了一系列攻击。因此，网络管理员已指派您负责配置第二层安全措施。

为了确保网络性能和安全性达到最优状态，管理员希望确定中心3560型号交换机作为根桥。为防止对生成树协议进行篡改攻击，管理员希望确保STP参数得到安全配置。针对CAM表溢出攻击的风险，网络管理员决定配置端口安全策略，限制每个交换机端口学习到的MAC地址数量。一旦学习到的MAC地址超过设定的限制，管理员希望建立机制自动关闭该端口。

所有交换机设备已经预先配置了以下信息：

- 启用密码： **ciscoenpa55**

- 控制台密码： **ciscoconpa55**

- SSH用户名及密码： **SSHadmin / ciscosshpa55**

### 第一部分：配置根桥

**步骤1：确定当前的根桥。** 

从中心交换机（Central）发出 `show spanning-tree` 命令，以确定当前的根桥、查看正在使用的端口及其状态。

> Central#show spanning-tree

```powershell
VLAN0001
  Spanning tree enabled protocol ieee
  Root ID    Priority    32769
             Address     0009.7C61.9058
             Cost        4
             Port        25(GigabitEthernet0/1)
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec

  Bridge ID  Priority    32769  (priority 32768 sys-id-ext 1)
             Address     00D0.D31C.634C
             Hello Time  2 sec  Max Age 20 sec  Forward Delay 15 sec
             Aging Time  20

Interface        Role Sts Cost      Prio.Nbr Type
---------------- ---- --- --------- -------- --------------------------------
Gi0/2            Desg FWD 4         128.26   P2p
Gi0/1            Root FWD 4         128.25   P2p
Fa0/1            Desg FWD 19        128.1    P2p
```

哪个交换机是当前的根桥？

基于当前的根桥，请绘制由此得出的生成树拓扑结构。

**步骤2：将Central设置为主根桥。** 

使用命令 `spanning-tree vlan 1 root primary` ，将 **Central** 设置为根桥。

> Central(config)#spanning-tree vlan 1 root primary

**步骤3：将SW-1设置为备用根桥。** 

使用命令 `spanning-tree vlan 1 root secondary` ，将 **SW-1** 设置为备用根桥。

> SW-1(config)#spanning-tree vlan 1 root secondary

**步骤4：验证生成树配置。** 

发出 `show spanning-tree` 命令来验证Central已成为根桥。

在Central#提示符下执行了该命令后显示如下信息：

```powershell
VLAN0001
   Spanning tree enabled protocol ieee
   Root ID  Priority      24577
            Address       00D0.D31C.634C
          -->>  This bridge is the root  <<--
            Hello Time  2 sec  Max Age  20 sec   Forward Delay  15 sec
```

根据上述信息，哪个交换机是当前的根桥？

基于新的根桥设置，请绘制由此得出的生成树拓扑结构。

### 第二部分：防止STP攻击

**步骤1：在所有接入端口上启用PortFast。** 

PortFast应在连接至单个工作站或服务器的接入端口上配置，以使它们更快地进入活动状态。在SW-A和SW-B的相连接入端口上使用 `spanning-tree portfast` 命令来启用 **PortFast** 。

> SW-A(config)#int range f0/1-4
> SW-A(config-if-range)#spanning-tree portfast

> SW-B(config)#int range f0/1-4
> SW-B(config-if-range)#spanning-tree portfast

**步骤2：在所有接入端口上启用BPDU防护。** 

BPDU guard是一项功能，可以有助于防止恶意交换机和在接入端口上的欺骗行为。在SW-A和SW-B的接入端口上启用BPDU防护。

注解：为了防止STP报文（BPDU）操纵攻击，在接口配置模式下可以对每个单独端口使用命令 `spanning-tree bpduguard enable` 来启用BPDU防护；或者在全局配置模式下使用命令 `spanning-tree portfast bpduguard default` 来默认为所有启用PortFast的端口启用BPDU防护。针对本活动评分目的，请使用 `spanning-tree bpduguard enable` 命令。

> SW-A(config)#int range f0/1-4
> SW-A(config-if-range)#spanning-tree bpduguard enable

> SW-B(config)#int range f0/1-4
> SW-B(config-if-range)#spanning-tree bpduguard enable

**步骤3：启用根保护。** 

根保护可以在非根端口的所有交换机端口上启用，最好部署在连接到其他非根交换机的端口上。使用 `show spanning-tree` 命令确定每个交换机上根端口的位置。

在SW-1上，在端口F0/23和F0/24上启用根保护。同样，在SW-2上，在端口F0/23和F0/24上也启用根保护。

> SW-1(config)#int range f0/23-24
> SW-1(config-if-range)#spanning-tree guard root

> SW-2(config)#int range f0/23-24
> SW-2(config-if-range)#spanning-tree guard root

### 第三部分：配置端口安全并禁用未使用端口

**步骤1：在连接到主机设备的所有端口上配置基本端口安全。** 

此操作应在SW-A和SW-B的所有接入端口上执行。设置允许学习的MAC地址最大数量为 **2** ，允许动态学习MAC地址，并将违规处理方式设为 **shutdown** （关闭）。

注解：只有当交换机端口配置为接入模式时，才能启用端口安全功能。

> SW-A(config)#interface range f0/1 - 22
> SW-A(config-if-range)#switchport mode access
> SW-A(config-if-range)#switchport port-security
> SW-A(config-if-range)#switchport port-security maximum 2
> SW-A(config-if-range)#switchport port-security violation shutdown
> SW-A(config-if-range)#switchport port-security mac-address sticky

> SW-B(config)#interface range f0/1-22
> SW-B(config-if-range)#switchport mode access
> SW-B(config-if-range)#switchport port-security max
> SW-B(config-if-range)#switchport port-security maximum 2
> SW-B(config-if-range)#switchport port-security violation shutdown
> SW-B(config-if-range)#switchport port-security mac-address sticky

为什么与其它交换机设备相连的端口不启用端口安全？

**步骤2：验证端口安全配置。** 

a. 在SW-A上，输入命令 `show port-security interface f0/1` 来确认已成功配置了端口安全。

> SW-A#show port-security int f0/1
> <mark>Port Security : Enabled</mark> 
> Port Status : Secure-up
> <mark>Violation Mode : Shutdown</mark> 
> Aging Time : 0 mins
> Aging Type : Absolute
> SecureStatic Address Aging : Disabled
> <mark>Maximum MAC Addresses : 2</mark> 
> Total MAC Addresses : 0
> Configured MAC Addresses : 0
> <mark>Sticky MAC Addresses : 0</mark> 
> <mark>Last Source Address:Vlan : 0000.0000.0000:0</mark> 
> Security Violation Count : 0

```powershell
SW-A# show port-security interface f0/1
端口安全              : 已启用
端口状态                : 安全且已启动
违规模式             : 关闭端口
老化时间                 : 0分钟
老化类型                 : 绝对时间
静态安全MAC地址老化: 禁用
最大MAC地址数      : 2
总MAC地址数        : 0
已配置MAC地址数   : 0
粘性MAC地址数       : 0
最近源地址:VLAN   : 0000.0000.0000:0
安全违规计数         : 0

```

b. 从C1向C2发送Ping请求，然后再次输入 `show port-security interface f0/1` 命令，以验证交换机是否已学会C1的MAC地址。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212726209.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212728180.png)

**步骤3：禁用未使用的端口。** 

禁用当前所有未使用的端口。

> SW-A(config)#int range f0/5-22
> SW-A(config-if-range)#shutdown

> SW-B(config)#int range f0/5-22
> SW-B(config-if-range)#shutdown

**步骤4：检查结果。** 

您的完成度应为100%。点击“检查结果”查看反馈信息以及所需组件完成情况的验证。

### 实验脚本：

**Central:** 

```powershell
! 使Central成为Vlan1的根桥
spanning-tree vlan 1 root primary
```

**SW-1:** 

```powershell
! 使SW-1成为Vlan1的次根桥
spanning-tree vlan 1 root secondary
! 进入f0/23-f0/24端口
interface range fastEthernet 0/23 - fastEthernet 0/24
! 启用STP根防护功能，在此端口不接受拥有更优BID的BPDU报文
spanning-tree guard root 
```

**SW-2:** 

```powershell
! 进入f0/23-f0/24端口
interface range fastEthernet 0/23 - fastEthernet 0/24
! 启用STP根防护功能，在此端口不接受拥有更优BID的BPDU报文
spanning-tree guard root 
```

**SW-A:** 

```powershell
! 选择接入的端口，F0/1-F0/4
interface range fastEthernet 0/1 - fastEthernet 0/4
! 让F0/1-F0/4端口开启portfast（不参与生成树）
spanning-tree portfast 
! 为他们启用BPDU防护功能，在此端口不接受BPDU；收到BPDU，端口禁用
spanning-tree bpduguard enable 
! 开启access模式
switchport mode access
! 开启端口安全
switchport port-security 
! 设置最大Mac学习数为2
switchport port-security maximum 2
! 设置学习到的Mac地址将被保存
switchport port-security mac-address sticky 
! 设置超过措施：关闭端口
switchport port-security violation shutdown 
! 进入不使用的端口
interface range fastEthernet 0/5 - fastEthernet 0/22
! 关闭
shutdown
```

**SW-B:** 

```powershell
! 选择接入的端口，F0/1-F0/4
interface range fastEthernet 0/1 - fastEthernet 0/4
! 让F0/1-F0/4端口开启portfast（不参与生成树）
spanning-tree portfast 
! 为他们开启BPDU
spanning-tree bpduguard enable 
! 开启access模式
switchport mode access
! 开启端口安全
switchport port-security 
! 设置最大Mac学习数为2
switchport port-security maximum 2
! 设置学习到的Mac地址将被保存
switchport port-security mac-address sticky 
! 设置超过措施：关闭端口
switchport port-security violation shutdown 
! 进入不使用的端口
interface range fastEthernet 0/5 - fastEthernet 0/22
! 关闭
shutdown
```