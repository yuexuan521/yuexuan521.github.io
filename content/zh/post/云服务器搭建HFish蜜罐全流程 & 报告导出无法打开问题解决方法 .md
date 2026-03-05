---
title: "云服务器搭建HFish蜜罐全流程 & 报告导出无法打开问题解决方法 "
date: 2026-03-05 12:00:00
category: ""
categories: 
  - ""
tags:
- "HFish"
- "蜜罐"
- "服务器"
- "网络安全"
---

闲来无事，用一台闲置的华为云服务器配置个HFish蜜罐，系统是CentOS 8。

![image-20251106181020300](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310735.png)

[HFish蜜罐官网：https://hfish.net/#/](https://hfish.net/#/)

## 蜜罐基础知识

### 蜜罐的定义

蜜罐是一种主动防御的欺骗技术，其核心思想是通过部署虚假的主机、服务或信息作为诱饵，诱导攻击者实施攻击。在此过程中，蜜罐能够记录攻击行为、分析攻击手法与工具，并推断攻击者的意图，从而帮助防御方更清晰地识别威胁，并针对性地提升真实系统的安全防护能力。[[蜜罐技术_百度百科](https://baike.baidu.com/item/蜜罐技术/9165942)]

### 蜜罐的优势

1. 误报率低，告警精准由于蜜罐本身不承载真实业务，正常情况下不应被访问，因此任何对其发起的连接或探测行为都具有较高的可疑性。相较于传统检测设备容易将正常业务请求误判为攻击的情况，蜜罐几乎不会产生误报，能够实现高度可信的安全告警。
2. 深度交互，信息全面蜜罐可模拟多种业务服务甚至对攻击做出合理响应，从而与攻击者进行深度交互。这使得蜜罐能够获取从初始探测到后续攻击链的完整数据，实现对攻击行为的全流程捕获。尤其在SSL加密通信或工业控制等特殊场景中，蜜罐可有效伪装为目标系统，获取非解密的原始攻击载荷。
3. 主动诱捕，生成威胁情报传统防护往往在攻击探测阶段即告结束，而蜜罐则能主动吸引攻击者深入交互，如诱使其上传恶意工具、连接C2服务器等。这些行为不仅被完整记录，还可进一步提取为高质量的本地威胁情报，赋能于IDS、防火墙等其他安全设备，实现对特定攻击手法（TTPs）的持续检测与预警。[[一篇文章带你搞懂蜜罐-先知社区](https://xz.aliyun.com/news/13713)]
4. 部署灵活，扩展性强蜜罐通常以软件形态存在，无需调整现有网络结构，即可灵活部署于物理网络、云环境或边缘节点。其轻量化的特性使其能够作为探针广泛分布于网络末端，将安全事件统一上报至态势感知平台，实现对全网威胁的可视化监控。

### 蜜罐与威胁情报

蜜罐是高质量威胁情报的稳定来源。通过诱使攻击者暴露其攻击工具、基础设施与行为模式，结合其误报率低、信息详实的特性，蜜罐能够持续产出精准的私有威胁情报。这些情报可整合至本地安全分析平台，有效提升对新型攻击的预见性与防护能力。

## 安装HFish蜜罐

如果部署的环境为Linux，且可以访问互联网，强烈建议使用一键部署脚本进行安装和配置，在使用一键脚本前，请先配置防火墙。

其它版本（及无网环境）安装指南：[https://hfish.net/#/quick-deploy](https://hfish.net/#/quick-deploy)

### 配置防火墙

以root权限运行以下命令，确保配置防火墙开启TCP/4433、TCP/4434

```
firewall-cmd --add-port=4433/tcp --permanent   #（用于web界面启动）
firewall-cmd --add-port=4434/tcp --permanent   #（用于节点与管理端通信）
firewall-cmd --reload
```

![image-20251106162714128](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310736.png)

可能提示需要开启防火墙，使用如下命令：

![image-20251106162510856](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310737.png)

```
systemctl status firewalld
systemctl start firewalld
systemctl status firewalld
```

![image-20251106162558843](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310738.png)

### 一键部署HFish蜜罐

以root权限运行以下一键部署命令，输入“1”，安装并运行。

```
bash <(curl -sS -L https://hfish.net/webinstall.sh)
```

![image-20251106162834638](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310739.png)

出现下面提示，表示成功安装。

![image-20251106163213209](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310740.png)



## 安装MySQL

### 使用 yum 安装

首先，尝试一下直接使用 yum 安装 MySQL

```text
yum install mysql-community-server
```

安装过程中，会提示让我们确认，一律输入 `y` 按回车即可

如果出现以下错误：

```text
Loading mirror speeds from cached hostfile
没有可用软件包 mysql-community-server。
错误：无须任何处理
```

表示我们没有添加安装包的源信息，需要安装 MySQL rpm 源信息

### 安装 MySQL rpm 源信息

打开 [http://dev.mysql.com/downloads/repo/yum/](https://link.zhihu.com/?target=http%3A//dev.mysql.com/downloads/repo/yum/)

![image-20251106180042285](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310741.png)

根据你的系统版本，选择对应的安装包，例如我的是CentOS 7.5，这个系统的Linux内核是 Linux 7，所以我选择了红框内的地址，大家依次类推。

拼接下载地址头：[http://dev.mysql.com/get/](https://link.zhihu.com/?target=http%3A//dev.mysql.com/get/mysql-community-release-el7-5.noarch.rpm)，得到以下地址

```text
 CentOS 7
 http://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
 CentOS 8
 http://dev.mysql.com/get/mysql84-community-release-el8-2.noarch.rpm
```

使用 wget + 刚才拼接的地址，下载安装包源信息

```text
CentOS 7
wget  http://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
CentOS 8
wget http://dev.mysql.com/get/mysql84-community-release-el8-2.noarch.rpm
```

rpm 安装源信息

```text
CentOS 7
rpm -ivh mysql80-community-release-el7-7.noarch.rpm
CentOS 8
rpm -ivh mysql84-community-release-el8-2.noarch.rpm
```

### 禁用 MySQL 模块

如果还是出现错误，需要禁用默认启用的 MySQL 模块。

```
yum module disable mysql
```

![image-20251106171221748](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310742.png)

### 再次安装

再尝试使用 yum 安装MySQL

```text
yum install mysql-community-server
```

安装过程中，会提示让我们确认，一律输入 `y` 按回车即可

### 检查安装是否成功

检查一下刚才的安装是否成功

```text
rpm -qa | grep mysql
```

输出：

```text
mysql-community-libs-compat-8.0.33-1.el7.x86_64
mysql-community-icu-data-files-8.0.33-1.el7.x86_64
mysql80-community-release-el7-7.noarch
mysql-community-common-8.0.33-1.el7.x86_64
mysql-community-libs-8.0.33-1.el7.x86_64
mysql-community-server-8.0.33-1.el7.x86_64
mysql-community-client-8.0.33-1.el7.x86_64
mysql-community-client-plugins-8.0.33-1.el7.x86_64
```

输出类似以上内容，表示安装完成

### 登录和修改密码

我们安装的时候，并没有设置初始密码

所以 mysql 在第一次启动的时候，会自动初始化一个密码

通过以下这行代码，我们可以查看 mysql 自动初始化的密码：

```text
# 第一次启动后，可以查看mysql初始化密码
grep 'temporary password' /var/log/mysqld.log

输出（root@localhost: 后面的是密码）：
2023-04-21T06:03:27.071550Z 6 [Note] [MY-010454] [Server] A temporary password
is generated for root@localhost: r2to%yZ%a)%s
```

### 登录

```text
# 登录mysql，一定要注意：-p和'密码'之间是没有空格的
mysql -u root -p'r2to%yZ%a)%s'
```

### 修改 root 密码

注意了，默认的密码策略，需要：大写英文 + 特殊字符 + 数字

```text
ALTER USER 'root'@'localhost' IDENTIFIED BY 'Root_123';
```

### 创建需要的HFish数据库

```
CREATE DATABASE HFish001;
show databases;
```

![image-20251106172357982](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310743.png)

## 登录Web界面

华为云服务器需要添加一条安全组规则，允许访问4433端口

![image-20251106181213291](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310745.png)

完成安装后，通过以下网址、账号密码登录

```
登陆链接：https://[ip]:4433/web/
账号：admin
密码：HFish2021
```

如果管理端的IP是192.168.1.1，则登陆链接为：https://192.168.1.1:4433/web/

```
注意：访问管理端的URL中必须有/web/目录
```

![image-20251106175157010](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310746.png)

初次配置需要选择数据库，端口默认3306，数据库名：HFish001，用户名密码为MySQL的数据库密码

![image-20251106164928890](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310747.png)

配置成功，等待重启

![image-20251106172526860](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310748.png)

看到下方的管理界面

![image-20251106172711580](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310749.png)



## 配置蜜罐服务

选择“节点管理”，可以配置蜜罐服务

![image-20251107113140322](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310750.png)

华为云服务器需要相应添加安全组规则，开放端口

![image-20251107113313689](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310751.png)

CentOS内的firewall也需要开放相应端口

```
安全组规则：8080,9215,6379,9200,9000,8081,135,139,445,1433,3389

 firewall-cmd --add-port=6379/tcp --add-port=9200/tcp --add-port=9000/tcp --add-port=8081/tcp --add-port=135/tcp --add-port=139/tcp --add-port=445/tcp --add-port=1433/tcp --add-port=3389/tcp --add-port=80/tcp --permanent    //firewall批量添加端口
 
 firewall-cmd --reload
```

测试http://[ip]:[port]，相应的服务已经可以访问了

![image-20251107113512203](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310752.png)

稍等片刻，就可以看到攻击者的记录了

![image-20251107113743753](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310753.png)



## 其它配置

### 配置白名单

在系统配置内，选择“白名单配置”，填入自己的网段可以减少管理蜜罐时产生的误报

![image-20251107114238724](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310754.png)

### 数据大屏

![image-20251109152230667](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310755.png)

其它功能详见HFish蜜罐功能手册：[[快速了解HFish](https://hfish.net/#/README)]



## 报告导出word无法打开问题解决

我在使用HFish蜜罐导出自动生成的周报时遇到问题，下载下来的word（.docx）文件无法打开，显示错误如下。网上修复的方法试了很多，最后找到一种真正有效的方法。

可以在网站上预览：

![image-20251114220404344](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310756.png)



![image-20251114220427065](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310757.png)

通过Word打开显示错误如下：

![image-20251114220224883](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310758.png)

![image-20251114220332844](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310759.png)

我的Office版本为2021，2019版本也会遇到这个问题。

![562c9b288224fcba368ca2ae21f52afb](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310760.png)

### 解决方法：

使用WPS可以正常打开下载下来的。或者用WPS另存为.doc文件后，word也可以正常打开。

![image-20251116223834022](https://raw.githubusercontent.com/yuexuan521/image/main/20260305215310761.png)