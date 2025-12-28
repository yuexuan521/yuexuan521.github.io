---
title: "BUUCTF [GUET-CTF2019]zips 1"
date: 2024-09-21 20:14:36
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "安全"
- "密码学"
- "zip"
- "掩码爆破"
- "zip伪加密"
- "BUUCTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191059911.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![image-20240521144712363](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191101989.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

得到一个attachment.zip文件

---

### 解题思路：

1、解压attachment.zip，得到222.zip文件。尝试解压需要密码，使用Ziperello爆破密码，先尝试1~9位纯数字暴力破解，得到密码723456

```

```

 ![屏幕截图 2024-05-19 164506](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191103382.png)

2、解压222.zip，得到111.zip文件。使用Ziperello打开111.zip文件，提示没有读取到加密文件，猜测存在zip伪加密。

 ![屏幕截图 2024-05-19 164527](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191105384.png)

 ![屏幕截图 2024-05-19 164638](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191107184.png)

[zip伪加密原理](https://blog.csdn.net/qq_26187985/article/details/83654197) 

[zip伪加密例子](https://blog.csdn.net/YueXuan_521/article/details/134055375?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522171627454816800197088397%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=171627454816800197088397&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_ecpm_v1~rank_v31_ecpm-1-134055375-null-null.nonecase&utm_term=%E4%BC%AA%E5%8A%A0%E5%AF%86&spm=1018.2226.3001.4450) 

使用010Editor打开111.zip文件，修改压缩源文件数据区和目录区的全局方式位标记，达到将伪压缩文件恢复到未加密的状态的目的。

 ![image-20240521151732976](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191109292.png)

解压得到一个zip压缩包和脚本文件

 ![image-20240521152003008](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191111146.png)

3、尝试解压flag.zip需要密码。使用记事本打开setup.sh文件，如下：

```
#!/bin/bash
#
zip -e --password=`python -c "print(__import__('time').time())"` flag.zip flag
```

> 
> 
> 1. `#!/bin/bash` : 这是一个Shebang行，指定了该脚本应使用 `/bin/bash` 解释器执行。它是Unix/Linux系统中可执行脚本的标准起始行。
> 
> 2. `zip -e --password=` : 这部分命令调用了 `zip` 程序来创建或更新一个ZIP归档文件，并使用 `-e` 选项指明需要对存档中的文件进行加密。
> 
> 3. `python -c "print(__import__('time').time())"` : 这里嵌入了一个Python命令，用于执行一段Python代码。具体来说，它导入了 `time` 模块，并调用其 `time()` 函数来获取当前的Unix时间戳。这个时间值将作为接下来操作的密码。
> 
> 4. `flag.zip flag` : 表示要创建或更新的ZIP文件名为 `flag.zip` ，并且要将当前目录下的一个名为 `flag` 的文件添加到此ZIP文件中。由于前面设置了 `-e` 和 `--password` ，所以在添加过程中会对 `flag` 文件进行加密，并使用由Python计算出的时间戳作为加密密码。
> 
> 

这段脚本是用Bash编写的，其主要功能是使用Python计算当前时间（以Unix时间戳形式表示，即从1970年1月1日00:00:00 UTC以来的秒数）并以此时间为密码来加密一个名为 `flag.zip` 的ZIP文件，其中包含一个名为 `flag` 的文件。

截取其中的 `print(__import__('time').time())` python代码，在Python2环境下运行，得到时间戳格式（Python2与Python3得到的时间戳格式不一样）

```
1716272025.41
```

 ![image-20240521153241328](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191112198.png)

当我们知道密码格式后就可以采用掩码爆破节约时间，使用Ziperello进行掩码爆破，首先定义掩码字符模板，再设置密码模板，选择起始密码就可以开始爆破。（这里我已经知道大致密码，所以为了节约时间，从1500000000.00开始）

 ![image-20240521153442653](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191114100.png)

得到密码1558080832.15

 ![image-20240521152729804](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191116037.png)

```
1558080832.15
```

使用密码解压flag.zip，得到flag文件，打开得到flag。

 ![image-20240521152847067](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191119379.png)

### flag：

```bash
flag{fkjabPqnLawhvuikfhgzyffj}
```