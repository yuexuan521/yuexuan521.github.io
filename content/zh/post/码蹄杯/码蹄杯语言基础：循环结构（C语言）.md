---
title: "码蹄杯语言基础：循环结构（C语言）"
date: 2023-05-30 16:25:24
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "算法"
- "c++"
- "数学"
- "开发语言"
---

码蹄集网站地址：https://www.matiji.net/exam/ojquestionlist

## ⭐MT1185while循环

请编写一个简单程序，从小到大输出所有小于8的正整数和0（从0开始输出）。

格式
输入格式：
无

输出格式：
输出整型，空格分隔

```c
#include<stdio.h>
int main()
{
    int i = 0;
    while (i < 8)
    {
        printf("%d ", i);
        i++;
    }
    return 0;
}
```

## ⭐MT1186do-while循环

请编写一个简单程序，从大到小输出所有小于n的正整数，直到0为止(不含0)。n从键盘输入

格式
输入格式：
输入整型数n

输出格式：
输出整型，空格分隔

```c
#include<stdio.h>
int main()
{
    int n;
    scanf("%d", &n);
    do
    {
        printf("%d ", n);
        n--;
    } while (n);
    return 0;
}
```

## ⭐MT1187累加和

从1累加到10，输出累加和

格式
输入格式：
无

输出格式：
输出整型

```c
#include<stdio.h>
int main()
{
    int i;
    int sum = 0;
    for (i = 0; i <= 10; i++)
    {
        sum += i;
    }
    printf("%d", sum);
    return 0;
}
```

## ⭐MT1188平均值

请编写一个简单程序，随机输入n个数字，输出他们的平均值

格式
输入格式：
输入分两行，第一行输入n，第二行输入n个float型数据，空格分隔

输出格式：
输出float型，空格分隔，保留2位小数

```c
#include<stdio.h>
int main()
{
    int n, i;
    float temp, sum = 0;
    scanf("%d", &n);
    for (i = 0; i < n; i++)
    {
        scanf("%f", &temp);
        sum += temp;
    }
    printf("%.2f", sum / n);
    return 0;
}
```

## ⭐MT1189正数负数的和

编写程序先输入n，再输入n个实数并分别统计正数的和、负数的和，然后输出统计结果。

格式
输入格式：
输入分两行，第一行输入整数n，第二行输入n个实数，空格分隔。

输出格式：
输出正数的和，和负数的和，实型，中间一个空格

```c
#include<stdio.h>
int main()
{
    int n, i;
    double temp, p_sum = 0, n_sum = 0;
    scanf("%d", &n);
    for (i = 0; i < n; i++)
    {
        scanf("%lf", &temp);
        if (temp >= 0)
        {
            p_sum += temp;
        }
        else
        {
            n_sum += temp;
        }
    }
    printf("%lf %lf", p_sum, n_sum);
    return 0;
}
```

## ⭐MT1190分数乘法

输入5组分数，对他们进行乘法运算，输出结果。不考虑分母为0等特殊情况。

格式
输入格式：
输入整型，每组一行，如样例所示。

输出格式：
输出计算结果实型，如样例所示。

```c
#include<stdio.h>
int main()
{
    double x1, y1, x2, y2, i, result;
    for (i = 0; i < 5; i++)
    {
        scanf("%lf/%lf %lf/%lf", &x1, &y1, &x2, &y2);
        result = (x1 / y1) * (x2 / y2);
        printf("%lf\n", result);
    }
    return 0;
}
```

## ⭐MT1191减半

输入两个值N和M，输出N做M次减半后的值。比如100，减半后依次为50, 25, 12…，减半3次后是12。输入不考虑0，负数或者其他特殊情况。

格式
输入格式：
输入为整型，空格分隔

输出格式：
输出为整型

```c
#include<stdio.h>
int main()
{
    int N, M;
    scanf("%d %d", &N, &M);
    for (int i = 0; i < M; i++)
    {
        N /= 2;
    }
    printf("%d", N);
    return 0;
}
```

## ⭐MT1192翻倍

输入两个值N和M。输出N做M次翻倍后的值。比如12，翻倍后依次为24, 48, 96…。输入不考虑0，负数或者其他特殊情况。

格式
输入格式：
输入为整型，空格分隔

输出格式：
输出为整型

```c
#include<stdio.h>
int main()
{
    int N, M;
    scanf("%d %d", &N, &M);
    for (int i = 0; i < M; i++)
    {
        N *= 2;
    }
    printf("%d", N);
    return 0;
}
```

## ⭐MT1193偶数的平方和

输入正整数N，求前N个偶数的平方和。不考虑溢出。

格式
输入格式：
输入正整数N

输出格式：
输入整型

```c
#include<stdio.h>
int main()
{
    int N, i, sum = 0;
    scanf("%d", &N);
    for (i = 2; i <= 2 * N; i)
    {
        sum += i * i;
        i += 2;
    }
    printf("%d\n", sum);
    return 0;
}
```

## ⭐MT1194奇数的平方和

输入正整数N，求前N个奇数的平方和。不考虑溢出。

格式
输入格式：
输入正整数N

输出格式：
输入整型

```c
#include<stdio.h>
int main()
{
    int N, i, sum = 0;
    scanf("%d", &N);
    for (i = 1; i <= 2 * N - 1; i)
    {
        sum += i * i;
        i += 2;
    }
    printf("%d\n", sum);
    return 0;
}
```