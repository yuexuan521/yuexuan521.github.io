---
title: "码蹄杯语言基础：预处理命令（C语言）"
date: 2023-05-28 16:42:09
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

## ⭐MT1100带参数的宏

请编写一个简单程序，把f(x)=(x*x)定义成带参数的宏，计算f(9)/f(6)并输出结果。

格式
输入格式：
无

输出格式：
输出为实型

```c
#include<stdio.h>
#define f(x) ((x)*(x))
int main()
{
    printf("%lf\n", f(9.0) / f(6.0));
    return 0;
}

```

## ⭐MT1101带参数的宏II

请编写一个简单程序，把f(x)=x*(x-1)定义成带参数的宏，从键盘输入a，b，将a+b的和作为宏实参计算并输出结果。

格式
输入格式：
输入整型，空格分隔。

输出格式：
输出为实型

```c
#include<stdio.h>
#define f(x) ((x)*((x)-1))
int main()
{
    int a, b;
    scanf("%d %d", &a, &b);
    printf("%lf\n", (double)f(a + b));
    return 0;
}

```

## ⭐MT1102长方体

将长方体体积计算公式定义为宏。在主函数中输入长方体长、宽、高求体积。不考虑不合理的输入或是溢出等特殊情况。

格式
输入格式：
输入为实型（正数），空格分隔。

输出格式：
输出为实型

```c
#include<stdio.h>
#define VOLUME(x, y, z) ((x)*(y)*(z))
int main()
{
    double x, y, z;
    scanf("%lf %lf %lf", &x, &y, &z);
    printf("%lf\n", VOLUME(x, y, z));
    return 0;
}
```

## ⭐MT1103球体积

将球体积计算公式定义为宏。在主函数中输入半径求体积。

格式
输入格式：
输入为实型

输出格式：
输出为实型

```c
#include<stdio.h>
#define PI 3.14159
#define VOLUME(r) (4.0/3.0*PI*(r)*(r)*(r))
int main()
{
    double r;
    scanf("%lf", &r);
    printf("%lf\n", VOLUME(r));
    return 0;
}
```

## ⭐MT1105英寸英尺英里

定义关于长度的宏，英寸/厘米、英尺/米、英里/公里，计算英制与公制单位转换，在主函数中输入数据输出计算结果。假定1英寸=2.54厘米、1英尺=0.31米、1英里=1.61公里。

格式
输入格式：
输入英寸、英尺、英里为实型，空格分隔。

输出格式：
输出厘米、米、公里为实型，空格分隔。保留2位小数。

```c
#include<stdio.h>
#define INCH_cm(x) ((x) * 2.54)
#define FEET_m(x) ((x) * 0.31)
#define MILE_km(x) ((x) * 1.61)
int main()
{
    double a, b, c;
    scanf("%lf %lf %lf", &a, &b, &c);
    printf("%.2lf %.2lf %.2lf\n", INCH_cm(a), FEET_m(b), MILE_km(c));
    return 0;
}
```

## ⭐MT1107加仑/升

定义关于容量的宏，加仑/升，计算单位转换，在主函数中输入数据输出计算结果。

格式
输入格式：
输入加仑为实型。

输出格式：
输出升为实型，保留2位小数。

```c
#include<stdio.h>
#define GALLON_LITRE(x) ((x) * 3.79)
int main()
{
    double x;
    scanf("%lf", &x);
    printf("%.2lf\n", GALLON_LITRE(x));
    return 0;
}
```