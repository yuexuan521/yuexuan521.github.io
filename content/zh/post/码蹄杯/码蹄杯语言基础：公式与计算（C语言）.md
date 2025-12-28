---
title: "码蹄杯语言基础：公式与计算（C语言）"
date: 2025-03-03 09:30:02
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "算法"
- "开发语言"
- "数据结构"
- "码蹄杯"
- "C++"
---

码蹄集网站地址：https://www.matiji.net/exam/ojquestionlist

## ⭐MT1041求圆面积和周长

请编写一个简单程序，输入半径，输出圆面积和周长。（PI是3.1415926）

格式
输入格式：
double型

输出格式：
分2行输出圆面积和周长，保留6位小数

```c
#include<stdio.h>
int main()
{
    double x, area, perimeter;
    double PI = 3.1415926;
    scanf("%lf", &x);
    area = PI * x * x;
    perimeter = 2 * PI * x;
    printf("Area=%.6lf\nCircumference=%.6lf", area, perimeter);
    return 0;
}

```

## ⭐MT1042求矩形的面积和周长

请编写一个简单程序，输入矩形的长度和宽度，输出矩形的面积和周长。

格式
输入格式：
实型，空格分隔

输出格式：
分2行输出矩形的面积和周长，保留6位小数

```c
#include<stdio.h>
int main()
{
    double x, y;
    scanf("%lf %lf", &x, &y);
    printf("Area=%.6lf\nPerimeter=%.6lf", x * y, 2 * (x + y));
    return 0;
}
```

## ⭐MT1043椭圆计算

请编写一个简单程序，输入长半轴和短半轴长度，计算输出椭圆的面积。（PI是3.1415926）

格式
输入格式：
double型，空格分隔

输出格式：
输出椭圆的面积，保留6位小数

```c
#include<stdio.h>
int main()
{
    double a, b;
    double PI = 3.1415926;
    scanf("%lf %lf", &a, &b);
    printf("Area = %.6lf", PI * a * b);
    return 0;
}
```

## ⭐MT1044三角形面积

请编写一个简单程序，计算给定底面和高度的三角形面积。

格式
输入格式：
输入float型，空格分隔

输出格式：
输出三角形面积，保留2位小数

```c
#include<stdio.h>
int main()
{
    float a, b;
    scanf("%f %f", &a, &b);
    printf("Area=%.2f", 1.0 / 2.0 * a * b);
    return 0;
}
```

## ⭐MT1045平行四边形

请编写一个简单程序，输入平行四边形底和高，输出平行四边形面积。不考虑非法输入。

格式
输入格式：
输入实型，空格分隔。

输出格式：
输出实型

```c
#include<stdio.h>
int main()
{
    double a, b;
    scanf("%lf %lf", &a, &b);
    printf("%lf", a * b);
    return 0;
}
```

## ⭐MT1046菱形

输入菱形的两个对角线的长度，输出菱形面积。

格式
输入格式：
输入实型，空格分隔。

输出格式：
输出实型，保留2位小数。

```c
#include<stdio.h>
int main()
{
    double a, b;
    scanf("%lf %lf", &a, &b);
    printf("%.2lf", 1.0 / 2.0 * a * b);
    return 0;
}

```

## ⭐MT1047梯形

输入梯形的两个底的长度和高，输出梯形面积。

格式
输入格式：
输入实型，空格分隔。

输出格式：
输出实型，保留2位小数。

```c
#include<stdio.h>
int main()
{
    double a, b, h;
    scanf("%lf %lf %lf", &a, &b, &h);
    printf("%.2lf", (a + b) * h / 2.0);
    return 0;
}
```

## ⭐MT1049三角形坐标

输入三角形三个顶点A，B，C的坐标（x,y），根据公式计算并输出三角形面积。
S=1/2 * |x1y2+x2y3+x3y1-x1y3-x2y1-x3y2|

格式
输入格式：
依次输入三个顶点A，B，C的坐标（x,y），整型，空格分隔。

输出格式：
输出实型，保留2位小数。

