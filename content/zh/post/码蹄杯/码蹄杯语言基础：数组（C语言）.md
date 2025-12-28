---
title: "码蹄杯语言基础：数组（C语言）"
date: 2023-06-09 17:17:45
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "c++"
- "算法"
- "数据结构"
- "开发语言"
---

码蹄集网站地址： [https://www.matiji.net/exam/ojquestionlist](https://www.matiji.net/exam/ojquestionlist) 

## ⭐MT1381逆序输出数组

定义一个长度为10的整型数组，输入10个数组元素的值，然后逆序输出他们

格式
输入格式：
输入10个数组元素的值，整型，空格分隔

输出格式：
逆序输出10个数组元素的值，整型，空格分隔

```c
#include<stdio.h>
#define N 10
int main()
{
    int i, a[N];
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = N - 1; i >= 0; i--)
    {
        printf("%d ", a[i]);
    }
    return 0;
}
```

## ⭐MT1382奇数项

定义一个长度为10的整型数组，输入10个数组元素的值，然后输出奇数项。

格式
输入格式：
输入10个数组元素的值，整型，空格分隔

输出格式：
输出数组奇数项，整型，空格分隔

```c
#include<stdio.h>
#define N 10
int main()
{
    int i, a[N];
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = 1; i < N; i)
    {
        printf("%d ", a[i]);
        i += 2;
    }
    return 0;
}
```

## ⭐MT1385查找

在一组给定的数据中，找出某个数据是否存在。定义长度为10的数组，输入数组元素，和要查找的数据，如果找到输出下标。没找到则输出No。

格式
输入格式：
第1行输入数组元素，空格分隔

第2行输入要查找的整数n

输出格式：
输出整型

```c
#include<stdio.h>
#define N 10
int main()
{
    int a[N], i, n;
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    scanf("%d", &n);
    for (i = 0; i < N; i++)
    {
        if (a[i] == n)
        {
            printf("%d\n", i);
            break;
        }
        else
        {
            if (i == N - 1)
            {
                printf("No");
            }
        }
    }
    return 0;
}
```

## ⭐MT1386第n个数

编写程序读入n（n<200）个整数（输入-9999结束）。找出第1到第n－1个数中第1个与第n个数相等的那个数，并输出该数的序号（序号从1开始）。如果没有，则输出”no such number”。

格式
输入格式：
输入为整型，空格分隔。

输出格式：
输出为整型。

```c
//#include<stdio.h>
//#define N 200
//int main()
//{
//    int a[N], i = -1, n = 0;
//    do
//    {
//        i += 1;
//        scanf("%d", &a[i]);
//    } while (a[i] == -9999);
//    while (n > i - 1)
//    {
//        if (a[n] == a[i - 1])
//        {
//            printf("%d\n", n + 1);
//            break;
//        }
//        else
//        {
//            if (n == i - 1)
//            {
//                printf("no such number");
//            }
//            else
//            {
//                n++;
//            }
//        }
//    }
//    return 0;
//}
#include<stdio.h>
#define N 200
int main()
{
    int a[N], i, n = 0;
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = 0; i < N; i++)
    {
        if (a[i] == -9999)
        {
            break;
        }
    }
    for (n = 0; n < i; n++)
    {
        if (a[n] == a[i - 1])
        {
            if (n == i - 1)
            {
                printf("no such number\n");
            }
            else
            {
                printf("%d\n", n + 1);
                break;
            }
        }
    }
    return 0;
}

```

## ⭐MT1387删除指定元素

定义一个长度为n的整型数组，输入n个数组元素的值，然后输入要删除的数编号，比如删掉从左向右第5个数，输出删除后的数组。

格式
输入格式：
输入整型，分3行输入。第一行输入n，第二行输入n个数组元素的值，空格分隔，第三行输入编号

输出格式：
输出整型，空格分隔

```c
#include<stdio.h>
int main()
{
    int n, i, K;
    scanf("%d", &n);
    int a[n];
    for (i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
    }
    scanf("%d", &K);
    for (i = 0; i < n; i++)
    {
        if (i >= K - 1)
        {
            if (i != n - 1)
            {
                a[i] = a[i + 1];
            }
            else
            {
                a[i] = 0;
            }
        }
    }
    for (i = 0; i < n - 1; i++)
    {
        printf("%d ", a[i]);
    }

    return 0;
}

```

## ⭐MT1393重复元素

请编写一个简单程序，输入10个整型元素，依次输出重复元素。

格式
输入格式：
输入整型元素，空格分隔。

输出格式：
输出整型，空格分隔。

```c
#include<stdio.h>
#define N 10
int main()
{
    int a[N], i, j;
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = 0; i < N; i++)
    {
        for (j = i + 1; j < N; j++)
        {
            if (a[i] == a[j])
            {
                printf("%d ", a[i]);
            }
        }
    }
    return 0;
}
```

## ⭐MT1394元素频次

请编写一个简单程序，输入10个整型元素，输出数组中每个元素出现的次数。

格式
输入格式：
输入整型，空格分隔。

输出格式：
依次输出元素频次，每个一行。

```c
//#include<stdio.h>
//#define N 10
//int main()
//{
//    int a[N], i, j, o, count, bool_i;
//    for (i = 0; i < N; i++)
//    {
//        scanf("%d", &a[i]);
//    }
//    for (i = 0; i < N; i++)
//    {
//        count = 1;
//        for (j = i + 1; j < N; j++)
//        {
//            if (a[i] == a[j])
//            {
//                count++;
//            }
//        }
//        bool_i = 1;
//        for (o = 0; o < i; o++)
//        {
//            if (a[o] == a[i])
//            {
//                bool_i = 0;
//            }
//        }
//        if (bool_i)
//        {
//            printf("%d %d\n", a[i], count);
//        }
//    }
//    return 0;
//}

#include<stdio.h>
#include<stdbool.h>
#define N 10
// #define TRUE 1
// #define FALSE 0
int main()
{
    int a[N], i, j, o, count, flag;
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = 0; i < N; i++)
    {
        count = 1;
        for (j = i + 1; j < N; j++)
        {
            if (a[i] == a[j])
            {
                count++;
            }
        }
        flag = true;
        for (o = 0; o < i; o++)
        {
            if (a[o] == a[i])
            {
                flag = false;
            }
        }
        if (flag)
        {
            printf("%d %d\n", a[i], count);
        }
    }
    return 0;
}

```

## ⭐MT1395统计

统计一个整型数组中不同元素出现的次数。

格式
输入格式：
第一行输入数组元素个数N为整型，第二行输入元素，如样例所示。

输出格式：
输出为整型，前面是元素，后面是出现的次数，每种一行。

```c
#include<stdio.h>
#include<stdbool.h>
int main()
{
    int N, i, j, o, count, flag;
    scanf("%d", &N);
    int a[N];
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = 0; i < N; i++)
    {
        count = 1;
        for (j = i + 1; j < N; j++)
        {
            if (a[i] == a[j])
            {
                count++;
            }
        }
        flag = true;
        for (o = 0; o < i; o++)
        {
            if (a[o] == a[i])
            {
                flag = false;
            }
        }
        if (flag)
        {
            printf("%d %d\n", a[i], count);
        }
    }
    return 0;
}

```

## ⭐MT1396排序吧

定义一个长度为n的整型数组，输入n个数组元素的值，然后输出从小到大排序后数组元素。

格式
输入格式：
输入整型，分2行输入。第一行输入n，第二行输入n个数组元素的值，空格分隔

输出格式：
输出整型，空格分隔

```c
#include<stdio.h>
void BubbleSort(int a[], int size)
{
    int i, j, temp;
    for (i = 0; i < size - 1; i++)
    {
        for (j = 0; j < size - i - 1; j++)
        {
            if (a[j] > a[j + 1])
            {
                temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
            }
        }
    }
}
int main()
{
    int n, i, size;
    scanf("%d", &n);
    int a[n];
    for (i = 0; i < n; i++)
    {
        scanf("%d", &a[i]);
    }
    size = sizeof(a) / sizeof(a[0]);
    BubbleSort(a, size);
    for (i = 0; i < n; i++)
    {
        printf("%d ", a[i]);
    }
    return 0;
}

```

## ⭐MT1399冒泡排序

输入10个整型元素，对数组进行冒泡排序，输出从小到大排序后的新数组。

格式
输入格式：
输入整型，空格分隔。

输出格式：
输出整型，空格分隔。

```c
#include<stdio.h>
#define N 10
int main()
{
    int a[N], i, j, temp;
    for (i = 0; i < N; i++)
    {
        scanf("%d", &a[i]);
    }
    for (i = 0; i < N - 1; i++)
    {
        for (j = 0; j < N - i - 1; j++)
        {
            if (a[j] > a[j + 1])
            {
                temp = a[j];
                a[j] = a[j + 1];
                a[j + 1] = temp;
            }
        }
    }
    for (i = 0; i < N; i++)
    {
        printf("%d ", a[i]);
    }
    return 0;
}
```