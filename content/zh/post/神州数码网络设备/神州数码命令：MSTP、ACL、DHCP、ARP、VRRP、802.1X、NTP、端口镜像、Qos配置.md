---
title: "神州数码命令：MSTP、ACL、DHCP、ARP、VRRP、802.1X、NTP、端口镜像、Qos配置"
date: 2024-10-12 11:23:34
category: "神州数码网络设备"
categories: 
  - "神州数码网络设备"
tags:
- "windows"
- "网络"
- "神州数码"
- "ACL"
- "DHCP"
- "terminal"
- "DCN"
---

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251222130004043.jpeg)

### MSTP、ACL、DHCP、ARP、VRRP、802.1X、NTP、端口镜像、Qos配置

#### 一、MSTP配置（MSTP是基于STP或RSTP的，因此在配置前应开启任一模式）

1、开启生成树
命令： `spanning-tree` 
2、设置交换机运行生成树的模式
命令： `spanning-tree mode {mstp|stp}` 
3、进入MSTP域配置模式
命令： `spanning-tree mst configuration` 
4、为域命名（域配置模式）
命令： `name <your_domain_name>` 
5、将实例应用到VLAN（域配置模式）
命令： `instance <instance_id> vlan <vlan_range>` 
6、设置实例的优先级（在全局模式配置）
命令： `spanning-tree mst priority <priority_value>` 
7、设置当前端口指定实例的优先级值（端口配置模式下）
命令： `spanning-tree mst port-priority <priority_value>` 

#### 二、ACL配置

**（一）、标准的ACL** 
`access-list 10 {deny | permit} {<源网络> <反掩码>} | any-source | {host-source <源地址>}}` 
**（二）、命名标准ACL** 
1、命名
`ip access-list standard acb` 
2、制定规则
`{deny | permit} {<源网络> <反掩码>} | any-source | {host-source <源地址>}}` 
3、应用到接口
`ip access-group 名称 in/out` 
**（三）、扩展的ACL** 
`access-list 102 {deny | permit} 协议类型 {<源网络> <反掩码>} | any-source | {host-source<源地址>}} {<目标网络> <反掩码>} | any-destination | {host-destination <目标主机地址>}} [precedence <优先级值（0-7）>] [tos<服务级类型（1-15）>][time-range<时间范围名称>]` 
**（四）、命名扩展ACL** 
1、命名
`ip access-list extended acb` 
2、制定规则
`[no] {deny | permit} 协议类型 <源网络> <反掩码> | any-source | {host-source` 
`<源地址>}} {<目标网络> <反掩码>} | any-destination | {host-destination <目标主机地址>}} [precedence <优先级值（0-7）>] [tos<服务级类型（1-15）>][time-range<时间范围名称>]` 
3、应用到接口

**(五)、MAC命名扩展ACL** 
1、命名： `mac-ip-access-list extended` 
2、制定规则
`{deny|permit}{any-source-mac|{host-source-mac<host_smac>}|{}}` 
`{any-destination-mac|{host-destination-mac<host_dmac>}|{}}` 

```
例：在SW3的3号接口上,要求mac为 00-FF-51-FD-AE-15的主机不能访问行政部的主机MAC地址为：E0-94-67-05-5D-84，其余主机正常访问。
mac-ip-access-list extended aaa
deny host-source-mac 00-FF-51-FD-AE-15 host-destination-mac E0-94-67-05-5D-84
```

3、应用到接口
`ip access-group 名称 in/out` 

**（六）、制定时间范围** 

1、命名
`[no] time-range <time_range_name>` 
2、一周之内的循环时间
`[no] absolute-periodic{Monday|Tuesday|Wednesday|Thursday|Friday|Saturday|` 
`Sunday}<开始时间>to{Monday|Tuesday|Wednesday|Thursday|Friday|Saturday|` 
`Sunday} <结束时间>` 
或
`[no]periodic{Monday+Tuesday+Wednesday+Thursday+Friday+Saturday+Sunday}|` 
`daily| weekdays | weekend} <开始时间> to <结束时间>` 
3、绝对时间范围
`[no]absolute start <开始时间> <开始日期> [end <结束时间> <结束日期>]` 

#### 三、DHCP 配置

1、创建地址池
命令： `ip dhcp pool <pool_name>` 
2、配置地址可分配的范围及其它参数
（1）配置地址可分配范围
命令： `network-address [subnet-mask | prefix-length]` 

