---
title: JavaBrowser 开发
tags:
  - Java
categories: 开发
description: <center>开发了一个简单的网页浏览器，可以实现网页访问、保存，以及操作的前进、后退功能。</center>
abbrlink: 2069714304
date: 2016-11-30 15:30:00
---

开发了一个简单的网页浏览器，可以实现网页访问、保存，以及操作的前进、后退功能。

# 功能描述
- 访问：输入URL，可以在线访问对应网页。
- 另存为：可以保存正在访问的页面。
- 前进：访问当前页面的上一个页面。
- 后退：访问当前页面的下一个页面。
- 查看源文件：查看访问页面的HTML源文件，并且可以保存。
<br />

# 理论基础
- 事件种类（低级事件和语义事件）、事件监听器接口、事件适配器（Adapter）
- Swing相关组件（JFram、JMenu、JButton、JTextField及JOptionPane）
- 输入输出流
<br />

# 总体设计
主要程序由WebBrowser.java和ViewSourceFrame.java两个文件组成。
1. WebBrowser.java
	此文件包含名为WebBrowser的public类，其主要功能为生成网页浏览器的主体框架，实现框架上各个组件的事件监听。WebBrowser.java主要包括四个模块：图形用户界面的构建、组件监听接口的实现、文件保存功能的实现、查看源代码框架的生成。
2. ViewSourceFrame.java
	此文件包含名为ViewSourceFrame的类，其主要功能是实现了源文件查看的主题框架，并实现了源文件的保存功能。ViewSourceFrame.java主要包括两个模块：图形用户界面的构建和组件监听接口的实现。
<br />

# 代码实现
[https://github.com/s1xt33nth/JavaBrowser](https://github.com/s1xt33nth/JavaBrowser "https://github.com/s1xt33nth/JavaBrowser")

# 参考资料
[Java 发送邮件 | 菜鸟教程](http://www.runoob.com/java/java-sending-email.html "Java 发送邮件 | 菜鸟教程")
[【Apache POI】Java 读取Excel文件](https://blog.csdn.net/lmb55/article/details/64537293 "【Apache POI】Java 读取Excel文件")
[java读取Excel的方法，解析xls、xlsx两种格式](https://blog.csdn.net/superdog007/article/details/48338337 "java读取Excel的方法，解析xls、xlsx两种格式")
[Java Web(十三) 使用javamail进行发送邮件，(使用QQ，163，新浪邮箱服务器)](http://www.cnblogs.com/whgk/p/6506027.html "Java Web(十三) 使用javamail进行发送邮件，(使用QQ，163，新浪邮箱服务器)")
[POI对Excel自定义日期格式的读取 - ITeye - ITeye博客](http://yl-fighting.iteye.com/blog/1726285726285 "POI对Excel自定义日期格式的读取 - ITeye - ITeye博客")
