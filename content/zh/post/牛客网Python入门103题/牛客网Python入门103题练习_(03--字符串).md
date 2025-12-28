---
title: "牛客网Python入门103题练习|(03--字符串)"
date: 2023-03-23 22:59:04
category: "牛客网Python入门103题"
categories: 
  - "牛客网Python"
tags:
- "python"
---

## ⭐NP10 牛牛最好的朋友们

### 描述

牛牛有两个最好的朋友，他们的名字分别用input读入记录在两个字符串中，请使用字符串连接（+）帮助牛牛将两个朋友的名字依次连接在一个字符串中输出。

#### 输入描述：

依次输入两个字符串

#### 输出描述：

输出连接后的字符串

### 示例1

输入：NiuMei

NiuNeng

输出：NiuMeiNiuNeng

```python
print(input()+input())
```

## ⭐NP11 单词的长度

### 描述

牛妹正在学英语，但是背单词实在是太痛苦了，她想让你帮她写一个小程序，能够根据输入的单词，快速得到单词的长度。

#### 输入描述：

输入一个字符串，仅包含大小写字母。

#### 输出描述：

输出字符串的长度。

### 示例1

输入：Hello

输出：5

```python
str = input()
print(len(str))
#len（）函数统计字符串长度
```

## ⭐NP12 格式化输出（二）

### 描述

牛牛、牛妹和牛可乐都是Nowcoder的用户，某天Nowcoder的管理员希望将他们的用户名以某种格式进行显示，

现在给定他们三个当中的某一个名字name，请分别按全小写、全大写和首字母大写的方式对name进行格式化输出（注：每种格式独占一行）。

#### 输入描述：

一行一个字符串表示名字。

#### 输出描述：

请分别按全小写、全大写和首字母大写的方式对name进行格式化输出（注：每种格式独占一行）。

### 示例1

输入：niuNiu

输出：niuniu

```python
str = input()
print(str.lower())
print(str.upper())
print(str.title())
 
#lower()函数，把字符串中所有字符都转成小写字母
#upper()函数，把字符串中所有字符都转成大写字母
#title()函数，把每个单词的第一个字符转换为大写，把每个单词的剩余字符转换为小写
```

## ⭐NP13 格式化输出（三）

### 描述

牛牛、牛妹和牛可乐都是Nowcoder的用户，某天Nowcoder的管理员由于某种错误的操作导致他们的用户名的左右两边增加了一些多余的空白符（如空格或'\t'等），

现在给定他们三个当中的某一个名字name，请输出name去掉两边的空白符后的原本的内容。

#### 输入描述：

一行一个字符串表示名字name（注：name两边带有一些多余的空白符）。

#### 输出描述：

一行输出name去掉两边的空白符后的原本的内容。

### 示例1

输入：Niuniu

输出：Niuniu

```python
str = input()
print(str.strip())
#strip()，去除左右两边的空格
```

## ⭐NP14 不用循环语句的重复输出

### 描述

牛牛正在学习Python，他想多次输出朋友的名字，但是因为还没有学习循环语句，他不知道该怎么输出，你能够帮助他将输入的朋友的名字重复输出100次吗？（提示：不可以使用循环或者递归语句，使用字符串 * 运算）

#### 输入描述：

输入一个字符串。

#### 输出描述：

输出重复100次之后的字符串，字符串之间没有间隔。

### 示例1

输入：Hello

输出：HelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHelloHello

```python
str = input()
str = str * 100
print(str)
```

## ⭐NP15 截取用户名前10位

### 描述

牛客网正在录入用户的昵称，但是有的昵称太长了，对于这些过长的昵称，牛牛觉得截取昵称字符串前10个字符就可以了，你可以帮他写一个程序吗？

#### 输入描述：

输入一个字符串，长度一定不低于10。

#### 输出描述：

输出截取前10个字符后的子串。

### 示例1

输入：NiuNiuisBest

输出：NiuNiuisBe

```python
str = input()
print(str[0:10:1])
 
#str[0:10:1],表示从下标为0的字符开始，到下标为9的字符结束，步长为1
```