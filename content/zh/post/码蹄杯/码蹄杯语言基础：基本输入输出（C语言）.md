---
title: "码蹄杯语言基础：基本输入输出（C语言）"
date: 2023-05-21 12:17:10
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "c++"
- "开发语言"
- "算法"
- "数据结构"
---


码蹄集网站地址： [https://www.matiji.net/exam/ojquestionlist](https://www.matiji.net/exam/ojquestionlist) 

## ⭐MT1001程序设计入门

欢迎来到程序设计的世界，请编写一个简单程序，输出2行字符，第一行为“This is my first program!”，第二行为“Coding is fun!”

格式

输入格式：

无

输出格式：

第一行为“This is my first program!”，第二行为“Coding is fun!”

```c
#include<stdio.h>

int main()
{
    printf("This is my first program!\nCoding is fun!");
    return 0;
}

```

## ⭐MT1002输入和输出整型数据

请编写一个简单程序，用户输入一个整数存储在变量中，并输出在屏幕上。

格式
输入格式：
一个整型数据

输出格式：
输出You entered:刚才输入的整型数据

```c
int main()
{
    int num;
    scanf("%d", &num);
    printf("You entered:%d", num);
    return 0;
}
```

## ⭐MT1003整数运算

请编写一个简单程序，用户输入2个整型数据存储在变量中，并输出他们的和与差。

格式
输入格式：
2个整型数据，用逗号分隔

输出格式：
输出分两行，分别输出他们的和与差

```c
#include<stdio.h>
int main()
{
    int a, b;
    scanf("%d,%d", &a, &b);
    printf("%d+%d=%d\n", a, b, a + b);
    printf("%d-%d=%d", a, b, a - b);
    return 0;
}
```

## ⭐MT1006实型数运算

请编写一个简单程序，用户输入2个实型数据存储在变量中，并输出他们的乘积与商。（本题不考虑除数为0的情况）

格式
输入格式：
2个实型数据，用空格分隔

输出格式：
输出分两行，分别输出他们的乘积与商

```c
#include<stdio.h>
int main()
{
    double a, b;
    scanf("%lf %lf", &a, &b);
    printf("%lf*%lf=%lf\n", a, b, a * b);
    printf("%lf/%lf=%lf", a, b, a / b);

    return 0;
}

```

## ⭐MT1007平均分

输入一名学生的C++、python和C语言成绩，输出总分和和平均分。不考虑不合理的输入或是溢出等特殊情况。

格式
输入格式：
输入为实型，空格分隔

输出格式：
输出为实型，保留6位小数

```c
#include<stdio.h>
int main()
{
    double c_plus, python, c;
    scanf("%lf %lf %lf", &c_plus, &python, &c);
    double sum = c_plus + python + c;
    double ave = sum / 3;
    printf("%.6lf\n%.6lf", sum, ave);
    return 0;
}
```

## ⭐MT1008圆球等的相关计算

请编写一个简单程序，输入半径和高，输出圆周长，圆面积，球面积，球体积，圆柱体积。(PI = 3.1415926)

格式
输入格式：
输入为double型

输出格式：
分行输出，保留2位小数

```c
#include<stdio.h>
int main()
{
    double PI = 3.1415926;
    double radius, high;
    scanf("%lf %lf", &radius, &high);
    printf("%.2lf\n%.2lf\n%.2lf\n%.2lf\n%.2lf", 2 * PI * radius, PI * radius * radius, 4 * PI * radius * radius, 4.0 / 3.0 * PI * radius * radius * radius, PI * radius * radius * high);
    return 0;
}
```

## ⭐MT1009公式计算

计算公式
(1/2)∗(a∗x+(a+x)/(4∗a))(1/2)∗(a∗x+(a+x)/(4∗a))

格式
输入格式：
输入为整型x,a，空格分隔

输出格式：
输出为实型，保留2位小数

```c
#include<stdio.h>
int main()
{
    double x, a, result;
    scanf("%lf %lf", &x, &a);
    result = (1.0 / 2.0) * (a * x + (a + x) / (4.0 * a));
    printf("%.2lf", result);
    return 0;
}
```

## ⭐MT1010输入和输出字符型数据

请编写一个简单程序，用户输入2个的字符型数据存储在变量中，并分别以字符形式和整数形式输出在屏幕上。

格式
输入格式：
2个的字符型数据，用逗号分隔

输出格式：
输出两行TheASCIIcode of… is … (…处依次输出刚才输入的数据字符形式和整数形式）

```c
#include<stdio.h>
    int main()
{
    char character1, character2;
    scanf("%c,%c", &character1, &character2);
    printf("The ASCII code of %c is %d\n", character1, character1);
    printf("The ASCII code of %c is %d ", character2, character2);
    return 0;
}
```

## ⭐MT1011字符和整数

输出’X’、65的字符、十进制数据形式。

格式
输入格式：
无

输出格式：
输出字符、十进制整数，空格分隔

```c
#include<stdio.h>
int main()
{
    char character = 'X';
    int integer = 65;
    printf("%c %d\n%c %d", character, character, integer, integer);
    return 0;
}
```

## ⭐MT1012各种类型长

请编写一个简单程序，输出int、float、double和char的大小。

格式
输入格式：
无

输出格式：
输出分4行，分别输出int、float、double和char的大小

```c
#include<stdio.h>
int main()
{
    printf("Size of int: %d bytes\n", sizeof(int));
    printf("Size of float: %d bytes\n", sizeof(float));
    printf("Size of double: %d bytes\n", sizeof(double));
    printf("Size of char: %d byte\n", sizeof(char));
    return 0;
}

```