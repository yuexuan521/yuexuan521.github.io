---
title: "BUUCTF 梅花香之苦寒来 1"
date: 2024-09-23 10:52:19
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "安全"
- "CTF"
- "笔记"
- "网络安全"
- "BUUCTF"
- "Misc"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193050688.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193052876.png)

### 题目描述：

注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，解压得到一张.jpg图片。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193054864.jpeg)

---

### 解题思路：

1、用010Editor看了一下，刚开始以为是修改宽高的题，没有想到这个方向。（想到也不会做）
在010 Editor中看到，在图片数据的后面附加了很多的无关数据。（“FF D9”为jpg文件结尾）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193056343.png)

2、将这些数据转换一下，看看是什么文件的数据。将全部的灰色数据复制下来，新建一个txt文本，粘贴进去保存。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193058939.png)

在010 Editor中，使用“文件”选项卡的“导入16进制文件”选项，导入刚才新建的txt文件。

![在这里插入图片描述](./assets/017_6.png)

得到一堆坐标数据，接下来尝试将数据转换为图像。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193107572.png)

（这一步其实就是将16进制的数据转换为ASCII字符，使用任意转换工具都可以，这里提供一个python脚本）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193109380.png)

```python
with open('hex.txt', 'r') as h:     # hex.txt为要转换的文本文件
    val = h.read()
    h.close()

with open('result.txt', 'w') as re: # 转换完成后写入result.txt
    tem = ''
    for i in range(0, len(val), 2):
        tem = '0x' + val[i] + val[i+1]
        tem = int(tem, base=16)
        print(chr(tem), end="")
        re.write(chr(tem))
    re.close()
```

3、使用gnuplot来进行绘制图像（ [gnuplot下载地址](https://pan.baidu.com/s/1VE72XqOErGFQqJeuI3lIug#list/path=/) ，提取码：wel5），安装好gnuplot之后，需要去环境变量（查看高级系统设置）里添加变量，然后就可以在命令行里运行gnuplot了。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193111524.png)

4、使用gnuplot之前需要先将坐标数据格式转换成gnuplot可以识别的格式，下面是Python脚本：

```python
with open('result.txt', 'r') as res:  # 坐标格式文件比如(7,7)
    re = res.read()
    res.close()

with open('gnuplotTxt.txt', 'w') as gnup:  # 将转换后的坐标写入gnuplotTxt.txt
    re = re.split()
    tem = ''
    for i in range(0, len(re)):
        tem = re[i]
        tem = tem.lstrip('(')
        tem = tem.rstrip(')')
        for j in range(0, len(tem)):
            if tem[j] == ',':
                tem = tem[:j] + ' ' + tem[j + 1:]
        gnup.write(tem + '\n')
    gnup.close()
```

转换之后是这样的数据格式。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193113395.png)

5、将gnuplotTxt.txt放到gnuplot.exe的文件夹下，启动gnuplot，在命令行使用如下命令即可绘图。

```bash
plot "gnuplotTxt.txt"
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193114940.png)

得到一张二维码图片，扫描二维码得到flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193116644.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193118944.png)

（原来一开始图片属性就有提示）

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228193120998.png)

### flag：

```bash
flag{40fc0a979f759c8892f4dc045e28b820}
```