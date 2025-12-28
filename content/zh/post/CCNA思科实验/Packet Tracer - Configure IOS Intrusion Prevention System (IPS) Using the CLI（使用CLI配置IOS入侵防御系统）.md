---
title: "Packet Tracer - Configure IOS Intrusion Prevention System (IPS) Using the CLI（使用CLI配置IOS入侵防御系统）"
date: 2025-01-23 12:46:59
category: "CCNA思科实验"
categories: 
  - "CCNA"
tags:
- "ios"
- "智能路由器"
- "网络"
- "服务器"
- "运维"
- "网络安全"
- "安全"
---



## Packet Tracer - 使用CLI配置IOS入侵防御系统（IPS）

### 地址表

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212610089.png)

### 目标

- 启用IOS入侵防御系统（IPS）。

- 配置日志记录功能。

- 修改IPS签名规则。

- 验证IPS配置。

### 背景/场景

您的任务是在R1上启用IPS，扫描进入192.168.1.0网络的流量。

标记为“Syslog”的服务器用于接收和记录IPS消息。您需要配置路由器以识别该syslog服务器，并将日志消息发送到该服务器。在使用syslog监控网络时，在syslog消息中显示正确的时间和日期至关重要。因此，请设置时钟并为路由器上的日志记录功能配置时间戳服务。最后，启用IPS以产生警报并在线路上丢弃ICMP回显应答数据包。

服务器和PC已预先配置好。路由器也已经预先配置了以下内容：

o 启用密码： **ciscoenpa55** 

o 控制台密码： **ciscoconpa55** 

o SSH用户名和密码： **SSHadmin / ciscosshpa55** 

o OSPF进程号101

### 第一部分：启用IOS入侵防御系统（IPS）

注意：在Packet Tracer中，路由器已经导入并安装了签名文件。它们是闪存中的默认xml文件。因此，不需要配置公钥和手动导入签名文件。

**步骤1：启用安全技术包。** 

a. 在R1上，执行 `show version` 命令查看技术包许可证信息。

b. 如果尚未启用“Security Technology”包，请使用以下命令启用该包：

> R1(config)# license boot module c1900 technology-package securityk9

c. 接受最终用户许可协议。

d. 保存运行配置，并重新加载路由器以启用安全许可证。

> R1#write
> Building configuration…
> [OK]
> R1#reload

e. 使用 `show version` 命令验证是否已启用“Security Technology”包。

**步骤2：验证网络连接性。** 

a. 从PC-C向PC-A发送ping请求。应能成功ping通。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212612030.png)

b. 从PC-A向PC-C发送ping请求。应能成功ping通。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212617990.png)

**步骤3：在闪存中创建一个IOS IPS配置目录。** 

在R1上，使用 `mkdir` 命令在闪存中创建一个目录。将目录命名为ipsdir。

> R1#mkdir ipsdir
> Create directory filename [ipsdir]?
> Created dir flash:ipsdir

**步骤4：配置IPS签名存储位置。** 

在R1上，将IPS签名存储位置配置为刚刚创建的目录。

> R1(config)#ip ips config location flash:ipsdir

**步骤5：创建一个IPS规则。** 

在R1的全局配置模式下，使用 `ip ips name "name"` 命令创建一个IPS规则名称。将IPS规则命名为 **iosips** 。

> R1(config)#ip ips name iosips

**步骤6：启用日志记录。** 

IOS IPS支持使用syslog发送事件通知。Syslog通知默认情况下是启用的。如果启用了loggingconsole，则会显示IPS的syslog消息。

a. 如未启用syslog，则启用syslog。

> R1(config)#ip ips notify log

b. 如有必要，从特权EXEC模式下使用 `clock set` 命令重置时钟。

> R1#clock set 19:31:59 6 jan 2024

c. 使用 `show run` 命令验证路由器上的日志记录时间戳服务是否已启用。如果没有启用，则启用时间戳服务。

> R1(config)#service timestamps log datetime msec

d. 将日志消息发送到位于IP地址192.168.1.50的syslog服务器。

> R1(config)#logging host 192.168.1.50

**步骤7：配置IOS IPS使用签名类别。** 

使用 `retired true` 命令退休所有签名类别（签名发布内的所有签名）。使用 `retired false` 命令取消退休IOS_IPS Basic类别。

> R1(config)#ip ips signature-category
> R1(config-ips-category)#category all
> R1(config-ips-category-action)#retired true
> R1(config-ips-category-action)#exit
> R1(config-ips-category)#category ios_ips basic
> R1(config-ips-category-action)#retired false
> R1(config-ips-category-action)#exit
> R1(config-ips-category)#exit

**步骤8：将IPS规则应用于接口。** 

在接口配置模式下，使用 `ip ips name "direction"` 命令将IPS规则应用于接口。将规则应用于R1的G0/1接口的出站方向。启用IPS后，控制台行将收到一些日志消息，表明IPS引擎正在初始化。

> R1(config)#int g0/1
> R1(config-if)#ip ips iosips out

注：direction in表示IPS仅检查进入接口的流量。同样地，out表示IPS仅检查离开接口的流量。

### 第二部分：修改签名

**步骤1：更改签名的事件动作。** 

取消退休echo request签名（签名ID 2004，子签名ID 0），启用它，并将签名动作更改为alert和drop。

> R1(config)#ip ips signature-definition
> R1(config-sigdef)#signature 2004 0
> R1(config-sigdef-sig)#status
> R1(config-sigdef-sig-status)#retired false
> R1(config-sigdef-sig-status)#enabled true
> R1(config-sigdef-sig-status)#exit
> R1(config-sigdef-sig)#engine
> R1(config-sigdef-sig-engine)#event-action produce-alert
> R1(config-sigdef-sig-engine)#event-action deny-packet-inline
> R1(config-sigdef-sig-engine)#exit
> R1(config-sigdef-sig)#exit
> R1(config-sigdef)#exit

**步骤2：使用show命令验证IPS配置。** 

使用 `show ip ips all` 命令查看IPS配置状态摘要。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212619669.png)

iosips规则应用于哪些接口以及什么方向？

**步骤3：验证IPS是否正常工作。** 

a. 从PC-C尝试ping PC-A。这些ping请求成功了吗？请解释原因。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212621339.png)

b. 从PC-A尝试ping PC-C。这些ping请求成功了吗？请解释原因。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212623416.png)

**步骤4：查看syslog消息。** 

a. 点击Syslog服务器。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212625436.png)

b. 选择“服务”标签页。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228212627003.png)

c. 在左侧导航菜单中，选择SYSLOG以查看日志文件。

**步骤5：检查结果。** 

您的完成百分比应为100%。点击“CheckResults”查看反馈信息及已完成的必要组件验证。