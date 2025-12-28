---
title: "Packet Tracer - Configure and Verify a Site-to-Site IPsec VPN Using CLI（使用命令行界面配置和验证站点到站点IPsec VPN）"
date: 2025-01-23 16:37:23
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "智能路由器"
- "网络"
- "服务器"
- "运维"
- "思科"
- "网络安全"
- "计算机网络"
---



## PacketTracer - 使用命令行界面配置和验证站点到站点IPsec VPN

### 地址表

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212547156.png)

### 目标

- 验证整个网络的连通性。

- 配置R1以支持与R3之间的站点到站点IPsec VPN。

### 背景/场景

网络拓扑图显示了三个路由器。您的任务是配置R1和R3，以便在各自局域网（LAN）之间的流量流动时支持站点到站点的IPsec VPN。IPsec VPN隧道从R1经由R2到达R3。R2充当通过设备，并不了解VPN的存在。IPsec提供了一种在不受保护的网络（如互联网）上安全传输敏感信息的方法。IPsec在网络层运行，负责保护并验证参与IPsec设备（对等体）之间的IP数据包，例如Cisco路由器。

**ISAKMP阶段1策略参数** 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212549003.png)

注意：加粗参数为默认值。只有非加粗参数需要明确配置。

**IPsec阶段2策略参数** 

 ![（此处未给出具体参数，请补充完整）](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212550698.png)

路由器已预先配置以下内容：

· 控制台线路密码： **ciscoconpa55** 

· vty线路密码： **ciscovtypa55** 

· 启用密码： **ciscoenpa55** 

· SSH用户名和密码： **SSHadmin / ciscosshpa55** 

· OSPF进程号 **101** 

### 第一部分：在R1上配置IPsec参数

**步骤1：测试连通性。** 

从PC-A向PC-C发送ping请求。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212552384.png)

**步骤2：启用安全技术包。** 

a. 在R1上执行 `show version` 命令查看安全技术包许可证信息。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212554645.png)

b. 如果未启用安全技术包，请使用以下命令启用该包。

> R1(config)# license boot module c1900 technology-package securityk9

c. 接受最终用户许可协议。

d. 保存运行配置并重新加载路由器以启用安全许可证。

e. 使用 `show version` 命令验证是否已启用安全技术包。

**步骤3：在R1上识别感兴趣流量。** 

配置 **ACL 110** ，将来自R1 LAN到R3 LAN的流量标识为“感兴趣”流量。当R1和R3之间的LAN之间存在流量时，这种感兴趣的流量会触发实施IPsec VPN。除了这些流量外，所有其他源自LAN的流量都不会被加密。由于存在隐式拒绝所有规则，因此无需配置deny ip any any语句。

> R1(config)# access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255

**步骤4：在R1上配置IKE阶段1 ISAKMP策略。** 

在R1上配置crypto ISAKMP策略 **10** 属性以及共享的crypto密钥 **vpnpa55** 。请参考ISAKMP阶段1表中特定的参数进行配置。默认值不需要配置，因此只需要配置加密方法、密钥交换方法和DH方法。

注：当前Packet Tracer支持的最大DH组是组5。在生产网络中，您至少应配置DH 14。

> R1(config)# crypto isakmp policy 10
> R1(config-isakmp)# encryption aes 256
> R1(config-isakmp)# authentication pre-share
> R1(config-isakmp)# group 5
> R1(config-isakmp)# exit
> R1(config)# crypto isakmp key vpnpa55 address 10.2.2.2

**步骤5：在R1上配置IKE阶段2 IPsec策略。** 

a. 创建名为VPN-SET的转换集，使用 **esp-aes** 和 **esp-sha-hmac** 。

