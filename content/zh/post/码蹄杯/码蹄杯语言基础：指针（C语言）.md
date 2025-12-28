---
title: "码蹄杯语言基础：指针（C语言）"
date: 2023-06-11 09:45:58
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "c++"
- "开发语言"
- "算法"
---

码蹄集网站地址： [https://www.matiji.net/exam/ojquestionlist](https://www.matiji.net/exam/ojquestionlist) 
Gitee代码仓库： [码蹄杯代码](https://gitee.com/YueXuan521/c-language-practice/blob/master/%E7%A0%81%E8%B9%84%E6%9D%AF%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83/%E7%A0%81%E8%B9%84%E6%9D%AF%E7%AE%97%E6%B3%95%E8%AE%AD%E7%BB%83/practice.c) 

## ⭐MT1515整型变量和它的指针

定义一个整型变量和指针，让指针指向这个变量，通过指针输出变量的值。

格式
输入格式：
输入整型

输出格式：
输出整型

```c
#include<stdio.h>
int main() 
{
    int x, *p;
    scanf("%d", &x);
    p = &x;
    printf("%d\n", *p);
    return 0; 
}
```

## ⭐MT1517顺序输出

编写一个程序，输入3个整数，用指针方法指向他们， 将它们按由小到大的顺序输出。

格式
输入格式：
输入整型，空格分隔

输出格式：
输出整型，空格分隔

```c
#include<stdio.h>
int main() 
{
    int a, b, c, *x, *y, *z;
    scanf("%d %d %d", &a, &b, &c);
    x = &a;
    y = &b;
    z = &c;
    if ((*x < *y) && (*y < *z))
    {
        printf("%d %d %d", *x, *y, *z);
    }
    else if ((*z < *y) && (*y < *x))
    {
        printf("%d %d %d", *z, *y, *x);
    }
    else if ((*y < *x) && (*x < *z))
    {
        printf("%d %d %d", *y, *x, *z);
    }
    else if ((*z < *x) && (*x < *y))
    {
        printf("%d %d %d", *z, *x, *y);
    }
    else if ((*x < *z) && (*z < *y))
    {
        printf("%d %d %d", *x, *z, *y);
    }
    else if ((*y < *z) && (*z < *x))
    {
        printf("%d %d %d", *y, *z, *x);
    }
    return 0; 
}
```

## ⭐MT1518用指针交换

输入3个整数a,b,c，编写一个交换函数，用指针做参数，将它们按由小到大的顺序放到a,b,c中再输出。

格式
输入格式：
输入整型，空格分隔。

输出格式：
输出整型，空格分隔。

```c
#include<stdio.h>
#define N 3
void Exchange(int *p, int size)
{
    int i, j, temp, *q;
    q = p;
    for (i=0;i<size-1;i++)
    {
        for (j=0;j<size-i-1;j++)
        {
            if (*p > *++q)
            {
                temp = *p;
                *p = *q;
                *q = temp;
            }
        }
    }
}
int main() 
{
    int a[N], i, size;
    for (i=0;i<N;i++)
    {
        scanf("%d", &a[i]);
    }
    size = sizeof(a) / sizeof(a[0]);
    Exchange(a, size);
    for (i=0;i<N;i++)
    {
        printf("%d ", a[i]);
    }
    return 0; 
}
```

## ⭐MT1519实型指针

定义一个实型变量和指针，让指针指向这个变量，通过指针输出变量的值。

格式
输入格式：
输入实型

输出格式：
输出实型

```c
#include<stdio.h>
int main() 
{
    double x, *p;
    scanf("%lf", &x);
    p = &x;
    printf("%lf\n", *p); 
    return 0; 
}
```

## ⭐MT1520字符型指针

定义一个字符型变量和指针，让指针指向这个变量，通过指针输出变量的值。

格式
输入格式：
输入字符型

输出格式：
输出字符型

```c
#include<stdio.h>
int main() 
{
    char x, *p;
    scanf("%c", &x);
    p = &x;
    printf("%c\n", *p);
    return 0; 
}
```

## ⭐MT1534指针递增

编写一个使用指针递增方式访问数组a的元素的程序。数组长度为3，数组元素为1，2，3。

格式
输入格式：
无

输出格式：
分行输出数组a的元素，数组为整型

```c
#include<stdio.h>
int main()
{
    int a[3] = { 1, 2, 3 };
    int* p = a;
    for (int i = 0; i < 3; i++)
    {
        printf("a[%d]=%d\n", i, *p++);
    }
    return 0;
}
```

## ⭐MT1455

```c
// #include<stdio.h>
// #define N 10
// int main() 
// { 
//     int a[N], i;
//     for (i=0;i<N;i++)
//     {
//         scanf("%d", &a[i]);
//     }
//     int *p = a;
//     for (i=0;i<N;i++)
//     {
//         if (i % 2 == 0)
//         {
//             printf("%d ", *p++);
//         }
//         else
//         {
//             *p++;
//         }
//     }
//     return 0; 
// }

#include<stdio.h>
#define N 10
int main()
{
    int a[N], i;
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    int* p = a;
    for (i = 0; i < N / 2; i++)
    {
        printf("%d ", *p);
        p = p + 2;
    }
    return 0;

```