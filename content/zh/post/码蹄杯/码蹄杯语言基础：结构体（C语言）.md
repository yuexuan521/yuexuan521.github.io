---
title: "码蹄杯语言基础：结构体（C语言）"
date: 2023-06-26 16:41:19
category: "码蹄杯"
categories: 
  - "码蹄杯"
tags:
- "c语言"
- "c++"
- "算法"
- "开发语言"
- "数据结构"
---

码蹄集网站地址： [https://www.matiji.net/exam/ojquestionlist](https://www.matiji.net/exam/ojquestionlist) 

## ⭐MT1565长者

输出结构体数组中年龄最大者的数据 ，请设计一个结构体， 信息包括名字，年龄 。输入5个人信息，然后再输出年龄最大的人的信息。

格式
输入格式：
输入名字性别为字符型，年龄整型

输出格式：
输出名字性别为字符型，年龄整型

```c
#include<stdio.h>
struct information
{
    char name[10];
    int age;
};
int* max(struct information* people_a, struct information* people_b);
int main()
{
    struct information people1;
    struct information people2;
    struct information people3;
    struct information people4;
    struct information people5;
    struct information* elder;

    scanf("%s %d", people1.name, &people1.age);
    scanf("%s %d", people2.name, &people2.age);
    scanf("%s %d", people3.name, &people3.age);
    scanf("%s %d", people4.name, &people4.age);
    scanf("%s %d", people5.name, &people5.age);

    elder = max(max(max(&people1, &people2), max(&people3, &people4)), &people5);

    printf("%s %d", elder->name, elder->age);
    return 0;
}

int* max(struct information* people_a, struct information* people_b)
{
    if (people_a->age > people_b->age)
    {
        return people_a;
    }
    else
    {
        return people_b;
    }
}

```

## ⭐MT1566幼者

输出结构体数组中年龄最小者的数据 ，请设计一个结构体， 信息包括名字，年龄 。输入5个人信息，然后再输出年龄最小的人的信息。

格式
输入格式：
输入名字性别为字符型，年龄整型

输出格式：
输出名字性别为字符型，年龄整型

```c
#include<stdio.h>
struct information
{
    char name[10];
    int age;
};
int* min(struct information* people_a, struct information* people_b);
int main()
{
    struct information people1;
    struct information people2;
    struct information people3;
    struct information people4;
    struct information people5;
    struct information* young;

    scanf("%s %d", people1.name, &people1.age);
    scanf("%s %d", people2.name, &people2.age);
    scanf("%s %d", people3.name, &people3.age);
    scanf("%s %d", people4.name, &people4.age);
    scanf("%s %d", people5.name, &people5.age);

    young = min(min(min(&people1, &people2), min(&people3, &people4)), &people5);

    printf("%s %d", young->name, young->age);
    return 0;
}

int* min(struct information* people_a, struct information* people_b)
{
    if (people_a->age < people_b->age)
    {
        return people_a;
    }
    else
    {
        return people_b;
    }
}
```

## ⭐MT1567员工薪水

有3个员工，从键盘输入数据，包括工号、姓名、薪水，工号薪水整型，姓名字符型，输出薪水最高的员工信息。不考虑非法输入等特殊情况。

格式
输入格式：
每行输入一个员工的数据，空格分隔。

输出格式：
输出薪水最高的员工信息

```c
#include<stdio.h>
struct information
{
    int job_number;
    char name[10];
    int salary;
};
int* max(struct information* employee_a, struct information* employee_b);
int main()
{
    struct information employee1;
    struct information employee2;
    struct information employee3;
    struct information* employee;

    scanf("%d %s %d", &employee1.job_number, employee1.name, &employee1.salary);
    scanf("%d %s %d", &employee2.job_number, employee2.name, &employee2.salary);
    scanf("%d %s %d", &employee3.job_number, employee3.name, &employee3.salary);

    employee = max(max(&employee1, &employee2), &employee3);

    printf("%d %s %d", employee->job_number, employee->name, employee->salary);
    return 0;
}
int* max(struct information* employee_a, struct information* employee_b)
{
    if (employee_a->salary > employee_b->salary)
    {
        return employee_a;
    }
    else
    {
        return employee_b;
    }
}

```

## ⭐MT1568学生成绩

有3个学生，每个学生有3门课的成绩，从键盘输入数据，包括学号、姓名、三门课成绩，学号整型，姓名字符型，成绩实型，计算3门课程总平均成绩，以及平均分最高的学生信息。不考虑非法成绩等特殊情况。

格式
输入格式：
每行输入一个学生的数据，空格分隔。

输出格式：
输出平均分最高的学生信息

```c
#include<stdio.h>
struct information
{
    int student_number;
    char name[20];
    double course1;
    double course2;
    double course3;
};
int* ave_max(struct information* student_a, struct information* student_b);
int main()
{
    struct information student1;
    struct information student2;
    struct information student3;
    struct information* student;

    scanf("%d %s %lf %lf %lf", &student1.student_number, student1.name, &student1.course1, &student1.course2, &student1.course3);
    scanf("%d %s %lf %lf %lf", &student2.student_number, student2.name, &student2.course1, &student2.course2, &student2.course3);
    scanf("%d %s %lf %lf %lf", &student3.student_number, student3.name, &student3.course1, &student3.course2, &student3.course3);

    student = ave_max(ave_max(&student1, &student2), &student3);

    printf("%d %s %.0lf %.0lf %.0lf", student->student_number, student->name, student->course1, student->course2, student->course3);
    return 0;
}
int* ave_max(struct information* student_a, struct information* student_b)
{
    double ave_a, ave_b;
    ave_a = (student_a->course1 + student_a->course2 + student_a->course3) / 3.0;
    ave_b = (student_b->course1 + student_b->course2 + student_b->course3) / 3.0;
    if (ave_a > ave_b)
    {
        return student_a;
    }
    else
    {
        return student_b;
    }
}
```

## ⭐MT1572交网费

小码哥又喜欢看电视又喜欢打电话聊天，每个月都要花掉很多网费。从键盘输入数据，包括第几季度、该季度网费、话费，其全部整型，计算小码哥今年花了多少钱。不考虑非法输入等特殊情况。

格式
输入格式：
每行输入一组数据，空格分隔。

输出格式：
输出整型

```c
#include<stdio.h>
struct data
{
    int quarter_number;
    int internet_charges;
    int electricity;
};
int main()
{
    struct data quarter1;
    struct data quarter2;
    struct data quarter3;
    struct data quarter4;
    int sum_charges;

    scanf("%d %d %d", &quarter1.quarter_number, &quarter1.internet_charges, &quarter1.electricity);
    scanf("%d %d %d", &quarter2.quarter_number, &quarter2.internet_charges, &quarter2.electricity);
    scanf("%d %d %d", &quarter3.quarter_number, &quarter3.internet_charges, &quarter3.electricity);
    scanf("%d %d %d", &quarter4.quarter_number, &quarter4.internet_charges, &quarter4.electricity);

    sum_charges = quarter1.internet_charges + quarter1.electricity + quarter2.internet_charges + quarter2.electricity + quarter3.internet_charges + quarter3.electricity + quarter4.internet_charges + quarter4.electricity;
    printf("%d", sum_charges);
    return 0;
}

```