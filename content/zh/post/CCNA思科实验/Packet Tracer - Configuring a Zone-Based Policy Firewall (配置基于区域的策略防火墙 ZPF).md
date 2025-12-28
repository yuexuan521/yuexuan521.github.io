---
title: "Packet Tracer - Configuring a Zone-Based Policy Firewall (配置基于区域的策略防火墙 ZPF)"
date: 2025-01-23 12:41:12
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "智能路由器"
- "网络"
- "服务器"
- "运维"
- "笔记"
- "网络安全"
- "安全架构"
---

## Packet Tracer - 配置基于区域的策略防火墙（ZPF）

### 地址表

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212629252.png)

### 目标

- 在防火墙配置前验证设备之间的连通性。

- 在路由器R3上配置基于区域的策略（ZPF）防火墙。

- 使用ping、Telnet和网页浏览器验证ZPF防火墙功能。

### 背景/场景

基于区域的策略（Zone-Based Policy，ZPF）防火墙是Cisco防火墙技术发展的最新成果。在本活动中，您将在边缘路由器R3上配置一个基本的ZPF防火墙，允许内部主机访问外部资源，并阻止外部主机访问内部资源。然后，从内部和外部主机验证防火墙的功能。

路由器已预先配置了以下内容：

- 控制台密码： **ciscoconpa55**

- vty线路密码： **ciscovtypa55**

- 启用密码： **ciscoenpa55**

- 主机名和IP地址配置

- 静态路由配置

### 第一部分：验证基本网络连通性

在配置基于区域的策略防火墙之前，验证网络连通性。

**步骤1：从PC-A命令提示符，ping PC-C的192.168.3.3地址。** 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212631463.png)

**步骤2：从PC-C命令提示符，通过telnet连接到Router R2 S0/0/1接口的10.2.2.2地址。退出Telnet会话。** 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212633272.png)

**步骤3：从PC-C打开一个网页浏览器访问PC-A服务器。** 

a. 点击桌面标签页并点击Web浏览器应用程序。将PC-A的IP地址 **192.168.1.3** 作为URL输入。此时应显示来自Web服务器的Packet Tracer欢迎页面。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212634919.png)

b. 关闭PC-C上的浏览器。

### 第二部分：在路由器R3上创建防火墙区域

注意：对于所有配置任务，请确保使用指定的确切名称。

**步骤1：创建内部区域。** 

使用 **zone security** 命令创建名为 **IN-ZONE** 的区域。

```
R3(config)# zone security IN-ZONE
```

**步骤2：创建外部区域。** 

使用 **zone security** 命令创建名为 **OUT-ZONE** 的区域，并退出区域安全配置模式。

```
R3(config)# zone security OUT-ZONE
R3(config-sec-zone)# exit
```

### 第三部分：定义流量类别和访问列表

**步骤1：创建定义内部流量的ACL。** 

使用 `access-list` 命令创建扩展ACL 101，允许来自192.168.3.0/24源网络的所有IP协议到任何目的地。

```
R3(config)# access-list 101 permit ip 192.168.3.0 0.0.0.255 any
```

**步骤2：创建引用内部流量ACL的类映射。** 

使用带有match-all选项的 `class-map type inspect` 命令创建名为 **IN-NET-CLASS-MAP** 的类映射。使用 `match access-group` 命令匹配ACL 101。

```
R3(config)# class-map type inspect match-all IN-NET-CLASS-MAP
R3(config-cmap)# match access-group 101
R3(config-cmap)# exit
```

注：虽然在本Packet Tracer练习中不支持，但可以通过match-any选项指定具体的协议（如HTTP、FTP等），以便对需要检查的流量类型提供更精确的控制。

### 第四部分：指定防火墙策略

**步骤1：创建策略映射以确定如何处理匹配的流量。** 

使用 `policy-map type inspect` 命令并创建一个名为IN-2-OUT-PMAP的策略映射。

```
R3(config)# policy-map type inspect IN-2-OUT-PMAP
```

**步骤2：指定inspect类型的类，并引用类映射IN-NET-CLASS-MAP。** 

