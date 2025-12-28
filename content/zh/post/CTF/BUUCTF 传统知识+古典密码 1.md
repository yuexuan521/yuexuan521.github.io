---
title: "BUUCTF 传统知识+古典密码 1"
date: 2024-09-25 21:45:45
category: "BUUCTF Crypto"
tags:
- "密码"
- "网络安全"
- "CTF"
- "CRTPTO"
- "Crypto"
- "安全"
- "密码学"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228204959212.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205001342.png)

### 题目描述：

小明某一天收到一封密信，信中写了几个不同的年份
辛卯，癸巳，丙戌，辛未，庚辰，癸酉，己卯，癸巳。
信的背面还写有“+甲子”，请解出这段密文。

key值：CTF{XXX}

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205002953.jpeg)

---

### 解题思路：

1、理解题目，出现“辛卯，癸巳，丙戌，辛未，庚辰，癸酉，己卯，癸巳。”这样的传统年份，同时，题目中出现“+甲子”的信息（古代人认为六十年为一个甲子），“+甲子”意味着+60，猜测与ASCII码有关。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205004510.jpeg)

古典密码学主要有两大基本方法：

①置换密码(又称易位密码)：明文的字母保持相同，但顺序被打乱了。栏栅密码

②代替密码：就是将明文的字符替换为密文中的另一种的字符，接收者只要对密文做反向替换就可以恢复出明文。凯撒密码

2、将“辛卯，癸巳，丙戌，辛未，庚辰，癸酉，己卯，癸巳。”与1、2、3、4…进行对照，得到“28，30，23，8，17，10，16，30，8”一串数字，加上一甲子，给数字+60，得到“88，90，83，68，77，70，76，90”，对照ASCII码表，得到“XZSDMFLZ”的字符串。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205006290.png)

3、接下来进行古典加密，对得到的字符串进行栏栅加密，分别进行2和4的解密。
[栏栅密码加密解密](https://www.qqxiuzi.cn/bianma/zhalanmima.php) 

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205008126.png)

4、得到密文“XMZFSLDZ”，进行凯撒加密，执行下列Python代码（进行小写转换，便于识别有效结果）。

```python
def decrypt(ciphertext, shift):
    """移位解密函数"""
    plaintext = ''
    for char in ciphertext:
        if char.isalpha(): # 如果是字母，进行移位解密
            if char.isupper():
                plaintext += chr((ord(char) - shift - 65) % 26 + 65) # 大写字母移位解密
            else:
                plaintext += chr((ord(char) - shift - 97) % 26 + 97) # 小写字母移位解密
        else: # 如果不是字母，直接输出
            plaintext += char
    return plaintext

# 加密密文和移位数
ciphertext = 'XMZFSLDZ'
shift = 3
ciphertext = ciphertext.lower()
# 小写易于观察
# 枚举所有可能的移位数，输出所有解密结果
for i in range(26):
    plaintext = decrypt(ciphertext, i)
    print("%d %s"% (i, plaintext))
```

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228205009726.png)

5、偏移量为5的字符串“shuangyu”，为有效结果，还原大写字母，作为结果。

### flag：

```
SHUANGYU
```