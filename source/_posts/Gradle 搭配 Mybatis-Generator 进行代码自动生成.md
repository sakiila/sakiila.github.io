---
title: Gradle 搭配 Mybatis-Generator 进行代码自动生成
tags:
  - gradle
  - mybatis
categories: 开发
description: <center>Gradle 搭配 Mybatis-Generator 进行代码自动生成。</center>
abbrlink: 680024629
date: 2022-08-24 18:10:37
---
# 一、Mybatis-Generator 引入

在 build.gradle 文件的 plugins 模块加上一行配置：
```
id "com.thinkimi.gradle.MybatisGenerator" version "2.4"
```

然后，在 build.gradle 文件的末尾加上配置引入：
```
configurations {
mybatisGenerator
}

mybatisGenerator {
verbose = true
configFile = 'bob-server-模块名/src/main/resources/MyBatisGeneratorConfig.xml'
}
```
然后 load 一下 Gradle 配置进行刷新。

# 二、MyBatisGeneratorConfig.xml 配置文件修改

将以下配置进行粘贴覆盖：
```xml
<jdbcConnection driverClass="com.mysql.jdbc.Driver"
connectionURL="jdbc:mysql://db" userId="username" password="password">
</jdbcConnection>

<javaModelGenerator targetPackage="me.bob.server.模块名.mapperbean"
targetProject="bob-server-模块名/src/main/java">
<property name="trimStrings" value="true"/>
</javaModelGenerator>

<sqlMapGenerator targetPackage="me.bob.server.模块名.mapperxml"
targetProject="bob-server-模块名/src/main/java"/>

<javaClientGenerator targetPackage="me.bob.server.模块名.mapper"
targetProject="bob-server-模块名/src/main/java" type="XMLMAPPER">
</javaClientGenerator>
```

# 三、使用插件进行代码生成

点击 IDEA 的 Gradle 标签（如果没有标签，见下方备注），打开 Tasks - util，双击 mbGenerator，即可生成代码。如有失败，参考提示信息进行排查。  

备注：如果没有 Gradle 标签，按以下步骤打开：View -> Tool Windows -> Gradle。



参考资料：  
https://segmentfault.com/a/1190000013534059  
https://github.com/kimichen13/mybatis-generator-plugin