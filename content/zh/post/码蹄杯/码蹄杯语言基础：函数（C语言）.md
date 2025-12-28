---
title: "码蹄杯语言基础：函数（C语言）"
date: 2023-06-09 17:05:12
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "c++"
- "算法"
---

码蹄集网站地址： [https://www.matiji.net/exam/ojquestionlist](https://www.matiji.net/exam/ojquestionlist) 

## ⭐MT1328用函数求和

定义一个函数intadd(int x,int y) ，在主函数中输入两个整数a，b，调用add函数求a，b的和，再在主函数中输出和。

格式
输入格式：
输入两个整数a,b，逗号分隔

输出格式：
输出和，整型

```c
#include<stdio.h>
int add(int x, int y)
{
    return x + y;
}
int main()
{
    int a, b;
    scanf("%d,%d", &a, &b);
    printf("%d\n", add(a, b));
    return 0;
}
```

## ⭐MT1329用函数计算公式

编写函数fun,求1＋4＋7＋10＋……n的和。主函数中输入正整数n，输出累加和。比如输入7，则求1＋4＋7的和，如果输入5，则求1＋4的和。

格式
输入格式：
输入整型

输出格式：
输出整型

```c
#include<stdio.h>
int func(int n)
{
    int i, sum = 0;
    for (i = 1; i <= n; i)
    {
        sum += i;
        i += 3;
    }
    return sum;
}
int main()
{
    int n;
    scanf("%d", &n);
    printf("%d\n", func(n));
    return 0;
}

```

## ⭐MT1332用函数求最大值

定义一个函数 ，在主函数中输入4个整数 ，调用函数求最大值，再在主函数中输出。

格式
输入格式：
输入整型，空格分隔

输出格式：
输出整型

```c
#include<stdio.h>
int max(int a, int b, int c, int d)
{
    int max1, max2;
    if (a > b)
    {
        max1 = a;
    }
    else
    {
        max1 = b;
    }
    if (c > d)
    {
        max2 = c;
    }
    else
    {
        max2 = d;
    }
    if (max1 > max2)
    {
        return max1;
    }
    else
    {
        return max2;
    }
}
int main()
{
    int a, b, c, d;
    scanf("%d %d %d %d", &a, &b, &c, &d);
    printf("%d\n", max(a, b, c, d));
    return 0;
}
```

## ⭐MT1333用函数求最小值

定义一个函数 ，在主函数中输入4个整数 ，调用函数求最小值，再在主函数中输出。

格式
输入格式：
输入整型，空格分隔

输出格式：
输出整型

```c
#include<stdio.h>
int min(int a, int b, int c, int d)
{
    int min1, min2;
    if (a < b)
    {
        min1 = a;
    }
    else
    {
        min1 = b;
    }
    if (c < d)
    {
        min2 = c;
    }
    else
    {
        min2 = d;
    }
    if (min1 < min2)
    {
        return min1;
    }
    else
    {
        return min2;
    }
}
int main()
{
    int a, b, c, d;
    scanf("%d %d %d %d", &a, &b, &c, &d);
    printf("%d\n", min(a, b, c, d));
    return 0;
}

```

## ⭐MT1334最小整数

编写函数getceil(x)，返回大于等于x的最小整数，例如getceil(2.8)为3，getceil(-2.8)为-2。

格式
输入格式：
输入为实型

输出格式：
输出为整型

```c
#include<stdio.h>
//正、负实数强制转换成整数后，被截断。例如，（2.3）--》（2），（-1.2）--》（-1）
int getceil(double x)
{
    if (x > 0)
    {
        if (x - (int)x > 0)
        {
            return (int)x + 1;
        }
        else
        {
            return (int)x;
        }
    }
    else
    {
        return (int)x;
    }
}
int main()
{
    double x;
    scanf("%lf", &x);
    printf("%d\n", getceil(x));
    return 0;
}

```

## ⭐MT1335最大整数

编写函数getfloor(x)，返回小于等于x的最大整数，例如getfloor(2.8)为2，getfloor(-2.8)为-3。

格式
输入格式：
输入为实型

输出格式：
输出为整型

```c
#include<stdio.h>
int getfloor(double x)
{
    if (x > 0)
    {
        return (int)x;
    }
    else
    {
        if (x - (int)x < 0)
        {
            return (int)x - 1;
        }
        else
        {
            return (int)x;
        }
    }
}
int main()
{
    double x;
    scanf("%lf", &x);
    printf("%d\n", getfloor(x));
    return 0;
}

```

## ⭐MT1336用函数求阶乘

定义一个函数int fact(int x) ，在主函数中输入正整数a，调用fact函数求a的阶乘，再在主函数中输出阶乘

格式
输入格式：
输入整型

输出格式：
输出整型

```c
#include<stdio.h>
int fact(int x)
{
    int i, sum = 1;
    for (i = 1; i <= x; i++)
    {
        sum *= i;
    }
    return sum;
}
int main()
{
    int x;
    scanf("%d", &x);
    printf("%d", fact(x));
    return 0;
}

```

## ⭐MT1337n次方

编写函数fun，求任一整数m的n次方（n为非负数）。

格式
输入格式：
输入整型，空格分隔

输出格式：
输出整型

```c
#include<stdio.h>
int fun(int m, int n)
{
    int i, result = 1;
    for (i = 0; i < n; i++)
    {
        result *= m;
    }
    return result;
}
int main()
{
    int m, n;
    scanf("%d %d", &m, &n);
    printf("%d\n", fun(m, n));
    return 0;
}
```