（2）配置地址池租用期限
命令： `lease day [hour [minute]] | infinite` 
（3）配置DHCP客户机的缺省网关
命令： `default-router <gateway_ip>` 
（4）配置DHCP客户机的DNS服务器地址
命令： `dns-server <dns_server_ip>` 
（5）配置WINS服务器的地址
命令： `netbios-name-server <wins_server_ip>` 
（6）配置DHCP客户机的节点类型
命令： `netbios-node-type ｛b-node|h-node|m-node|p-node|｝` 
（7）配置WWW服务器的地址
命令： `option {ascii | hex | ipaddress }` 
（8）排除某个地址或地址范围不用于动态分配
命令： `ip dhcp excluded-address <start_ip> <end_ip>` 
（9）配置DHCP客户机的域名
命令： `domain-name <domain_name>` 
(10) 打开DHCP服务器Ping方式检测地址冲突的功能
命令： `ip dhcp conflict ping-detection enable` 
3、启用DHCP服务
命令： `service dhcp enable` 
4、 **DHCP手工分配** 
创建静态绑定关系，将特定MAC地址与固定的IP地址关联：

- 创建地址池后进入该地址池配置模式

- 绑定IP与MAC：
  命令： `host <ip_address>` 
  然后指定硬件地址类型及MAC地址：
  `hardware-address Ethernet <mac_address>` 
  和可选的主机名：
  `client-name <hostname>`

5、 **DHCP Snooping** 

- 开启 DHCP Snooping 功能：
  命令： `ip dhcp snooping enable`

- 设置信任端口：
  先进入端口配置模式：
  `interface <interface_id>` 
  然后设置为信任端口：
  `ip dhcp snooping trust`

- 设置自动防御动作：
  可能的命令格式：
  `ip dhcp snooping action {shutdown|blackhole} [recovery <time>]` 
  当DHCP Snooping检测到非法DHCP活动时，可以选择关闭接口（shutdown）或丢弃数据包（blackhole），并可选择恢复时间间隔。

#### 四、ARP配置

**1、配置静态ARP 表项** 

```
Switch(Config-Vlan1)#arp 1.1.1.1 00-03-0f-f0-12-34
```

**2、ARP GUARD** 
添加 ARP GUARD 地址: `arp-guard ip <IP地址>` 
**3、防ARP 扫描** 
1)启动防ARP 扫描功能

```
SwitchA(config)#anti-arpscan enable
```

2）配置信任/超级信任端口

```
SwitchA (Config-If-Ethernet0/0/2)#anti-arpscan trust port //信任端口
SwitchA (Config-If-Ethernet0/0/19)#anti-arpscan trust supertrust-port //超级信任
```

3）配置信任IP

```
SwitchA(config)#anti-arpscan trust ip 192.168.1.100 255.255.255.0
```

4）配置自动恢复时间

```
Switch(Config)#anti-arpscan recovery enable //先在交换机上启动自动恢复功能
SwitchA(config)#anti-arpscan recovery time 3600
```

**4、设置基于端口的防ARP 扫描的接收ARP 报文的阈值** 

```
Switch(Config)#anti-arpscan port-based threshold 20
```

**5、设置基于IP 的防ARP 扫描的接收ARP 报文的阈值** 

```
Switch(Config)#anti-arpscan port-based threshold 6
```

**6、显示ARP 映射表：** 

```
Switch#sh arp
```

#### 五、VRRP配置

**(一) 在三层交换机上配置VRRP** 
1、进入要配置的接口（端口或虚拟子接口）
2、进入VRRP协义配置模式（两个设备的编号要一致）

```
DG5950(config)# router vrrp 1
```

3、配置虚拟网关

```
DG5950(config-router)#virtual-ip 192.168.100.254 //虚拟网关,两边必须一致
```

4、设置优先级

```
DG5950 (config-router)#priority 200
```

5、配置抢占模式(可选)

```
DG5950(config-router)# preempt-mode true
```

6、启动VRRP

```
DG5950(config-router)#enable
```

7、查看VRRP

```
Ruijie# show vrrp brief
```

**(二) 在路由器上配置VRRP** 
1、进入要配置的子接口并配置虚拟子接口IP，封装到指定VLAN。（单臂）

```
RB_config#interface GigaEthernet0/3.10
RB_config-if#ip address 192.168.10.254 255.255.255.0
RB_config-if#encapsulation dot1Q 10
```

