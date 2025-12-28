---
title: "BUUCTF [RCTF2019]draw 1"
date: 2025-09-15 08:30:00
category: "BUUCTF MISC"
categories: 
  - "CTF"
tags:
- "BUUCTF"
- "CTF"
- "RCTF2019"
- "Logo语言"
- "draw"
- "网络安全"
- "MISC"
---

![](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191728297.png)

**BUUCTF: [https://buuoj.cn/challenges](https://buuoj.cn/challenges)** 

---

相关阅读
[CTF Wiki](https://ctf-wiki.org/) 
[BUUCTF：[RCTF2019]draw](https://blog.csdn.net/mochu7777777/article/details/105369804) 
[RCTF 2019 Official Writeup](https://blog.rois.io/2019/06/06/rctf-2019-official-writeup/#:~:text=%E4%BD%BF%E7%94%A8Wire) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191730360.png)

### 题目描述：

得到的 flag 请包上 flag{} 提交。

### 密文：

保存attachment.txt文件，内容如下：

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191731854.png)

```bash
cs pu lt 90 fd 500 rt 90 pd fd 100 rt 90 repeat 18[fd 5 rt 10] lt 135 fd 50 lt 135 pu bk 100 pd setcolor pick [ red orange yellow green blue violet ] repeat 18[fd 5 rt 10] rt 90 fd 60 rt 90 bk 30 rt 90 fd 60 pu lt 90 fd 100 pd rt 90 fd 50 bk 50 setcolor pick [ red orange yellow green blue violet ] lt 90 fd 50 rt 90 fd 50 pu fd 50 pd fd 25 bk 50 fd 25 rt 90 fd 50 pu setcolor pick [ red orange yellow green blue violet ] fd 100 rt 90 fd 30 rt 45 pd fd 50 bk 50 rt 90 fd 50 bk 100 fd 50 rt 45 pu fd 50 lt 90 pd fd 50 bk 50 rt 90 setcolor pick [ red orange yellow green blue violet ] fd 50 pu lt 90 fd 100 pd fd 50 rt 90 fd 25 bk 25 lt 90 bk 25 rt 90 fd 25 setcolor pick [ red orange yellow green blue violet ] pu fd 25 lt 90 bk 30 pd rt 90 fd 25 pu fd 25 lt 90 pd fd 50 bk 25 rt 90 fd 25 lt 90 fd 25 bk 50 pu bk 100 lt 90 setcolor pick [ red orange yellow green blue violet ] fd 100 pd rt 90 arc 360 20 pu rt 90 fd 50 pd arc 360 15 pu fd 15 setcolor pick [ red orange yellow green blue violet ] lt 90 pd bk 50 lt 90 fd 25 pu home bk 100 lt 90 fd 100 pd arc 360 20 pu home
```

---

### 解题思路：

1、从题目和文本的数据可以看出，要画出什么东西。数据是Logo语言代码，使用任意Logo语言解释器就可以运行代码，得到flag。

Logo解释器： [https://www.calormen.com/jslogo/](https://www.calormen.com/jslogo/) 

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191733830.png)

附一个类似的python代码：

```python
# @Author：YueXuan
# @Date  ：2024/10/4 14:37

import turtle

# 初始化画布
screen = turtle.Screen()
screen.setup(width=800, height=800)

# 创建一个海龟对象
t = turtle.Turtle()

# 设置速度
t.speed(0)

# 开始绘制
t.penup()  # 抬笔
t.left(90)  # 向左转 90 度
t.forward(500)  # 向前走 500 单位
t.right(90)  # 向右转 90 度
t.pendown()  # 落笔

t.forward(100)  # 向前走 100 单位
t.right(90)  # 向右转 90 度

# 重复绘制
for _ in range(18):
    t.forward(5)
    t.right(10)

# 绘制下一个图形
t.left(135)  # 向左转 135 度
t.forward(50)  # 向前走 50 单位
t.left(135)  # 向左转 135 度
t.penup()  # 抬笔
t.backward(100)  # 向后走 100 单位
t.pendown()  # 落笔

# 设置颜色
colors = ['red', 'orange', 'yellow', 'green', 'blue', 'violet']
index = 0

# 重复绘制
for _ in range(18):
    t.forward(5)
    t.right(10)

t.right(90)  # 向右转 90 度
t.forward(60)  # 向前走 60 单位
t.right(90)  # 向右转 90 度
t.backward(30)  # 向后走 30 单位
t.right(90)  # 向右转 90 度
t.forward(60)  # 向前走 60 单位
t.penup()  # 抬笔
t.left(90)  # 向左转 90 度
t.forward(100)  # 向前走 100 单位
t.pendown()  # 落笔

t.right(90)  # 向右转 90 度
t.forward(50)  # 向前走 50 单位
t.backward(50)  # 向后走 50 单位
t.color(colors[index % len(colors)])  # 设置颜色
index += 1
t.left(90)  # 向左转 90 度
t.forward(50)  # 向前走 50 单位
t.right(90)  # 向右转 90 度
t.forward(50)  # 向前走 50 单位
t.penup()  # 抬笔
t.forward(50)  # 向前走 50 单位
t.pendown()  # 落笔
t.forward(25)  # 向前走 25 单位
t.backward(50)  # 向后走 50 单位
t.forward(25)  # 向前走 25 单位
t.right(90)  # 向右转 90 度
t.forward(50)  # 向前走 50 单位
t.penup()  # 抬笔

# 绘制下一个图形
t.color(colors[index % len(colors)])  # 设置颜色
index += 1
t.forward(100)  # 向前走 100 单位
t.right(90)  # 向右转 90 度
t.forward(30)  # 向前走 30 单位
t.right(45)  # 向右转 45 度
t.pendown()  # 落笔
t.forward(50)  # 向前走 50 单位
t.backward(50)  # 向后走 50 单位
t.right(90)  # 向右转 90 度
t.forward(50)  # 向前走 50 单位
t.backward(100)  # 向后走 100 单位
t.forward(50)  # 向前走 50 单位
t.right(45)  # 向右转 45 度
t.penup()  # 抬笔
t.forward(50)  # 向前走 50 单位
t.left(90)  # 向左转 90 度
t.pendown()  # 落笔
t.forward(50)  # 向前走 50 单位
t.backward(50)  # 向后走 50 单位
t.right(90)  # 向右转 90 度

# 绘制下一个图形
t.color(colors[index % len(colors)])  # 设置颜色
index += 1
t.forward(50)  # 向前走 50 单位
t.penup()  # 抬笔
t.left(90)  # 向左转 90 度
t.forward(100)  # 向前走 100 单位
t.pendown()  # 落笔
t.forward(50)  # 向前走 50 单位
t.right(90)  # 向右转 90 度
t.forward(25)  # 向前走 25 单位
t.backward(25)  # 向后走 25 单位
t.left(90)  # 向左转 90 度
t.backward(25)  # 向后走 25 单位
t.right(90)  # 向右转 90 度
t.forward(25)  # 向前走 25 单位

# 继续绘制
t.color(colors[index % len(colors)])  # 设置颜色
index += 1
t.penup()  # 抬笔
t.forward(25)  # 向前走 25 单位
t.left(90)  # 向左转 90 度
t.backward(30)  # 向后走 30 单位
t.pendown()  # 落笔
t.right(90)  # 向右转 90 度
t.forward(25)  # 向前走 25 单位
t.penup()  # 抬笔
t.forward(25)  # 向前走 25 单位
t.left(90)  # 向左转 90 度
t.pendown()  # 落笔
t.forward(50)  # 向前走 50 单位
t.backward(25)  # 向后走 25 单位
t.right(90)  # 向右转 90 度
t.forward(25)  # 向前走 25 单位
t.left(90)  # 向左转 90 度
t.forward(25)  # 向前走 25 单位
t.backward(50)  # 向后走 50 单位
t.penup()  # 抬笔
t.backward(100)  # 向后走 100 单位
t.left(90)  # 向左转 90 度

# 绘制圆弧
t.color(colors[index % len(colors)])  # 设置颜色
index += 1
t.forward(100)  # 向前走 100 单位
t.pendown()  # 落笔
t.right(90)  # 向右转 90 度
t.circle(20, 360)  # 绘制半径为 20 的圆
t.penup()  # 抬笔
t.right(90)  # 向右转 90 度
t.forward(50)  # 向前走 50 单位
t.pendown()  # 落笔
t.circle(15, 360)  # 绘制半径为 15 的圆
t.penup()  # 抬笔
t.forward(15)  # 向前走 15 单位
t.color(colors[index % len(colors)])  # 设置颜色
index += 1
t.left(90)  # 向左转 90 度
t.pendown()  # 落笔
t.backward(50)  # 向后走 50 单位
t.left(90)  # 向左转 90 度
t.forward(25)  # 向前走 25 单位
t.penup()  # 抬笔
t.home()  # 返回原点
t.backward(100)  # 向后走 100 单位
t.left(90)  # 向左转 90 度
t.forward(100)  # 向前走 100 单位
t.pendown()  # 落笔
t.circle(20, 360)  # 绘制半径为 20 的圆
t.penup()  # 抬笔
t.home()  # 返回原点

# 结束绘图
turtle.done()
```

 ![在这里插入图片描述](https://cdn.jsdelivr.net/gh/yuexuan521/image/20251228191735446.png)

### flag：

```bash
flag{RCTF_HeyLogo}
```