---
layout: wiki
title: linux
categories: linux
description: linux 基础
keywords: linux
---

linux命令


### 小技巧

1.合上笔记本，系统不睡眠
```shell
sudo sed -i 's/#\(HandleLidSwitch=\).*/\1ignore/g' /etc/systemd/logind.conf

# 重启生效
reboot
```