---
layout: post
title: ubuntu下jdk安装
tags: jdk ubuntu
categories: java
---

* TOC
{:toc}
## 1. 下载

到[官网](https://www.oracle.com/technetwork/java/javase/downloads/java-archive-javase8u211-later-5573849.html)下载对应版本`.tar.gz`文件即可；

## 2. 安装

1. 卸载openjdk（如果有的话）：

```shell
$: rpm -qa|grep java  # 查询openjdk相关文件
$: rpm -e --nodeps xxx  # 将查询到的全部卸载
```

2. 解压：

```shell
$: tar -zxvf jdk-8u212-linux-x64.tar.gz -C /opt/apps
```


3. 配置环境变量`/etc/profile`：

```shell
export JAVA_HOME=/opt/apps/jdk1.8.0_211
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```

4. 刷新配置：

```shell
$: source /etc/profile
```

5. 验证：

```shell
$: java -version
```

