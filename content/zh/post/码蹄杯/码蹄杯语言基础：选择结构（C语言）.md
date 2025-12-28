---
title: "码蹄杯语言基础：选择结构（C语言）"
date: 2023-05-28 16:56:02
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "c++"
- "蓝桥杯"
- "算法"
- "数据结构"
---

## ⭐MT1109和10相比

请编写一个简单程序，输入一个整数，和10比较，输出比较结果

格式
输入格式：
输入整型

输出格式：
输出…大于或者等于或者小于10

```c
#include<stdio.h>
int main()
{
    int x;
    scanf("%d", &x);
    if (x > 10)
    {
        printf("%d大于10", x);
    }
    else if (x < 10)
    {
        printf("%d小于10", x);
    }
    else
    {
        printf("%d等于10", x);
    }
    return 0;
}
```

## ⭐MT1110最小值

输入a,b两个整数，输出他们之间的最小值

格式
输入格式：
输入2个整数用空格分隔

输出格式：
输出为整型

```c
#include<stdio.h>
int main()
{
    int a, b;
    scanf("%d %d", &a, &b);
    if (a > b)
    {
        printf("%d", b);
    }
    else
    {
        printf("%d", a);
    }
    return 0;
}
```

## ⭐MT1111最大值

输入a,b两个整数，输出他们之间的最大值

格式
输入格式：
输入2个整数用空格分隔

输出格式：
输出为整型

```c
#include<stdio.h>
int main()
{
    int a, b;
    scanf("%d %d", &a, &b);
    if (a > b)
    {
        printf("%d", a);
    }
    else
    {
        printf("%d", b);
    }
    return 0;
}
```

## ⭐MT1112中庸之道

请编写一个简单程序，输入3个整数，比较他们的大小，输出中间的那个数

格式
输入格式：
输入整型，空格分隔

输出格式：
输出整型

```c
#include<stdio.h>
int main()
{
    int a, b, c;
    scanf("%d %d %d", &a, &b, &c);
    if ((a > b && b > c) || (a < b && b < c))
    {
        printf("%d", b);
    }
    else if ((c > a && a > b) || (c < a && a < b))
    {
        printf("%d", a);
    }
    else
    {
        printf("%d", c);
    }
    return 0;
}

```

## ⭐MT1114偶数还是奇数

请编写一个简单程序，检查一个正整数是偶数还是奇数，如果是偶数输出Y，否则输出N。（不考虑0）

格式
输入格式：
输入整型

输出格式：
输出Y或者N

```c
#include<stdio.h>
int main()
{
    int x;
    scanf("%d", &x);
    if (x % 2 == 0)
    {
        printf("Y");
    }
    else
    {
        printf("N");
    }
    return 0;
}
```

## ⭐MT1115小于m的偶数

判断n是否为小于m的偶数，不考虑0，负数或者其他特殊情况。

格式
输入格式：
输入为整型n、m，空格分隔

输出格式：
是则输出YES否则输出NO

```c
#include<stdio.h>
int main()
{
    int n, m;
    scanf("%d %d", &n, &m);
    if (n < m && n % 2 == 0)
    {
        printf("YES");
    }
    else
    {
        printf("NO");
    }
    return 0;
}
```

## ⭐MT1116正整数

判断n是否为两位数的正整数

格式
输入格式：
输入为整型n

输出格式：
是则输出YES否则输出NO

```c

#include<stdio.h>
int main()
{
    int x;
    scanf("%d", &x);
    if ((x >= 10) && (x <= 99))
    {
        if (x >= 0)
        {
            printf("YES");
        }
        else
        {
            printf("NO");
        }
    }
    else
    {
        printf("NO");
    }
    return 0;
}

```

## ⭐MT1117两个负数

判断x、y、z中是否有两个负数。

格式
输入格式：
输入为整型x、y、z，空格分隔

输出格式：
是则输出YES否则输出NO

```c
#include<stdio.h>
int main()
{
    int x, y, z;
    scanf("%d %d %d", &x, &y, &z);
    if ((x < 0 && y < 0) || (x < 0 && z < 0) || (z < 0 && y < 0))
    {
        printf("YES");
    }
    else
    {
        printf("NO");
    }
    return 0;
}
```

## ⭐MT1119大小写的转换

请编写一个简单程序，实现输入字符大小写的转换。其他非法输入（非字母的输入）则原样输出。

格式
输入格式：
输入字符型

输出格式：
输出字符型

```c
#include<stdio.h>
#include<ctype.h>
int main()
{
    char str;
    scanf("%c", &str);
    if (isalpha(str))
    {
        if (str >= 65 && str <= 90)
        {
            str += 32;
            printf("%c", str);
        }
        else
        {
            str -= 32;
            printf("%c", str);
        }
    }
    else
    {
        printf("%c", str);
    }
    return 0;
}

```

## ⭐MT1121小码哥考完咯

小码哥考完咯，你是她的老师，请使用switch语句编写一个程序，输出她的分数对应的成绩等级ABCDF。使用以下分级标准：A=90-100，B=80-89，C=70-79，D=60-69，F=0-59。不考虑负数或者其他特殊情况。本题要求使用switch语句。

格式
输入格式：
输入为整型

输出格式：
输出为字符型

```c
#include<stdio.h>
int main()
{
    int grade;
    scanf("%d", &grade);
    grade = grade / 10;
    switch (grade)
    {
    case 10:
        printf("A");
        break;
    case 9:
        printf("A");
        break;
    case 8:
        printf("B");
        break;
    case 7:
        printf("C");
        break;
    case 6:
        printf("D");
        break;
    default:
        printf("F");
    }
    return 0;
}

```