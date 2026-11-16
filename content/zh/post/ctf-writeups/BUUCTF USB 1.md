---
title: "BUUCTF USB 1"
date: 2025-10-06 12:09:55
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "MISC"
- "网络安全"
- "流量分析"
---

![文章题图：蓝发动漫少女，配文「刷题嘛~？一起呀」](/images/blog/20251228190257641.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)
- [CTF解题技能之压缩包分析基础篇](https://www.freebuf.com/column/199854.html)
- [BUUCTF：USB](https://blog.csdn.net/mochu7777777/article/details/109632626)
- [(usb键盘隐写)buuctf:USB](https://www.cnblogs.com/Dreamerwd/p/15159027.html#:~:text=%E5%85%88%E6%8A%8A%E5%8E%8B%E7%BC%A9%E5%8C%85%E6%8F%90)


 ![BUUCTF 题目「USB 1」页面，提示 Do your konw usb??，附件为 8c90b141-7….zip](/images/blog/20251228190259617.png)

### 题目描述：

Do your konw usb?? 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件解压，得到233.rar和key.ftm文件

 ![解压得到 233.rar、key.ftm 与一个同名文件夹](/images/blog/20251228190300928.png)

---

### 解题思路：

1、解压233.rar，发现文件损坏，233.png没有解压出来，flag.txt文件中没有flag。

 ![WinRAR 诊断信息：文件头已损坏，233.png 无法解压](/images/blog/20251228190302891.png)

 ![flag.txt 内容只有一句「想要flag??????」](/images/blog/20251228190304627.png)

用010Editor打开，提示文件的第三个块CRC报错，也就是233.rar的文件块。

> RAR是有四个文件块组成的，分别是分别是 `标记块` 、 `归档头部块` 、 `文件块` 、 `结束块` 。

 ![010 Editor 打开 233.rar，文件头是 Rar!，底部报 Header CRC mismatch in block #3](/images/blog/20251228190306232.png)

检查发现是文件块的HEAD_TYPE出错，原数值应为0x74，而非0x7A。修改后即可成功解压。

 ![把偏移处的 CF 改成 00 修正文件头](/images/blog/20251228190308479.png)

 ![另一处 62 也改成 00，修正后 RAR 可正常识别](/images/blog/20251228190310134.png)

2、解压已修改后的文件，得到233.png

 ![修复后解出的 233.png，是一张湖边长椅与女子背影的风景照](/images/blog/20251228190311759.png)

使用StegSolve打开图片，在Blue plane 0通道发现一个二维码

 ![StegSolve 查看 Blue plane 0，浮现出一个二维码](/images/blog/20251228190315061.png)

扫描二维码得到疑似flag的密文： `ci{v3erf_0tygidv2_fc0}` ，猜测为维吉尼亚密码。但缺少key值（密钥），暂时无法解密

 ![QR Research 解码得到 c［V3erf_0tygidv2_fc0}](/images/blog/20251228190316521.png)

3、回过头，key.ftm中应该就有我们要的key值

> **FTM** 是FamiTracker，用于生产任天堂（NES）系统的音乐的音频节目创建的音频跟踪器模块。 它包括短的音频样本和一系列包含旋律音符。

在010 Editor中搜索“key”关键字，发现隐藏zip压缩包，内部有key.pcap

 ![十六进制里 50 4B 03 04 处发现 key.pcap，查找结果中多处命中 KEY](/images/blog/20251228190318757.png)

将zip的数据另存为一个单独的zip文件，解压得到key.pcap。（或者使用WinRAR直接打开key.ftm）

 ![解包得到的 key.pcap，37 KB](/images/blog/20251228190320539.png)

打开发现全部为USB的流量

 ![Wireshark 打开 key.pcap，全是 USB URB_INTERRUPT in 包，底部显示 Leftover Capture Data](/images/blog/20251228190321587.png)

使用隐写脚本USBUsbKeyboardDataHacker.py，得到内容 `aababacbbdbdccccdcdcdbbcccbcbbcbbaababaaaaaaaaaaaaaaaaaakey{xinan}` ，key值为 `xinan` 

[https://github.com/WangYihang/UsbKeyboardDataHacker
](https://github.com/WangYihang/UsbKeyboardDataHacker) 

```shell
python UsbKeyboardDataHacker.py /root/key.pcap
```

 ![Kali 运行 UsbKeyboardDataHacker.py，还原出按键串并以 akey{xinan} 结尾（红框）](/images/blog/20251228190323953.png)

4、使用得到的key值，解密维吉尼亚密码，得到 `fa{i3eei_0llgvgn2_sc0}` 
[维吉尼亚密码加密解密](https://www.qqxiuzi.cn/bianma/weijiniyamima.php) 

 ![维吉尼亚密码工具：ci{v3erf_0tygidv2_fc0} 配密钥 xinan，解出 fa{i3eei_0llgvgn2_sc0}](/images/blog/20251228190326705.png)

猜测明文又经过栅栏加密，解密得到 `flag{vig3ne2e_is_c00l}` 
[栅栏密码加密解密](https://www.qqxiuzi.cn/bianma/zhalanmima.php) 

 ![栅栏密码工具：每组 2 字，把上一步结果还原为 flag{vig3ne2e_is_c00l}](/images/blog/20251228190328258.png)

### flag：

```bash
flag{vig3ne2e_is_c00l}
```