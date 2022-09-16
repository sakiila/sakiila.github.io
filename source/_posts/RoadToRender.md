---
title: RoadToRender
tags:
  - Java
categories: 开发
description: <center>RoadToRender 开发</center>
abbrlink: 3652395819
date: 2017-11-30 15:30:00
---
使用的语言：Java
使用的工具：Eclipse

### 一、设计思路
设计五个类，名字分别为MazeFactory、Maze、Check、Main和Test。
#### 1.MazeFactory类
该类包含一个create()方法，传入第一行字符串，使用二维数组进行初始化一个渲染网格，然后返回一个Maze对象。

#### 2.Maze类
该类有一个构造方法，参数为二维数组。另外有一个render()方法，传入第二行字符串，连通网格，并使网格字符化。

#### 3.Check类
该类有两个方法：checkLine1()和checkLine2()。分别对输入的第一行和第二行字符串做检查，返回一个字符串。

#### 4.Main类
该类是主类，有一个main()方法。该方法读取控制台输入的字符串，然后调用Check类的方法检查，再返回一个字符串作为结果。

#### 5.Test类
测试类，用来运行程序，并在控制台输出结果。



### 二、UML图

![RoadToRender-UML](https://upload-images.jianshu.io/upload_images/2348575-d734d316af894e16.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 三、程序测试
#### 1.正常输入：

```log
3 3
0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
```

运行结果：

	Please input:
	3 3
	0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
	The result is:
	[W] [W] [W] [W] [W] [W] [W] 
	[W] [R] [W] [R] [R] [R] [W] 
	[W] [R] [W] [R] [W] [R] [W] 
	[W] [R] [R] [R] [R] [R] [W] 
	[W] [W] [W] [R] [W] [R] [W] 
	[W] [R] [R] [R] [W] [R] [W] 
	[W] [W] [W] [W] [W] [W] [W] 
	
	End.


#### 2.无效的数字：

	3 %
	0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1

运行结果：

	Please input:
	3 %
	0,1 0,2;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
	The result is:
	Invalid number format.
	End.



#### 3.数字超出预定范围：

	3 3
	8,1 0,4;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1

运行结果：

	Please input:
	3 3
	8,1 0,4;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
	The result is:
	Incorrect command format.
	End.


#### 4.格式错误；

	3 3
	0 1 0 2 0 0 1 0 0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1

运行结果：

	Please input:
	3 3
	0 1 0 2 0 0 1 0 0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
	The result is:
	Incorrect command format.
	End.



#### 5.连通性错误：

	3 3
	0,1 0,3;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1

运行结果：

	Please input:
	3 3
	0,1 0,3;0,0 1,0;0,1 1,1;0,2 1,2;1,0 1,1;1,1 1,2;1,1 2,1;1,2 2,2;2,0 2,1
	The result is:
	Incorrect command format.
	End.

