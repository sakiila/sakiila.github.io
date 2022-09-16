---
title: Alfred Workflow 时间转换
tags:
  - Alfred
  - 实战
categories: 开发
description: <center>使用 Alfred Workflow 创建一个时间转换的工具。</center>
abbrlink: 393446932
date: 2022-03-23 19:37:39
---

# 创建 Workflow

参考教程：https://sspai.com/post/61754 。

# 注意：如果使用 Python3

需要使用此仓库的 workflow 文件夹内容：https://github.com/NorthIsUp/alfred-workflow-py3。

# 注意：默认时区为当地时区

如果需要 UTC 时间，请手动将 timestamp.py 中 第 14 行的 `time.localtime(ts)` 修改为 `time.gmtime(ts)` 。

# 使用示例

![TimeConvert](../images/TimeConvert.png)

# 项目地址

https://github.com/sakiila/minipy/tree/main/AlfredWorkflow


参考教程：
[使用python实现一个日期和时间戳互转的Alfred workflow](https://pythonmana.com/2022/02/202202252149470874.html)

