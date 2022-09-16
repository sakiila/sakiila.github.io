---
title: 使用 WMITools 解除浏览器被 hao123 劫持
tags:
  - hao123
categories: 开发
description: <center>最近使用 Chrome 浏览器时，主页会自动变 成hao123.com。改 hosts，清理注册表都无法彻底解决该问题。</center>
abbrlink: 2135150665
date: 2016-03-04 18:49:00
---
最近使用 Chrome 浏览器时，主页会自动变成 hao123.com。改 hosts，清理注册表都无法彻底解决该问题。

经过搜索后，发现可以使用 WMITools 工具进行清理 VBScript 调用系统服务项。

[原网址](http://www.iefans.net/ie-zhuye-jiechi-www-2345-com-kunown/)

# 下载安装
链接：http://pan.baidu.com/s/1nuTC9Kl 密码：1qdv

安装最好使用默认地址，一路确定即可。
# 工具使用
打开安装文件夹，右键 wbemeventviewer.exe 程序，使用管理员身份打开。

点击左上角钢笔形状按钮
![](http://hibaobo.com/wp-content/uploads/2016/12/QQ截图20161206223953.jpg)
 点击OK
 ![](http://hibaobo.com/wp-content/uploads/2016/12/QQ截图20161206224008.jpg)
 再次点击OK
 ![](http://hibaobo.com/wp-content/uploads/2016/12/QQ截图20161206224031.jpg)
 将此文件夹下的文件右键删除
![](https://hibaobo.com/wp-content/uploads/2016/12/QQ截图20161206224059.jpg)
# 最后
右键桌面上的浏览器，属性中目标一栏，将 hao123 等网址删除。