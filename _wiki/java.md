---
layout: wiki
title: java
categories: java
description: java 常用操作记录。
keywords: java
---

## 安装部署


### window下安装

1. 在官网下载jdk,这里使用jdk-8u202-windows-x64.exe

2. 默认安装在C盘，配至系统环境变量。
- set JAVA_HOME=C:\Program Files\Java\jdk1.8.0_202
- set CLASSPATH=.;%JAVA_HOME%\lib;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar
- set Path=;%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin

### linux下安装

```shell script
sudo apt-get install openjdk-8-jdk
```

