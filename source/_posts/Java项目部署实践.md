---
title: Java 项目部署实践
tags:
  - Java
  - 实战
categories: 开发
description: <center>简要记录如何在服务器上搭建相关环境，配置相关服务等操作。</center>
abbrlink: 1026407118
date: 2022-01-16 14:37:26
---

系统基于 CentOS7。

# 配置环境

## 安装 JDK

查看 yum 库中的 JDK 安装包 ：`yum -y list java*`

安装需要的 JDK 版本的所有程序：`yum -y install java-1.8.0-openjdk*`

参考链接：[在CentOS7.4中安装jdk的几种方法及配置环境变量_勿忘初心的博客-CSDN博客_centos配置java环境变量](https://blog.csdn.net/qq_32786873/article/details/78749384)

## 安装 Maven

安装命令：`yum install -y apache-maven`

修改镜像：`vi /usr/share/apache-maven/conf/settings.xml`

```xml
<mirror>
  <id>alimaven</id>
  <name>aliyun maven</name>
  <url><http://maven.aliyun.com/nexus/content/groups/public/></url>
  <mirrorOf>central</mirrorOf>
</mirror>
```

参考链接：[CentOS上用yum安装Maven](https://www.jianshu.com/p/dfccd5de6032)

## 安装 Docker

安装命令：`yum -y install docker-ce`

启动命令：`systemctl start docker`

参考链接：[centos7安装Docker详细步骤（无坑版教程）](https://cloud.tencent.com/developer/article/1701451)

修改加速地址：

```bash
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["<https://xxx.mirror.aliyuncs.com>"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

参考链接：[官方镜像加速](https://help.aliyun.com/document_detail/60750.html#title-s0s-jjs-26k)

# 项目部署

## Jar 包方式

使用 IDEA 内置 deployment，上传 jar 包到服务器。

## Docker 方式

在 /usr/lib/systemd/system/docker.service，配置远程访问。

主要是在[Service]这个部分的 ExecStart 中，加上下面两个参数：

```yaml
-H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock
```

然后重启：

```yaml
systemctl daemon-reload
systemctl restart docker
```

在 IDEA 中，连接 Docker：`tcp://ip:2375`。

参考链接：

[Docker开启Remote API 访问 2375端口](https://cloud.tencent.com/developer/article/1683689)

[用idea部署springboot项目到docker_今日相乐，皆当喜欢的博客-CSDN博客_idea部署项目到docker](https://blog.csdn.net/weixin_42687829/article/details/104249583)

# 运行命令

后台运行：`nohup java -jar xxx.jar >temp.txt &`

查看运行的任务：`jobs`

如果想将某个作业调回前台控制，只需要 `fg + 编号` 即可。

查看某端口占用的线程的pid：`netstat -nlp |grep :9181`

查看当前运行的jar包程序进程号：`ps -ef|grep xxx.jar` 或者 `ps -aux | grep java`

关闭进程：`kill -s 9 24204`

参考链接：

[Linux中jar包启动和jar包后台运行的实现方式](https://cloud.tencent.com/developer/article/1722069)

[Java：idea打包jar包&部署到服务器过程全记录](https://lullabychen.github.io/2019/10/09/Java-IDEA打jar包-部署到服务器过程全记录/)

# 项目架构

## 搭建项目

1. 在代码仓库新建项目，clone 到本地。
2. 新建 Maven 工程，编辑 pom.xml 文件。（或使用 [start.spring.io](http://start.spring.io)、IDEA 内置 Initializr）groupId 定义当前 Maven项目隶属的实际项目，artifactId 建议使用实际项目作为前缀。
3. 新建 Modules。



参考链接：[Spring Data JDBC入门使用Demo - 掘金](https://juejin.cn/post/6906453629381804046)
