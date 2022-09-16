---
title: ExcelGo
tags:
  - Java
  - Excel
categories: 开发
description: <center>ExcelGo 是一个能读取 Excel 文件内容，并获取到其中的邮箱，将表格内容发送至指定的邮箱地址。</center>
abbrlink: 2646382575
date: 2018-07-17 21:10:00
---
ExcelGo 是一个能读取 Excel 文件内容，并获取到其中的邮箱，将表格内容发送至指定的邮箱地址。最初构想来自于需要将公司内部对账单文件中的每行内容发送至各人邮箱。
# 功能描述
- 获取表格内容：通过Apache POI组件读取Excle表格，将内容存放至HashMap集合中。
- 发送邮件：通过员工编号将同一个人的信息组合起来，使用JavaMail API 和Java Activation Framework (JAF) 发送至其邮箱。

# 总体设计
主要程序由GoExcel.java、GoMail.java和Main.java三个文件组成。
1. GoExcel.java
	此文件判断Excel文件后缀类型，读取Excel表格表头和数据内容，并根据Cell类型设置数据。
2. GoMail.java
	此文件包含名为goMail的方法，其主要功能是获得收件人邮箱和表格内容，通过QQ邮箱来发送消息。
3. Main.java
	此文件包含Main方法，定义Excel文件路径，组合同一个收件人的消息，并调用goMail方法发送邮件。

# 文件格式
因为特别的原因，此处将前三列分别定义为员工编号、收件人姓名和其邮箱地址。
![Excel文件格式](https://upload-images.jianshu.io/upload_images/2348575-ed5d4735a94e7901.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "Excel文件格式")

# 效果展示
![收件效果](https://upload-images.jianshu.io/upload_images/2348575-46de67c91fe48053.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "收件效果")

# 代码实现
[https://github.com/sakiila/ExcelGo](https://github.com/sakiila/ExcelGo)