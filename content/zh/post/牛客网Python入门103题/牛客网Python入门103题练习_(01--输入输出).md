---
title: "牛客网Python入门103题练习|(01--输入输出)"
date: 2023-03-20 13:30:09
category: "牛客网Python入门103题"
categories: 
  - "牛客网Python"
tags:
- "python"
---

## ⭐NP1 Hello World!

### 描述

将字符串 'Hello World!' 存储到变量str中，再使用print语句将其打印出来。

#### 输入描述：

无

#### 输出描述：

一行输出字符串Hello World!

```python
str =  'Hello World!'
print(str)
 
#str字符串类型
#print()打印函数，表示输出
```

## ⭐NP2 多行输出

### 描述

将字符串 'Hello World!' 存储到变量str1中，再将字符串 'Hello Nowcoder!' 存储到变量str2中，再使用print语句将其打印出来（一行一个变量）。

#### 输入描述：

无

#### 输出描述：

第一行输出字符串Hello World!，第二行输出字符串Hello Nowcoder!

```python
str1 = 'Hello World!'
str2 = 'Hello Nowcoder!'
print(str1+"\n"+str2)
 
#“\n”转义字符，表示换行操作。
```

## ⭐NP3 读入字符串

### 描述

小白正在学习Python，从变量输出开始。请使用input函数读入一个字符串，然后将其输出。

#### 输入描述：

输入一行字符串。

#### 输出描述：

将读入的变量输出。

### 示例1

输入：Nowcoder

输出：Nowcoder

```python
str = input()
print(str)
 
#input输入函数，可输入内容，输入内容类型为字符串类型
```

## ⭐NP4 读入整数数字

### 描述

在学会读入字符串以后，小白还想要读入整数，请你帮他使用input函数读入数字并输出数字与变量类型。

#### 输入描述：

输入只有整数。

#### 输出描述：

将输入的数字输出，同时换行输出变量类型。

输入：1

输出：1

```python
num = int(input())
print(num)
print(type(num))
 
#int()函数，可将其它数据类型转换为int(整数)类型
#type()函数，可返回该数据的数据类型
```

## ⭐NP5 格式化输出（一）

### 描述

牛牛、牛妹和牛可乐正在Nowcoder学习Python语言，现在给定他们三个当中的某一个名字name，

假设输入的name为Niuniu，则输出 I am Niuniu and I am studying Python in Nowcoder!

请按以上句式输出相应的英文句子。

#### 输入描述：

一行一个字符串表示名字。

#### 输出描述：

假设输入的name为Niuniu，则输出I am Niuniu and I am studying Python in Nowcoder!

请按以上句式输出相应的英文句子。

### 示例1

输入：Niuniu

输出：I am Niuniu and I am studying Python in Nowcoder!

```python
str = input()
print('I am',str,'and I am studying Python in Nowcoder!')
 
#第二种方法
#print(f"I am {str} and I am studying Python in Nowcoder!")
#第三种方法
#print('I am {0} and I am studying Python in Nowcoder!'.format(name))
#第四种方法
#print('I am %s and I am studying Python in Nowcoder!'%str)
#'%s'为占位符
```

## ⭐NP6 牛牛的小数输出

### 描述

牛牛正在学习Python的输出，他想要使用print函数控制小数的位数，你能帮助它把所有读入的数据都保留两位小数输出吗？

#### 输入描述：

读入一个浮点类型小数。

#### 输出描述：

保留两位小数输出。

### 示例1

输入：1.000000

输出：1.00

```python
num = float(input())
print("%.2f"%num)
 
#float()函数，可将其它数据类型转换为float(浮点数)类型
#"%.2f"，表示浮点数占位符，并且小数点位后保留两位
```