```c
#include<stdio.h>
int main()
{
    int x1, y1, x2, y2, x3, y3;
    double S;
    scanf("%d %d %d %d %d %d", &x1, &y1, &x2, &y2, &x3, &y3);
    if (x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2 >= 0)
    {
        S = 1.0 / 2.0 * (x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2);
    }
    else
    {
        S = -1.0 / 2.0 * (x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2);
    }
    printf("%.2lf", S);
    return 0;
}
```

## ⭐MT1050空间三角形

输入在三维空间的三角形三个顶点A，B，C的坐标（x,y,z），计算并输出三角形面积。不考虑不能构成三角形的特殊情况。

格式
输入格式：
依次输入三个顶点A，B，C的坐标（x,y,z），整型，空格分隔。

输出格式：
输出实型，保留2位小数。

```c
#include<stdio.h>
#include<math.h>
int main()
{
    int x1, y1, z1, x2, y2, z2, x3, y3, z3, a, b, c;
    double S, A, B, C, P;
    scanf("%d %d %d %d %d %d %d %d %d %d", &x1, &y1, &z1, &x2, &y2, &z2, &x3, &y3, &z3);
    a = (x1 - x2) * (x1 - x2) + (y1 - y2) * (y1 - y2) + (z1 - z2) * (z1 - z2);
    b = (x3 - x2) * (x3 - x2) + (y3 - y2) * (y3 - y2) + (z3 - z2) * (z3 - z2);
    c = (x1 - x3) * (x1 - x3) + (y1 - y3) * (y1 - y3) + (z1 - z3) * (z1 - z3);
    A = sqrt(a);
    B = sqrt(b);
    C = sqrt(c);
    P = (A + B + C) / 2.0;
    S = sqrt(P * (P - A) * (P - B) * (P - C));
    printf("%.2lf", S);
    return 0;
}

```

## ⭐MT1051四边形坐标

输入四边4个顶点A，B，C，D的坐标（x,y），计算并输出四边形面积。

格式
输入格式：
依次输入4个顶点A，B，C，D的坐标（x,y），四边形一定是凸四边形，整型，空格分隔。

输出格式：
输出实型，保留2位小数。

```c
// #include<stdio.h>
// int main() 
// {
//     int x1, y1, x2, y2, x3, y3, x4, y4, X1, X2;
//     double S;
//     scanf("%d %d %d %d %d %d %d %d", &x1, &y1, &x2, &y2, &x3, &y3, &x4, &y4);
//     X1 = x1*y2+x2*y3+x3*y1-x1*y3-x2*y1-x3*y2;
//     X2 = x1*y4+x4*y3+x3*y1-x1*y3-x4*y1-x3*y4;
//     if (X1 >= 0 && X2 >= 0)
//     {
//         S=1.0/2.0*(X1+X2);
//     }
//     else
//     {
//         S=-1.0/2.0*(X1+X2);
//     }
//     printf("%.2lf", S);
//     return 0; 
// }

#include<stdio.h>
int main()
{
    int x1, y1, x2, y2, x3, y3, x4, y4;
    double S;
    scanf("%d %d %d %d %d %d %d %d", &x1, &y1, &x2, &y2, &x3, &y3, &x4, &y4);
    if (x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2 >= 0)
    {
        S = 1.0 / 2.0 * (x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2);
    }
    else
    {
        S = -1.0 / 2.0 * (x1 * y2 + x2 * y3 + x3 * y1 - x1 * y3 - x2 * y1 - x3 * y2);
    }
    if (x2 * y3 + x3 * y4 + x4 * y2 - x2 * y4 - x3 * y2 - x4 * y3 >= 0)
    {
        S = 1.0 / 2.0 * (x2 * y3 + x3 * y4 + x4 * y2 - x2 * y4 - x3 * y2 - x4 * y3) + S;
    }
    else
    {
        S = -1.0 / 2.0 * (x2 * y3 + x3 * y4 + x4 * y2 - x2 * y4 - x3 * y2 - x4 * y3) + S;
    }
    printf("%.2lf", S);
    return 0;
}
```