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

### 常用配置命令

1.固定网络IP配置
```shell
# 查看网关
ip route show default

# 查看DNS
nmcli dev show | grep DNS

# 查看网络设备及链接名称
nmcli dev
# 固定IP配置
nmcli con mod "Wired connection 1" ipv4.addresses 10.0.100.69/24
nmcli con mod "Wired connection 1" ipv4.gateway 10.0.100.254
nmcli con mod "Wired connection 1" ipv4.dns "61.139.2.69,223.5.5.5"
nmcli con mod "Wired connection 1" ipv4.method manual

# 重启网络
nmcli connection down "Wired connection 1"
nmcli connection up "Wired connection 1"
```