---
layout: wiki
title: docker
categories: docker
description: docker命令
keywords: docker命令
---

## 常用命令

- 构建镜像

```shell
docker build -t test/app:1.0.0 .
```

- 推送镜像到dockerhub

本地镜像为images:1.0

首先docker login输入用户名密码登录
```shell
IMAGE_NAME=coco:latest
REPOSITORY=username/$IMAGE_NAME
docker tag $IMAGE_NAME $REPOSITORY
docker push $REPOSITORY
```


- 设置docker代理地址

```shell
# 创建目录
mkdir -p /etc/systemd/system/docker.service.d

# 编辑配置文件
vim /etc/systemd/system/docker.service.d/http-proxy.conf

# 添加以下内容
[Service]
Environment="HTTP_PROXY=http://10.0.100.86:30001"
Environment="HTTPS_PROXY=http://10.0.100.86:30001"
# 重新加载配置
systemctl daemon-reload
systemctl restart docker
```