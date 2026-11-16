---
title: "BUUCTF 面具下的flag 1"
date: 2024-07-23 22:52:23
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "CTF"
- "MISC"
- "BUUCTF"
- "隐写"
---

![题图同样是那位蓝发女孩](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193637232.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读

- [CTF Wiki](https://ctf-wiki.org/)


![题目页「面具下的flag 1」，附件 2c1606ee-47…](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193639343.png)

### 题目描述：

下载附件，得到一张.jpg图片。

### 密文：

![《你的名字》海报：男女主角隔着一条光带对望](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193641415.jpeg)

---

### 解题思路：

1、将图片放到Kali中，使用binwalk检测出隐藏zip包。

![binwalk mianju.jpg：JPEG EXIF，478718（0x74DFE）起是 Zip，compressed 153767、uncompressed 3145728，name: flag.vmdk](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193644310.png)

使用foremost提取zip压缩包到output目录下

![foremost mianju.jpg，输出 foundat=flag.vmdk](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193646403.png)

![tree output：audit.txt、jpg/00000000.jpg、zip/00000934.zip](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193649231.png)

解压zip压缩包，需要密码，但题目中没有关于密码的提示，猜测是zip伪加密。

![unzip 时 flag.vmdk 要密码，输错就 skipping](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193650854.png)

2、使用010Editor修改压缩源文件数据区和目录区的全局方式位标记（下图红色标识），将伪压缩文件恢复到未加密的状态。

![010 Editor 里 2:58D0h 红框 00 00 是加密标志，右侧可见 flag.vmdk](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193652725.png)

```bash
未加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为00 00

伪加密：

文件头中的全局方式位标记为00 00

目录中源文件的全局方式位标记为09 00

真加密：

文件头中的全局方式位标记为09 00

目录中源文件的全局方式位标记为09 00

ps:也不一定要09 00或00 00，只要是奇数都视为加密，而偶数则视为未加密
```

解压zip压缩包，得到一个.vmdk文件。在Kali中使用7z解压vmdk文件，查看目录。

```powershell
7z x flag.vmdk -o./
```

![解开后是 vmdk 镜像，tree 出 key_part_one 与 key_part_two 两个目录](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193654525.png)

3、查看key_part_one目录下的NUL文件，如下：

![key_part_one 里那个 NUL 文件，内容是 Ook! 代码](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193656647.png)

```bash
+++++ +++++ [->++ +++++ +++<] >++.+ +++++ .<+++ [->-- -<]>- -.+++ +++.<
++++[ ->+++ +<]>+ +++.< +++++ +[->- ----- <]>-- ----- --.<+ +++[- >----
<]>-- ----- .<+++ [->++ +<]>+ +++++ .<+++ +[->- ---<] >-.<+ +++++ [->++
++++< ]>+++ +++.< +++++ [->-- ---<] >---- -.+++ .<+++ [->-- -<]>- ----- .< 
```

这是 **Brainfuck编码** ，使用在线工具进行解密，得到前半部分的flag。
[在线解密：https://www.splitbrain.org/services/ook](https://www.splitbrain.org/services/ook) 

![在线 Ook 转换器把它转回文本，得到 flag{N7F5_AD5](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193658937.png)

```bash
flag{N7F5_AD5
```

4、查看key_part_two目录下的where_is_flag_part_two.txt:flag_part_two_is_here.txt文件。

![key_part_two 里还有个 NTFS 数据流 where_is_flag_part_two.txt:flag_part_two_is_here.txt，cat 出来满屏 Ook](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193700324.png)

```bash
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook! Ook? Ook! Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook? Ook. Ook?
Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook? Ook! Ook!
Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook? Ook. Ook? Ook! Ook. Ook? Ook.
Ook. Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook! Ook? Ook! Ook! Ook. Ook? Ook! Ook! Ook! Ook! Ook!
Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook? Ook. Ook? Ook! Ook. Ook?
Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook? Ook! Ook! Ook. Ook? Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook? Ook. Ook? Ook! Ook.
Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook!
Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook? Ook! Ook! Ook. Ook?
Ook. Ook. Ook. Ook. Ook. Ook. Ook? Ook. Ook? Ook! Ook. Ook? Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook. Ook! Ook! Ook! Ook!
Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook. Ook! Ook. Ook?
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook? Ook! Ook! Ook. Ook? Ook. Ook.
Ook. Ook. Ook. Ook. Ook? Ook. Ook? Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook!
Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook? Ook! Ook! Ook. Ook? Ook! Ook! Ook!
Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook!
Ook? Ook. Ook? Ook! Ook. Ook? Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook! Ook!
Ook! Ook! Ook! Ook! Ook! Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook! Ook? Ook!
Ook! Ook. Ook? Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook? Ook. Ook? Ook! Ook. Ook? Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook. Ook.
Ook. Ook. Ook. Ook. Ook! Ook. Ook? Ook.
```

这是 **Ook!编码** ，使用在线前一个在线网站进行解密，得到后半部分的flag。
[在线解密：https://www.splitbrain.org/services/ook](https://www.splitbrain.org/services/ook) 
 ![再转一次，拿到 _i5_funny!}，和前半段接上就是完整 flag](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193702690.png)

```bash
_i5_funny!}
```

将两部分组合起来，得到最终的flag。

### flag：

```bash
flag{N7F5_AD5_i5_funny!}
```