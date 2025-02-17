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

#### 固定网络IP配置
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

#### 挂载磁盘

1.查看磁盘信息

```shell
fdisk -l
```

2.创建分区

如果磁盘没有分区，你需要先在新的磁盘上创建一个分区。假设新磁盘是 /dev/sdb，你可以使用 fdisk 工具来创建分区

```shell
sudo fdisk /dev/sdb
```

然后根据提示进行操作：

输入 n 创建新分区。
输入 p 选择主分区。
按照提示选择分区大小和其他设置。
输入 w 保存更改并退出。

3.格式化分区

创建分区后，你需要格式化它。假设你创建的是 /dev/sdb1，可以使用 mkfs 命令进行格式化（选择你需要的文件系统格式，常见的是 ext4）：

```shell
sudo mkfs.ext4 /dev/sdb1
```

4.创建挂载点

```shell
sudo mkdir /data
```

5.挂载磁盘

```shell
sudo mount /dev/sdb1 /data
```

6.设置开机自动挂载

编辑 /etc/fstab 文件，添加一行：

首先获取磁盘 UUID：

```shell
sudo blkid /dev/sdb1
```

复制显示的 UUID（例如：UUID="xxxx-xxxx"）。然后编辑 /etc/fstab 文件：

```shell
UUID=xxxx-xxxx /data ext4 defaults 0 2
```

保存并退出。