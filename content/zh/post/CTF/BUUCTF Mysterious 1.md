---
title: "BUUCTF Mysterious 1"
date: 2024-09-21 20:20:58
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "网络安全"
- "IDA"
- "逆向"
- "CTF"
- "汇编"
- "BUUCTF"
- "MISC"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190029799.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190031828.png)

### 题目描述：

自从报名了CTF竞赛后，小明就辗转于各大论坛，但是对于逆向题目仍是一知半解。有一天，一个论坛老鸟给小明发了一个神秘的盒子，里面有开启逆向思维的秘密。小明如获至宝，三天三夜，终于解答出来了，聪明的你能搞定这个神秘盒子么？ 注意：得到的 flag 请包上 flag{} 提交

### 密文：

下载附件，得到一个.exe文件。
 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190033728.png)

---

### 解题思路：

1、双击执行文件，出现如下界面，随便输入一些内容，没有回显。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190035734.png)

根据题目描述，本题应该与逆向存在关系，用记事本打开.exe文件，查看这个exe文件是32位还是64位程序。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190037106.png)

> 第二行不远处有PE两个字母，再后面两个空格后第三个字符就是标记了，如果是字母L的话，就是32位应用程序，如果是d?就表示是64位应用程序。

`PE..L..` 是32位exe文件特征， `PE..d?..` 是64位exe文件特征。

2、使用IDA打开exe文件，（在这一步之前可以使用查壳工具进行查壳），使用快捷键shift+F12查看字符串，寻找有用的信息。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190039114.png)

双击 `well done` ，找到与flag相关的字符串，定位关键函数。

![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190040987.png)

使用快捷键ctrl+x（交叉引用）查看那段函数调用了该字符串，点击“OK”，进入该段函数，查看相关汇编代码。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190042783.png)

使用快捷键F5查看该汇编代码的伪C代码。( “well done!” )

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190044376.png)

```c
int __stdcall sub_401090(HWND hWnd, int a2, int a3, int a4)
{
  int v4; // eax
  char Source[260]; // [esp+50h] [ebp-310h] BYREF
  _BYTE Text[257]; // [esp+154h] [ebp-20Ch] BYREF
  __int16 v8; // [esp+255h] [ebp-10Bh]
  char v9; // [esp+257h] [ebp-109h]
  int Value; // [esp+258h] [ebp-108h]
  CHAR String[260]; // [esp+25Ch] [ebp-104h] BYREF

  memset(String, 0, sizeof(String));
  Value = 0;
  if ( a2 == 16 )
  {
    DestroyWindow(hWnd);
    PostQuitMessage(0);
  }
  else if ( a2 == 273 )
  {
    if ( a3 == 1000 )
    {
      GetDlgItemTextA(hWnd, 1002, String, 260);
      strlen(String);
      if ( strlen(String) > 6 )
        ExitProcess(0);
      v4 = atoi(String);
      Value = v4 + 1;
      if ( v4 == 122 && String[3] == 120 && String[5] == 122 && String[4] == 121 )
      {
        strcpy(Text, "flag");
        memset(&Text[5], 0, 0xFCu);
        v8 = 0;
        v9 = 0;
        _itoa(Value, Source, 10);
        strcat(Text, "{");
        strcat(Text, Source);
        strcat(Text, "_");
        strcat(Text, "Buff3r_0v3rf|0w");
        strcat(Text, "}");
        MessageBoxA(0, Text, "well done", 0);
      }
      SetTimer(hWnd, 1u, 0x3E8u, TimerFunc);
    }
    if ( a3 == 1001 )
      KillTimer(hWnd, 1u);
  }
  return 0;
}
```

3、分析这段代码，当满足

```c
 if ( v4 == 122 && String[3] == 120 && String[5] == 122 && String[4] == 121 )
```

时，程序就会返回flag。同时，要满足

```c
 if ( strlen(String) > 6 )
        ExitProcess(0);
        //ExitProcess(0); 如果字符串长度大于6，则调用系统API ExitProcess(0) 结束当前进程，其中参数0通常表示正常退出。
```

输入的内容string必须小于等于6。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190046202.png)

那么，当v4= `122` ，而String[3]、String[4]、String[5]分别对应ASCII字符 `x、y、z` 时，满足要求返回flag。

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190047633.png)

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228190049042.png)

### flag：

```bash
flag{123_Buff3r_0v3rf|0w}
```