2、配置置虚拟网关

```
RB_config-if#vrrp 10 associate 192.168.10.250 255.255.255.0
```

3、配置优先级

```
RB_config-if#vrrp 10 priority 150
```

4、配置抢占模式(可选)

```
RB_config-if#vrrp 10 preempt
```

5、配置要监听的上行端口

```
RB_config-if#vrrp 10 track interface GigaEthernet0/4 30
```

#### 六 、802.1X

```
Switch(Config)#interface vlan 1↵ //创建VLAN
Switch(Config-if-vlan1)#ip address 10.1.1.2 255.255.255.0↵ //配置IP地址
Switch(Config-if-vlan1)#exit↵
Switch(Config)#radius-server authentication host 10.1.1.3//配置认证服务器地址
Switch(Config)#radius-server accounting host 10.1.1.3↵ //配置计费服务器地址
Switch(Config)#radius-server key test↵ //配置RADIUS 认证密钥
Switch(Config)#aaa enable↵ //开启交换机的aaa认证功能
Switch(Config)#aaa-accounting enable↵ //开启交换机的计费功能

Switch(Config)#dot1x enable↵ //打开交换机全局的802.1x 功能
Switch(Config)#interface ethernet 0/0/2↵ //进入端口
Switch(Config-Ethernet0/0/1)#dot1x enable↵ //打开交换机端口的802.1x 功能
Switch(Config-Ethernet0/0/1)#dot1x port-method macbased↵ //接口的接入方式为macbased
Switch(Config-Ethernet0/0/1)#dot1x port-control auto↵ //接口的控制方式为auto
Switch(Config-Ethernet0/0/1)#exit
```

#### 七、NTP配置

例：客户端

```
Switch(config)#ntp enable //开启NTP功能
Switch(config)#ntp server 192.168.1.11 //配置时间服务器地址
Switch(config)#ntp server 192.168.2.11 //配置时间服务器地址
```

#### 八、端口镜像设置

1、指定镜像源端口：
`Switch(Config)#monitor session <session_number> source interface <interface_list> {tx | rx | both}` 

- `rx` 表示只镜像从源端口接收到的流量（入站流量）。

- `tx` 表示只镜像从源端口发送出去的流量（出站流量）。

- `both` 表示同时镜像源端口的入站和出站流量。

2、指定镜像目的端口；
`Switch(Config)#monitor session <session_number> destination interface <destination_interface>` 
例：

```
S4600-28P-SI(config)#monitor session 1 source interface e1/0/1
S4600-28P-SI(config)#monitor session 1 destination interface e1/0/3
```

#### 九、Qos

##### (一)路由器的Qos

网络组件处理接收通信量溢出的一个方法是：使用队列算法对通信按照优先次序排在输出链接上。一般有：FIFO，PQ（Priority Queuing），CQ（Custom Queuing），SFQ（Fair Queuing）以及WFQ（Weighted Fair Queuing）。
1.PQ（Priority Queuing）配置任务序列
1)定义特定的访问控制列表（如果要用到它们的话）
2)创建优先级列表
`priority-list protocol {high | medium | normal | low}` 
或： `priority-list interface {high | medium | normal | low}` 

*任何不匹配优先级队列中任何一行的流量将被放入到缺省队列中。对于优先级队列，如果没有定义，缺省队列是normal。下面的命令用于安排一个缺省队列：
`priority-list default {high | medium |normal |low}` 
*为了给每一个优先级队列指定所能够容纳的最大数据包个数，可以在全局配置模式下使用下面的命令：
`priority-list queue-limit [ ]` 
3)把优先级列表应用到接口上
`priority-group` 
4)配置接口上的驱动队列长度 （可选项）
serial queue 取值可为2、4、8和16
配置示范：配置串口1的驱动队列长度为4。
Router(config)#interface serial 2/0
Router(config-serial2/0)#serial queue 4
5)验证队列过程（可选项）
show queuing priority
例：
Router(config)#queue-list 1 protocol ip 1 tcp 23 放入到高优先级队列
Router(config)#priority-list 1 interface ethernet 0 high //接口0的流量都被放入到高优先级
Router(config)#priority-list 1 protocol ip middle //IP流量被放入到medium队列中
Router(config)#priority-list 1 protocol ipx middle //IPX流量被放入到medium队列中
Router(config)#priority-list 1 default low //缺省队列是LOW
Router(config)#priority-list 1 queue-limit 10 40 60 90 //high，middle，normal，low的10，40，60，90
2.CQ（Custom Queuing）配置任务序列
1)定义特定的访问控制列表（如果要用到它们的话）
2)创建自定义队列
queue-list protocol < keyword-value>
*使用如下命令，可以指定任何经过一特定接口进入路由器的数据流到一特定队列：
queue-list interface
3)指定自定义队列的最大容量
queue-list queue limit
使用以下命令能改变字节总数阈值：
queue-list queue byte-count
4)把自定义队列应用到接口上
custom-queue-list
5)验证队列过程（可选项）
show queuing custom

