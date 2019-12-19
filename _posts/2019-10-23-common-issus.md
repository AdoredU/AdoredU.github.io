---
layout: post
title: 常见问题记录
tags: 常见问题
categories: 问题记录
---

### 1. linux运行idea报错

- 问题：linux启动idea/pycharm时报错：

  ```txt
  Can't connect to X11 window server using ':0.0' as the value of the DISPLAY...
  ```

- 原因：非管理员用户启动图形界面应用至linux桌面时需要权限。

- 解决：

  - 切换至root用户（未初始化root用户时先执行`sudo passwd root`进行初始化）：`su root`；
  - root用户执行：`xhost +`；
  - 切换至普通用户正常启动。

### 2. MAC使用VMware Fusion安装虚拟机时间同步

- 设置 —> 高级 —> 勾选"同步时间"。