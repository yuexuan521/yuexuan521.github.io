---
title: "Packet Tracer - Configure AAA Authentication on Cisco Routers（在思科路由器上配置 AAA 认证）"
date: 2025-01-23 12:48:24
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "智能路由器"
- "信息与通信"
- "网络安全"
- "服务器"
- "运维"
- "AAA"
- "CCNA"
---



## Packet Tracer - 在思科路由器上配置 AAA 认证

### 地址表

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212534006.png)

### 目标

- 在R1上配置本地用户账户，并使用本地AAA进行控制台和vty线路的身份验证。

- 从R1控制台和PC-A客户端验证本地AAA身份验证功能。

- 配置基于服务器的AAA身份验证，采用TACACS+协议。

- 从PC-B客户端验证基于服务器的AAA（TACACS+）身份验证。

- 配置基于服务器的AAA身份验证，采用RADIUS协议。

- 从PC-C客户端验证基于服务器的AAA（RADIUS）身份验证。

### 背景/场景

网络拓扑图显示了路由器R1、R2和R3。目前，所有管理安全性都基于enable secret密码。您的任务是配置并测试本地及基于服务器的AAA解决方案。

您将在路由器R1上创建一个本地用户账户，并配置本地AAA以测试控制台和vty登录：

- 用户账户：Admin1，密码admin1pa55
  接下来，将配置路由器R2以支持通过TACACS+协议实现的基于服务器的身份验证。TACACS+服务器已经预先配置了以下信息：

- 客户端：R2，关键字为tacacspa55

- 用户账户：Admin2，密码admin2pa55
  最后，您将配置路由器R3以支持通过RADIUS协议实现的基于服务器的身份验证。RADIUS服务器已预先配置如下信息：

- 客户端：R3，关键字为radiuspa55

- 用户账户：Admin3，密码admin3pa55
  此外，路由器还预配置了以下内容：

- 启用秘密密码：ciscoenpa55

- 使用MD5认证的OSPF路由协议，密码为：MD5pa55
  注意：控制台和vty线路尚未预先配置。

注意：尽管IOS版本15.3使用了更为安全的加密哈希算法SCRYPT，但在Packet Tracer当前支持的IOS版本中仍使用MD5。请始终在您的设备上使用最安全的选项。

---

### 第一部分：在R1上配置本地AAA认证以实现控制台访问

**步骤1：测试连通性** 

- 从PC-A向PC-B执行Ping操作。

- 从PC-A向PC-C执行Ping操作。

- 从PC-B向PC-C执行Ping操作。

**步骤2：在R1上配置本地用户名** 

- 在R1上配置一个名为 **Admin1** 的用户名，设置秘密密码为 **admin1pa55** 。

> R1(config)# username Admin1 secret admin1pa55

**步骤3：在R1上为控制台访问配置本地AAA认证** 

- 在R1上启用AAA功能，并配置控制台登录时使用本地数据库进行AAA身份验证。

> R1(config)# aaa new-model
> R1(config)# aaa authentication login default local

**步骤4：配置控制台线路使用定义的AAA认证方法** 

- 在R1上针对控制台登录启用AAA，并配置其使用默认方法列表进行AAA身份验证。

> R1(config)# line console 0
> R1(config-line)# login authentication default

**步骤5：验证AAA认证方法** 

- 使用本地数据库验证用户EXEC登录过程。

通过以上配置后，可以在R1的控制台上用Admin1账户和对应的密码admin1pa55进行登录，验证本地AAA身份验证是否生效。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212536693.png)

### 第二部分：在R1上配置本地AAA认证以实现vty线路访问

**步骤1：配置域名和加密密钥以配合SSH使用** 
a. 在R1上将 **ccnasecurity.com** 设置为域名。
b. 创建一个1024位的RSA加密密钥。

> R1(config)#ip domain-name ccnasecurity.com
> R1(config)# crypto key generate rsa

```shell
R1(config)# crypto key generate rsa
The name for the keys will be: R3.ccnasecurity.com
Choose the size of the key modulus in the range of 360 to 2048 for your
General Purpose Keys. Choosing a key modulus greater than 512 may take
a few minutes.
 
How many bits in the modulus [512]: 1024
% Generating 1024 bit RSA keys, keys will be non-exportable...[OK]
```

**步骤2：为R1上的vty线路配置命名列表AAA认证方法** 

