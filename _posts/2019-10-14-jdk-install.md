---
layout: post
title: ubuntu下jdk安装
tags: jdk ubuntu
categories: Java
---

* TOC
{:toc}

## 1. 下载

到[官网](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8u211-later-5573849.html)下载对应版本`.tar.gz`文件即可；

## 2. 安装

2.1 卸载openjdk（如果有的话）：

```shell
$: rpm -qa|grep java  # 查询openjdk相关文件
$: rpm -e --nodeps xxx  # 将查询到的全部卸载
```

2.2 解压：

```shell
$: tar -zxvf jdk-8u212-linux-x64.tar.gz -C /opt/apps
```

2.3 配置环境变量`/etc/profile`：

```shell
export JAVA_HOME=/opt/apps/jdk1.8.0_211
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

2.4 刷新配置：

```shell
$: source /etc/profile
```

2.5 验证：

```shell
$: java -version
```

补充：在hadoop源码的building.txt中，关于oracle jdk（和maven）的安装：

```txt
...
Installing required packages for clean install of Ubuntu 14.04 LTS Desktop:

* Oracle JDK 1.8 (preferred)
  $ sudo apt-get purge openjdk*
  $ sudo apt-get install software-properties-common
  $ sudo add-apt-repository ppa:webupd8team/java
  $ sudo apt-get update
  $ sudo apt-get install oracle-java8-installer
* Maven
  $ sudo apt-get -y install maven
...
```

