---
title: "BUUCTF [MRCTF2020]Hello_ misc 1"
date: 2025-08-04 08:00:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "MRCTF2020"
- "BUUCTF"
- "网络安全"
- "安全"
- "密码学"
- "Base64隐写"
- "CTF"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191514677.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[神奇的 Base64 隐写](https://www.tr0y.wang/2017/06/14/Base64steg/) 
[MRCTF复现](https://blog.csdn.net/weixin_44377940/article/details/105206585) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191516830.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。
感谢Galaxy师傅供题。

### 密文：

下载附件解压，得到hello文件夹，两个文件

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191518410.png)

---

### 解题思路：

1、解压flag.rar需要密码，先处理try to restore it.png图片。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191520430.png)

盲猜LSB隐写，用StegSolve打开，在Red 0通道发现PNG图片数据。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191522008.png)

保存为png文件，得到：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191523571.png)

得到zip-passwd密码，但不是flag.rar的密码。

2、用010Editor打开try to restore it.png图片，在文件尾找到 `PK` 文件头，提取出来保存为zip文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191525518.png)

使用刚得到的密码解压，得到out.txt，文件内容如下：

```bash
127
255
63
191
127
191
63
127
127
255
63
191
63
191
255
127
127
255
63
63
127
191
63
127
127
255
63
255
127
255
63
255
127
255
127
255
127
191
127
63
63
255
191
191
63
255
63
63
127
191
63
127
127
191
63
255
63
255
63
127
127
191
127
191
127
191
127
127
63
255
127
191
127
191
63
191
63
255
127
255
63
255
127
255
127
191
63
191
127
191
127
127
63
255
127
127
127
191
127
63
127
191
63
191
127
191
127
127
```

一眼TTL隐写，解密脚本如下：

```python
# @Author：YueXuan
# @Date  ：2024/10/10 16:56

def read_numbers_from_file(filename):
    """从文件中读取数字列表"""
    with open(filename, 'r') as file:
        numbers = [int(line.strip()) for line in file]
    return numbers

def decode_ttl(numbers):
    """解码数字列表为二进制字符串"""
    binary_str = ''
    mapping = {63: '00', 127: '01', 191: '10', 255: '11'}
    for number in numbers:
        binary_str += mapping[number]
    return binary_str

def binary_to_string(binary_str):
    """将二进制字符串转换为ASCII字符串"""
    text = ''
    for i in range(0, len(binary_str), 8):
        byte = binary_str[i:i+8]
        text += chr(int(byte, 2))
    return text

def main():
    filename = 'out.txt'  # 输入文件名
    output_filename = 'output.txt'  # 输出文件名

    # 从文件读取数字
    numbers = read_numbers_from_file(filename)

    # 解码数字为二进制字符串
    binary_str = decode_ttl(numbers)

    # 将二进制字符串转换为ASCII字符串
    hidden_text = binary_to_string(binary_str)

    # 输出结果
    print("隐藏的文本:", hidden_text)

    # 可选地保存到文件
    with open(output_filename, 'w') as output_file:
        output_file.write(hidden_text)

if __name__ == '__main__':
    main()
```

解密得到 `rar-passwd:0ac1fe6b77be5dbe` ，rar压缩包的密码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191527512.png)

3、用得到的密码解压flag.rar压缩包，得到flag文件夹下的fffflag.zip文件。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191528857.png)

然而它是一个doc文件，更改后缀打开。打开之后，全选内容更改字体颜色，得到一堆密文。（Where is the flag ?）

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191529916.png)

```python
MTEwMTEwMTExMTExMTEwMDExMTEwMTExMTExMTExMTExMTExMTExMTExMTExMTExMTAxMTEwMDAwMDAxMTExMTExMTExMDAxMTAx
MTEwMTEwMTEwMDAxMTAxMDExMTEwMTExMTExMTExMTExMTExMTExMTExMTExMTExMTExMTAxMTExMTExMTExMTExMTEwMTEwMDEx
MTEwMDAwMTAxMTEwMTExMDExMTEwMTExMTExMTAwMDExMTExMTExMTExMDAxMDAxMTAxMTEwMDAwMDExMTExMDAwMDExMTExMTEx
MTEwMTEwMTAwMDAxMTExMDExMTEwMTExMTExMDExMTAxMTExMTExMTEwMTEwMTEwMTAxMTExMTExMTAwMTEwMTExMTExMTExMTEx
MTEwMTEwMTAxMTExMTExMDExMTEwMTExMTAxMDExMTAxMTExMTExMTEwMTEwMTEwMTAxMTAxMTExMTAwMTEwMTExMTExMTExMTEx
MTEwMTEwMTAwMDAxMTAwMDAwMTEwMDAwMDAxMTAwMDExMTAwMDAwMTEwMTEwMTEwMTAxMTEwMDAwMDAxMTExMDAwMDExMTExMTEx
```

密文使用Base64隐写加密，使用下面的解密脚本进行解密

[Base64隐写加解密工具](https://github.com/jerrita/b64steg) 

```shell
python b64steg.py -f flag.txt -s output.txt
```

得到

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191531572.png)

```python
110110111111110011110111111111111111111111111111101110000001111111111001101
110110110001101011110111111111111111111111111111111101111111111111110110011
110000101110111011110111111100011111111111001001101110000011111000011111111
110110100001111011110111111011101111111110110110101111111100110111111111111
110110101111111011110111101011101111111110110110101101111100110111111111111
110110100001100000110000001100011100000110110110101110000001111000011111111
```

4、全选数据，回到fffflag.doc文件中，粘贴进去。然后搜索“ `0` ”，见证奇迹的时刻，你得到了flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191533487.png)

### flag：

```bash
flag{He1Lo_mi5c~}
```