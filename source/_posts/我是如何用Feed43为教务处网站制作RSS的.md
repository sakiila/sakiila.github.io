---
title: 我是如何用 Feed43 为教务处网站制作 RSS 的
tags:
  - RSS
categories: 开发
description: "<center>教务处网站当年有正方制作，并未考虑到 RSS 源。\t每次去查阅通知都需要主动打开网站检索，于是试图利用 Feed43 将通知被动的发送到个人邮箱中。</center>"
abbrlink: 492101974
date: 2016-11-27 11:23:00
---
教务处网站当年有正方制作，并未考虑到RSS源。每次去查阅通知都需要主动打开网站检索，于是试图利用Feed43将通知被动的发送到个人邮箱中。

## 使用Feed43
打开 feed43.com，点击创建 feed。输入教务处通知网站网址。成功读取。

![](http://upload-images.jianshu.io/upload_images/2348575-d7d3475a55dc734d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 代码抓取
第二步时，我遇到困难。教务处网站代码的div标签过多，尝试多次都无法抓取到内容。
于是使用教务处首页网址进行读取。重新改写检索规则。

![](http://upload-images.jianshu.io/upload_images/2348575-000e0d42c61a1899.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

成功。

![](http://upload-images.jianshu.io/upload_images/2348575-78d9e679281a5200.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
	
## 检索代码：
```html
<li><a href="{%}" target="_blank" title={%}>{%}</a>
<span class="time">{%}
{%}</span>{*}
</li>{*}
```

## RSS如下：
[http://feed43.com/wust-notify.xml](http://feed43.com/wust-notify.xml)


2016年11月27日 12:28:05
自己做的rss源还是有很多不足的地方，过段时间再来修改。
大神勿喷。

2016年11月30日 23:19:39 
新检索代码测试成功。可以推送到邮箱。