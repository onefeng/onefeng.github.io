---
layout: wiki
title: CICD
categories: CICD
description: CICD持续集成和持续部署
keywords: CICD
---

## 环境

1.ubuntu 20.04
2.外网服务器
3.jenkins
4.docker

## 操作步骤

基本jenkins+docker持续集成和持续部署，基于github项目触发自动构建和部署。

1.使用docker安装jenkins ，并使用宿主机的docker，保证宿主机docker已登录dockerhub

```shell
docker run -d \
-v /opt/docker/jenkins:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
-v /usr/bin/docker:/usr/bin/docker -p 8080:8080 \
-e TZ=Asia/Shanghai \
-e LANG=C.UTF-8 \
-e LANGUAGE=zh_CN.UTF-8 \
-e LC_ALL=zh_CN.UTF-8 \
-p 50000:50000   --name jenkins --user root   jenkins/jenkins:lts
```

2.`cat /opt/docker/jenkins/secrets/initialAdminPassword` 获取初始密码进入jenkins,选择默认安装的插件,确保安装了github插件。

3.配置github项目，新建一个项目，点击项目的settings，找到`Webhooks`选项卡，点击`Add webhook`，输入jenkins的地址和端口(http://<ip>:8080/github-webhook/)，
Content type选择`application/json`，选择`Just the push event`，点击`Add webhook`按钮。

![webhook配置](/images/blog/img_5.png)

4.配置jenkins接收github推送，点击`系统管理` -> `系统配置` -> `GitHub` -> `Add GitHub Server`，输入github的地址和"Person Access Token".

![github配置](/images/blog/img_6.png)

添加"Secret text"类型凭据，用于github项目的webhook验证。

![添加凭据](/images/blog/img_7.png)

5.配置jenkins构建项目，点击`新建任务` -> `构建一个自由风格的软件项目`，输入项目名称，在源码管理中选择`Git`，
输入项目的github地址，添加"Username with password"凭据，填用户名和"Person Access Token"。

![构建项目](/images/blog/img_8.png)

“Triggers”选择“GitHub hook trigger for GITScm polling”

在"Build Steps"中添加`Execute shell`，输入：

```shell
#!/bin/bash
# 构建镜像
docker build -f Dockerfile -t onefeng/python-app:latest .
docker push onefeng/python-app:latest

ssh -p 61002 user@1.15.152.62 <<EOF
# 创建 /data/workplace 目录
mkdir -p /data/workplace

# 进入 /data/workplace 目录
cd /data/workplace

# 运行容器（如果已存在旧容器，则先删除）
docker stop python-app || true
docker rm python-app || true
docker run -d --name python-app -p 28080:8080 onefeng/python-app:latest
EOF
    
```

上诉shell需要与目标服务器的ssh免密登录配置，并在目标服务器上安装docker。
```shell
# 进入jenkins容器
docker exec -it jenkins /bin/bash
# 生成ssh密钥对
ssh-keygen -t rsa -b 4096
# 复制公钥到目标服务器
ssh-copy-id -p 61002 user@1.15.152.62
```