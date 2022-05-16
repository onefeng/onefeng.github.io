---
layout: post
title: 轻量级portainer界面管理docker swarm集群
category: 运维
tags: [docker]
description: 轻量级portainer界面管理docker swarm集群
keywords: 运维
---

轻量级portainer界面管理docker swarm集群

## 搭建docker swam集群

这里用到2台机器，这里将hostname修改为tp,hs。直接改/etc/hostname分别替换为tp,hs。reboot重启机器。

1.将tp作为maste节点，执行

```shell
docker swarm init
```

然后将输出，将你自己的复制出来
```shell
docker swarm join --token SWMTKN-1-0h0sfcqboutezfxlnn19elpe1xm6k70tlwsr8iysainvq40dd8-4szq6qgvcveebry1mqsvnrwwr 192.168.0.102:2377
```

2.在hs（worker节点）执行上一步复制的值

3.tp节点执行docker node ls

![](/images/posts/docker/img.png)

4.使用docker swarm join-token worker添加worker

![](/images/posts/docker/img_2.png)

## 使用portainer管理docker集群

直接给出排版

```yaml
version: '3.2'

services:
  agent:
    image: portainer/agent
    environment:
       AGENT_CLUSTER_ADDR: tasks.portainer_agent
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /var/lib/docker/volumes:/var/lib/docker/volumes
    networks:
      - agent_net
    deploy:
      mode: global
      placement:
        constraints: [node.platform.os == linux]

  portainer:
    image: portainer/portainer-ce
    command: -H tcp://tasks.portainer_agent:9001 --tlsskipverify
    ports:
      - "9000:9000"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    networks:
      - agent_net
    deploy:
      mode: replicated
      replicas: 1
      placement:
        constraints: [node.hostname == tp] # master节点的hostname

networks:
  agent_net:
    driver: overlay
    attachable: true
```

执行命令
```shell
docker stack deploy -c ./portainer-agent-stack.yml portainer
```

在浏览器上请求 http://ip:9000/，按要求设置admin的密码，界面如下

![](/images/posts/docker/img_1.png)