```
R3(config-pmap)# class type inspect IN-NET-CLASS-MAP
```

**步骤3：为该策略映射指定inspect操作。** 

使用inspect命令会调用基于上下文的访问控制（其他选项包括pass和drop）。

```
R3(config-pmap-c)# inspect

%No specific protocol configured in class IN-NET-CLASS-MAP for inspection. All protocols will be inspected.
```

提示信息表示IN-NET-CLASS-MAP类没有配置特定协议进行检查，因此所有协议都将被检查。

连续两次发出exit命令，退出config-pmap-c模式并返回到config模式。

```
R3(config-pmap-c)# exit
R3(config-pmap)# exit
```

### 第五部分：应用防火墙策略

**步骤1：创建一对区域。** 

使用 `zone-pair security` 命令，创建一个名为 **IN-2-OUT-ZPAIR** 的区域对。指定在任务1中创建的源和目标区域。

```
R3(config)# zone-pair security IN-2-OUT-ZPAIR source IN-ZONE destination OUT-ZONE
```

**步骤2：为两个区域之间的流量指定策略映射。** 

通过 `service-policy type inspect` 命令将策略映射及其关联操作附加到区域对，并引用之前创建的策略映射IN-2-OUT-PMAP。

```
R3(config-sec-zone-pair)# service-policy type inspect IN-2-OUT-PMAP
R3(config-sec-zone-pair)# exit
R3(config)#
```

**步骤3：将接口分配给相应的安全区域。** 

在接口配置模式下，使用 `zone-member security` 命令将Fa0/1分配给IN-ZONE，将S0/0/1分配给OUT-ZONE。

```
R3(config)# interface fa0/1
R3(config-if)# zone-member security IN-ZONE
R3(config-if)# exit

R3(config)# interface s0/0/1
R3(config-if)# zone-member security OUT-ZONE
R3(config-if)# exit
```

**步骤4：将运行配置复制到启动配置。** 

### 第六部分：从IN-ZONE到OUT-ZONE测试防火墙功能

验证配置基于区域的策略防火墙后，内部主机仍能访问外部资源。

**步骤1：从内部PC-C，ping外部PC-A服务器。** 

从PC-C命令提示符，ping PC-A的192.168.1.3地址。ping操作应成功。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212636519.png)

**步骤2：从内部PC-C，telnet到路由器R2 S0/0/1接口。** 

a. 从PC-C命令提示符，telnet到R2的10.2.2.2，并提供vty密码 **ciscovtypa55** 。Telnet会话应成功。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212638375.png)

b. 在活动的Telnet会话中，在R3上执行命令 `show policy-map type inspect zone-pair sessions` 以查看已建立的会话。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212640108.png)

源IP地址和端口号是什么？

目标IP地址和端口号是什么？

**步骤3：从PC-C退出R2上的Telnet会话并关闭命令提示符窗口。** 

**步骤4：从内部PC-C，打开一个网页浏览器访问PC-A服务器的网页。** 

在浏览器URL字段中输入服务器IP地址192.168.1.3，并点击“Go”。HTTP会话应成功。在HTTP会话活动期间，在R3上执行命令 `show policy-map type inspect zone-pair sessions` 以查看已建立的会话。

注：如果在您在R3上执行命令之前HTTP会话超时，您需要在PC-C上点击“Go”按钮来生成PC-C与PC-A之间的会话。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212641865.png)

源IP地址和端口号是什么？

目标IP地址和端口号是什么？

**步骤5：关闭PC-C上的浏览器。** 

### 第七部分：从OUT-ZONE到IN-ZONE测试防火墙功能

验证配置基于区域的策略防火墙后，外部主机无法访问内部资源。

**步骤1：从PC-A服务器命令提示符，ping PC-C。** 

从PC-A命令提示符，ping PC-C的192.168.3.3地址。ping操作应失败。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212643452.png)

**步骤2：从路由器R2，ping PC-C。** 

从R2，ping PC-C的192.168.3.3地址。ping操作应失败。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212645060.png)

**步骤3：检查结果。** 

您的完成百分比应为100%。点击“CheckResults”查看反馈和已完成的必要组件验证。