> R1(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac

b. 创建名为VPN-MAP的crypto映射，将所有阶段2参数绑定在一起。使用序列号10，并将其标识为ipsec-isakmp映射。

> R1(config)# crypto map VPN-MAP 10 ipsec-isakmp
> R1(config-crypto-map)# description VPN connection to R3
> R1(config-crypto-map)# set peer 10.2.2.2
> R1(config-crypto-map)# set transform-set VPN-SET
> R1(config-crypto-map)# match address 110
> R1(config-crypto-map)# exit

**步骤6：配置接口上的crypto映射。** 

将VPN-MAP crypto映射绑定到 **Serial 0/0/0** 出站接口。

> R1(config)# interface s0/0/0
> R1(config-if)# crypto map VPN-MAP

### 第二部分：在R3上配置IPsec参数

**步骤1：启用安全技术包。** 

a. 在R3上执行 `show version` 命令以验证是否已启用安全技术包许可证信息。

b. 如果尚未启用安全技术包，则启用该包并重新加载R3。

**步骤2：配置路由器R3以支持与R1的站点到站点VPN。** 

在R3上配置相应的参数。配置 **ACL 110** ，将来自R3 LAN到R1 LAN的流量标识为“感兴趣”流量。

> R3(config)# access-list 110 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255

**步骤3：在R3上配置IKE阶段1 ISAKMP属性。** 

在R3上配置crypto ISAKMP策略 **10** 属性以及共享的crypto密钥 **vpnpa55** 。

> R3(config)# crypto isakmp policy 10
> R3(config-isakmp)# encryption aes 256
> R3(config-isakmp)# authentication pre-share
> R3(config-isakmp)# group 5
> R3(config-isakmp)# exit
> R3(config)# crypto isakmp key vpnpa55 address 10.1.1.2

**步骤4：在R3上配置IKE阶段2 IPsec策略。** 

a. 创建名为VPN-SET的转换集，使用 **esp-aes** 和 **esp-sha-hmac** 。

> R3(config)# crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac

b. 创建名为VPN-MAP的crypto映射，将所有阶段2参数绑定在一起。使用序列号 **10** ，并将其标识为ipsec-isakmp映射。

> R3(config)# crypto map VPN-MAP 10 ipsec-isakmp
> R3(config-crypto-map)# description VPN connection to R1
> R3(config-crypto-map)# set peer 10.1.1.2
> R3(config-crypto-map)# set transform-set VPN-SET
> R3(config-crypto-map)# match address 110
> R3(config-crypto-map)# exit

**步骤5：配置接口上的crypto映射。** 

将VPN-MAP crypto映射绑定到 **Serial 0/0/1** 出站接口（注意：此操作不会被评估）。

> R3(config)# interface s0/0/1
> R3(config-if)# crypto map VPN-MAP

### 第三部分：验证IPsec VPN

**步骤1：在出现感兴趣流量之前验证隧道。** 

在R1上执行 `show crypto ipsec sa` 命令。注意封装、加密、解封装和解密的包数量均设置为 **0** 。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212557007.png)

**步骤2：创建感兴趣流量。** 

从PC-A向PC-C发送ping请求。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212558358.png)

**步骤3：在产生感兴趣流量后验证隧道。** 

在R1上重新执行 `show crypto ipsec sa` 命令。注意包的数量大于0，这表明IPsec VPN隧道正在工作。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212600560.png)

**步骤4：创建非感兴趣流量。** 

从PC-A向PC-B发送ping请求。注：从路由器R1向PC-C或R3向PC-A发送ping请求不属于感兴趣流量。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212602410.png)

**步骤5：再次验证隧道。** 

在R1上重新执行 `show crypto ipsec sa` 命令。注意包的数量没有改变，这证实了非感兴趣流量并未被加密。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212604285.png)

**步骤6：检查结果。** 

您的完成百分比应为100%。点击“CheckResults”查看反馈信息以及已完成的必要组件验证。

### 实验脚本：

**R1:** 

```bash
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 5
 
crypto isakmp key vpnpa55 address 10.2.2.2

crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac

access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255

crypto map VPN-MAP 10 ipsec-isakmp 
 description VPN connection to R3
 set peer 10.2.2.2
 set transform-set VPN-SET 
 match address 110

interface Serial0/0/0
 crypto map VPN-MAP
```

**R3:** 

```bash
crypto isakmp policy 10
 encr aes 256
 authentication pre-share
 group 5

crypto isakmp key vpnpa55 address 10.1.1.2

crypto ipsec transform-set VPN-SET esp-aes esp-sha-hmac

access-list 110 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255

crypto map VPN-MAP 10 ipsec-isakmp 
 description VPN connection to R1
 set peer 10.1.1.2
 set transform-set VPN-SET 
 match address 110

interface Serial0/0/1
 crypto map VPN-MAP
```