- 配置名为 **SSH-LOGIN** 的命名列表，用于使用本地AAA进行登录认证。

> R1(config)# aaa authentication login SSH-LOGIN local

**步骤3：配置vty线路使用定义的AAA认证方法** 

- 配置vty线路使用已定义的AAA方法，并只允许通过SSH进行远程访问。

> R1(config)# line vty 0 4
> R1(config-line)# transport input ssh
> R1(config-line)# login authentication SSH-LOGIN

**步骤4：验证AAA认证方法** 

- 从PC-A的命令提示符处通过SSH连接到R1，验证SSH配置及AAA身份验证。
   ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212538090.png)

### 第三部分：在R2上配置基于TACACS+服务器的AAA认证

**步骤1：配置备用本地数据库条目（Admin）** 

- 为了备份目的，在R2上配置一个本地用户名 **Admin2** ，密码为 **admin2pa55** 。

> R2(config)# username Admin2 secret admin2pa55

**步骤2：验证TACACS+服务器配置** 

- 点击TACACS+ Server，查看“服务”选项卡中的AAA设置，确认存在针对R2的网络配置条目和针对Admin2的用户设置条目。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212539539.png)

**步骤3：在R2上配置TACACS+服务器详细信息** 

- 在R2上配置AAA TACACS+服务器IP地址和共享密钥。

注意：尽管 `tacacs-server host` 和 `tacacs-server key` 命令已过时，但目前Packet Tracer暂不支持新命令 `tacacs server` 。此处依然使用旧命令进行配置。

> R2(config)# tacacs-server host 192.168.2.2
> R2(config)# tacacs-server key tacacspa55

**步骤4：为R2的控制台访问配置AAA登录认证** 

- 启用R2上的AAA，并配置所有登录通过AAA TACACS+服务器进行认证，若服务器不可用，则使用本地数据库。

> R2(config)# aaa new-model
> R2(config)# aaa authentication login default group tacacs+ local

**步骤5：配置控制台线路使用定义的AAA认证方法** 

- 配置控制台登录使用默认的AAA认证方法。

> R2(config)#line console 0
> R2(config-line)#login authentication default

由于之前已经全局配置了AAA和TACACS+，此处不再需要单独配置console线路。

**步骤6：验证AAA认证方法** 

- 通过AAA TACACS+服务器验证用户EXEC登录。可以尝试从另一设备通过console或SSH等方式登录R2并观察其是否成功通过TACACS+服务器进行身份验证。
   ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212542088.png)

### 第四部分：在R3上配置基于RADIUS服务器的AAA认证

**步骤1：配置备用本地数据库条目（Admin）** 

- 为了备份目的，在R3上配置一个本地用户名 **Admin3** ，密码为 **admin3pa55** 。

> R3(config)# username Admin3 secret admin3pa55

**步骤2：验证RADIUS服务器配置** 

- 点击RADIUS服务器，并查看“服务”选项卡中的AAA设置。注意其中包含针对R3的网络配置条目和针对Admin3的用户设置条目。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212543941.png)

**步骤3：在R3上配置RADIUS服务器详细信息** 

- 在R3上配置AAA RADIUS服务器IP地址和共享密钥。

注意：虽然 `radius-server host` 和 `radius-server key` 命令可能已过时，但当前Packet Tracer版本暂不支持新的 `radius server` 命令。此处仍使用旧命令进行配置。

> R3(config)# radius-server host 192.168.3.2
> R3(config)# radius-server key radiuspa55

**步骤4：为R3的控制台访问配置AAA登录认证** 

- 启用R3上的AAA，并配置所有登录通过AAA RADIUS服务器进行认证，若服务器不可用，则使用本地数据库。

> R3(config)# aaa new-model
> R3(config)# aaa authentication login default group radius local

**步骤5：配置控制台线路使用定义的AAA认证方法** 

- 配置控制台登录使用默认的AAA认证方法。

> R3(config)#line console 0
> R3(config-line)#login authentication default

由于之前已经全局配置了AAA和RADIUS，此处不再需要单独配置console线路。

**步骤6：验证AAA认证方法** 

- 通过AAA RADIUS服务器验证用户EXEC登录。可以尝试从另一设备通过console或SSH等方式登录R3并观察其是否成功通过RADIUS服务器进行身份验证。
   ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212545768.png)

**步骤7：检查结果** 

- 您的完成度应达到100%。点击“检查结果”以查看反馈和已完成所需组件的验证情况。