例：
Router(config)#queue-list 1 interface ethernet 0 6 //接口0的数据流放入到队列6中
Router(config)#queue-list 1 protocol ip 1 tcp 21
Router(config)#queue-list 1 protocol ip 2 //IP流量均放入到队列2中
Router(config)#queue-list 1 protocol ipx 3 IPX流量放入到队列3中
Router(config)#queue-list 1 default 5 //不匹配以上规则的流量放到队列5中
Router(config)#queue-list 1 queue 1 limit 60 //队列1要处理60个记录（数据包）
Router(config)#queue-list 1 queue 1 byte-count 4500 //队列1要处理4500个字节

##### (二)交换机QoS

1． 启动QoS 功能
mls qos
2． 配置分类表（classmap）
建立一个分类规则，可以按照ACL，VLAN ID，IP Precedence，DSCP（差分服务代码点）来分类。
建立一个class-map（分类表），并进入class-map 模式
class-map
设置分类表中的匹配标准；本命令的no操作为删除指定的匹配标准。
match {access-group | ip dscp | ip precedence| vlan }
3． 配置策略表（policymap）
建立一个策略表，可以对相应的分类规则进行带宽限制，优先级降低等操作。
(1)建立一个policy-map（策略表），并进入policy-map（策略表）模式；
policy-map
(2)建立一个 class（策略分类表），并进入
class
(3)为分类后的流量分配一个新的DSCP和IP Precedence 值；
set {ip dscp | ip precedence}
(4)为分类后的流量配置一个策略；
police [exceed-action {drop |policed-dscp-transmit}]
(5)定义一个集合策略，这个策略可以在同一个策略表内部被多个策略分类表使用；
mls qos aggregate-policer exceed-action {drop|policed-dscp-transmit}
(6)为分类后的流量应用一个集合策略；
police aggregate
4． 将QoS 应用到端口
配置端口的信任模式，或者绑定策略。策略只有绑定到具体的端口，才在此端口生效。
(1)配置交换机端口信任状态
mls qos trust [cos [pass-through dscp]|dscp[pass-through cos]|ip-precedence [pass-throughcos]|port priority ]
(2)配置交换机端口的缺省CoS值；
mls qos cos { }
(3)在端口上应用一个策略表
service-policy {input | output}
(4)在端口上应用 DSCP 转换映射，本命令的no 操作为恢复DSCP 转换映射的缺省值。
mls qos dscp-mutation
5． 配置出队队列工作方式和权重
将出队工作方式配置为PQ 方式或者WRR 方式，设置4 个出口队列带宽的比例，以及
内部优先级到出口队列的映射关系，都是全局命令，对所有端口生效。
(1)设置交换机所有端口出队列的WRR 权重
wrr-queue bandwidth
(2)配置队列出队工作方式，将队列配置成pq出队工作方式
priority-queue out
(3)设置CoS 值对应交换机端口出队列的映射
wrr-queue cos-map <cos1 …cos8>
6． 配置QoS 映射关系
配置cos 到dscp，dscp 到cos，dscp mutation，ip precedent 到dscp，policed-dscp
的映射关系。
设置CoS-to-DSCP 映射，DSCP-to-CoS 映射，DSCP-to-DSCP-mutation映射，IP-precedence-to-DSCP映射和policed-DSCP 映射；
mls qos map {cos-dscp <dscp1…dscp8> | dscp-cos to | dscp-mutation to |ip-prec-dscp <dscp1…dscp8> | policed-dscp to }
7.显示QoS 的集合策略配置信息。
命令：show mls qos aggregate-policer []
Switch #show mls qos aggregate-policer policer1
8.显示QoS 的全局配置信息。
命令：show mls-qos
例：在SWB上设置PCA的流量不能超过5M



