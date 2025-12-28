---
title: "牛客网Python入门103题练习|【08--元组】"
date: 2023-08-18 10:58:41
category: "牛客网Python入门103题"
categories: 
  - "牛客网Python"
tags:
- "python"
- "算法"
- "开发语言"
- "数据结构"
- "笔记"
- "牛客网"
---

## ⭐NP62 运动会双人项目

### 描述

牛客运动会上有一项双人项目，因为报名成功以后双人成员不允许被修改，因此请使用元组（tuple）进行记录。先输入两个人的名字，请输出他们报名成功以后的元组。

#### 输入描述：

第一行以字符串的形式输入第一个人的名字。

第二行以字符串的形式输入第二个人的名字。

#### 输出描述：

直接输出两个名字组成的元组。

```python
print((input(),input()))
```

## ⭐NP63 修改报名名单

### 描述

牛牛和牛妹报名了牛客运动会的双人项目，但是由于比赛前一天牛妹身体不适，不能参加第二天的运动，于是想让牛可乐代替自己。

请创建一个依次包含字符串'Niuniu'和'Niumei'的元组entry_form，并直接输出整个元组。

然后尝试使用try- except代码块执行语句：entry-form[1] = 'Niukele'，若是引发TypeError错误，请输出'The entry form cannot be modified!'。

#### 输入描述：

无

#### 输出描述：

第一行输出创建的元组整体。

第二行若是修改失败，则输出错误信息。

```python
entry_form = ('Niuniu','Niumei')
print(entry_form)
try:
    entry_form[1] = 'Niukele'
except TypeError:
    print('The entry form cannot be modified!')
```

## ⭐NP64 输出前三同学的成绩

### 描述

学校录入考试排名信息以后，为了防止修改，都会记录为Python元组。请你根据输入的字符串，使用tuple函数将它们作为考生姓名记录到元组中，并使用切片法输出前三名同学的名字。

#### 输入描述：

一行输入多个字符串表示考生的名字，以空格间隔。

输入名字数可能少于3。

#### 输出描述：

截取输出该元组前三位的部分。

```python
t = tuple(input().split())
print(t[0:3])
```

## ⭐NP65 名单中出现过的人

### 描述

牛客网有一份秘密名单，['Tom', 'Tony', 'Allen', 'Cydin', 'Lucy', 'Anna']，请将它们创建为不可被修改的Python元组后，输出整个元组。

对于牛牛输入的名字name，请使用成员运算检验它是否出现在这份秘密名单中，若是在名单中则输出'Congratulations!'，否则输出'What a pity!'.

#### 输入描述：

以字符串的形式输入一个名字，只包含大小写字母。

#### 输出描述：

第一行输出完整元组，第二行根据判断输出相应语句。

```python
name = input()
t = ('Tom', 'Tony', 'Allen', 'Cydin', 'Lucy', 'Anna')
print(t)
if name in t:
    print('Congratulations!')
else:
    print('What a pity!')
```

## ⭐NP66 增加元组的长度

### 描述

牛牛有一个元组，其中记录数字1-5，请创建该元组，并使用len函数获取该元组的长度。

牛牛觉得这个元组太短了，想要在该元组后再连接一个6-10的元祖，请输出连接后的元组及长度。

#### 输入描述：

无

#### 输出描述：

第一行输出整体的原始元组。（带括号输出）

第二行输出原始元组的长度。

第三行输出连接后的整体元组。（带括号输出）

第四行输出连接后的元组长度。

```python
t1 = tuple(range(1,6))
t2 = tuple(range(6,11))
t3 = t1+t2
print(t1)
print(len(t1))
print(t3)
print(len(t3))
```