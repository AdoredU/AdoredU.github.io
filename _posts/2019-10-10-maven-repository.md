---
layout: post
title: ubuntu搭建nexus3私服
tags: java maven 私服 nexus3
categories: Java
---

* TOC
{:toc}

## 0. 介质

- ubuntu版本：ubuntu-16.04.6-desktop-amd64；
- nexus：nexus-3.19.1-01，[下载地址](https://cms2.hubspot.com/ctas/v2/public/cs/c/?cta_guid=b308aaca-ab41-4544-ba23-c53c1b469e0d&placement_guid=bde424ac-b77c-4799-913d-9d0db86ef1f8&portal_id=1958393&canon=https%3A%2F%2Fwww.sonatype.com%2Fnexus-repository-oss&redirect_url=APefjpEuSRBNm3_BBOnPkjV1fkK1VBQ76oQibd9S20VMwVDkBybQzwsUujMG3wisnuiSEpaJWr-rJEYf_48b0WbG45j43gRuS_MOZJZIzc19Gv9JC1Z4BlZ6oQ77Vv6VLxmlfh2Wv4Mb5AAAmBW_hwXOs9e5tCyXgw&click=d23eb35c-cfdb-40d3-8214-d27132068e2c&hsutk=d5663919ae15dcaa87721fee3e2b7b05&signature=AAH58kHPOKq9MgvEoz6i0qLyu1rlxclUHw&utm_referrer=https%3A%2F%2Fwww.sonatype.com%2Fproduct-nexus-repository&pageId=11385810717&__hstc=31049440.d5663919ae15dcaa87721fee3e2b7b05.1570697935016.1570697935016.1570697935016.1&__hssc=31049440.1.1570697935016&__hsfp=3522642101&contentType=standard-page)；

## 1. 安装

- 下载nexus-3.19.1-01-unix.tar.gz，放至`~/software`目录；

- `tar -zxvf nexus-3.19.1-01-unix.tar.gz `解压：

  ![image-20191010184115821](/Users/gp/Desktop/github_projects/AdoredU.github.io/_posts/assets/image-20191010184115821.png)

  ```
  - nexus-3.19.1-01：nexus主目录，包括bin目录；
  - sonatype-work：工作目录，包含配置文件和日志等。
  ```

## 2. 启动

- `cd nexus-3.19.1-01/bin`

- `./nexus`

  ![image-20191010184857350](/Users/gp/Desktop/github_projects/AdoredU.github.io/_posts/assets/image-20191010184857350.png)

  可以看到，启动、停止、重启等命令；

- `./nexus start`，等待片刻即可启动成功；

  日志可以在`/home/gp/software/sonatype-work/nexus3/log`下通过`tail`命令查看。

- 访问：nexus默认端口为8081，在本地输入虚拟机地址或者直接在虚拟机输入`http://localhost:8081`即可访问nexus（启动页很酷）：

  ![image-20191010185318816](/Users/gp/Desktop/github_projects/AdoredU.github.io/_posts/assets/image-20191010185318816.png)

## 3. 登录

- 点击右上角Sigh in，弹框：

  ![image-20191010185648413](/Users/gp/Desktop/github_projects/AdoredU.github.io/_posts/assets/image-20191010185648413.png)

- 根据提示打开`/home/gp/software/sonatype-work/nexus3/admin.password`，复制里面内容（初始密码）；

- 输入`admin`，粘贴复制的密码，即可登录；

- 首次登录成功后，弹框初始化设置，共4步，第2步设置新密码，第3步设置是否允许匿名访问，建议默认（不允许），第4步Finish；

## 4. 仓库

![image-20191011112604146](/Users/gp/Desktop/github_projects/AdoredU.github.io/_posts/assets/image-20191011112604146.png)

- 组仓库默认包含`maven-releases`和`maven snapshots`，以及`maven-central`，这样在依赖Jar包时既可以优先从私服下载，也可以从中央仓库下载；
- 组仓库包含仓库可以在设置中设置；
- 一般私服使用的就是组仓库；

## 5. 上传

- 配置用户：在mvn设置文件settings.xml中，servers节点添加用户。

  ```xml
  ...
  <server>
    <id>releases</id>
    <username>admin</username>
    <password>YOUR NEXUS PASSWORD</password>
  </server>
  <server>
    <id>snapshots</id>
    <username>admin</username>
    <password>YOUR NEXUS PASSWORD</password>
  </server>
  ...
  ```

- 在项目pom文件中配置上传路径：

  ```xml
  ...
  <distributionManagement>
      <repository>
          <!-- 注意id要与server节点一致 -->
          <id>releases</id>
          <url>http://YOUR NEXUS IP:8081/repository/maven-releases/</url>
      </repository>
      <snapshotRepository>
          <id>snapshots</id>
          <url>http://YOUR NEXUS IP}:8081/repository/maven-snapshots/</url>
      </snapshotRepository>
  </distributionManagement>
  ...
  ```

- 执行发布命令：`mvn deploy`。

## 4. 下载

- 配置用户（组仓库）：

  ```xml
  ...
  <server>
    <id>public</id>
    <username>admin</username>
    <password>YOUR NEXUS PASSWORD</password>
  </server>
  ...
  ```

- 配置模板：

  ```xml
  <profile>
    <!-- profile的id -->
    <id>dev</id>
    <repositories>
      <repository>
        <!-- 仓库的id，repositories可以配置多个仓库，保证id不重复 -->
        <id>public</id>
        <!-- 仓库地址，即nexus仓库组的地址 -->
        <url>http://YOUR NEXUS IP:8081/repository/maven-public/</url>
        <!-- 是否下载releases构件 -->
        <releases>
          <enabled>true</enabled>
        </releases>
        <!-- 是否下载snapshots构件 -->
        <snapshots>
          <enabled>true</enabled>
        </snapshots>
      </repository>
    </repositori
    <pluginRepositories>
      <!-- 插件仓库，maven运行依赖插件，也需要从私服下载 -->
      <pluginRepository>
        <!-- 插件仓库的id不允许重复，否则后面的会覆盖前面的 -->
        <id>public</id>
        <name>Public Repositories</name>
        <url>http://YOUR NEXUS IP:8081/repository/maven-public/</url>
      </pluginRepository>
    </pluginRepositori
  </profile>
  ```

- 激活模板：

  ```xml
  <activeProfiles>
    <!-- 模板id -->
    <activeProfile>dev</activeProfile>
  </activeProfiles>
  ```

- 项目中pom文件添加依赖，即可正常依赖。
