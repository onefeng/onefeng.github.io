---
layout: wiki
title: docker常用命令
categories: docker
description: docker命令
keywords: docker命令
---

## 常用命令

- 推送镜像到dockerhub

本地镜像为images:1.0

首先docker login输入用户名密码登录
```shell
IMAGE_NAME=coco:latest
REPOSITORY=username/$IMAGE_NAME
docker tag $IMAGE_NAME $REPOSITORY
docker push $REPOSITORY
```