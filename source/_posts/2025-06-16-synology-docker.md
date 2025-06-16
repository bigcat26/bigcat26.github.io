---
title: Synology Docker常用命令以及问题解决
date: 2025-06-16
# top_img: /img/dog.webp
# cover: /img/dog.webp
tags: 
  - Synology
  - docker
categories:
  - Docker

---

## 服务常用命令

### 查询服务状态

```shell
$ sudo systemctl status -l pkg-Docker-dockerd.service
● pkg-Docker-dockerd.service - Docker Application Container Engine
   Loaded: loaded (/usr/local/lib/systemd/system/pkg-Docker-dockerd.service; static; vendor preset: disabled)
   Active: active (running) since Mon 2025-06-16 01:06:55 CST; 8h ago
     Docs: https://docs.docker.com
 Main PID: 17391 (dockerd)
   Memory: 472.0M
```

从输出可以看得出**配置文件路径**: `/usr/local/lib/systemd/system/pkg-Docker-dockerd.service`

### 查询日志

```shell
sudo journalctl -xe |grep docker
```

### 重启服务

```shell
sudo systemctl restart pkg-Docker-dockerd.service
```

## 主要路径

`/var/packages/Docker`

## 常见问题

### docker组件无法启动

1. 查询服务状态，看看是否有报错
2. 删除最后create的容器的subvolumes

```shell
cd /volume1/@docker/btrfs/subvolumes
# 按最后修改日期排列
ls -lart

# 删除可疑文件夹, 如：082fee8f962d1fea5020c50dbab3b8b6c456bd26cdc12bb8f7d43d6cfc7f5c3c
HASH=082fee8f962d1fea5020c50dbab3b8b6c456bd26cdc12bb8f7d43d6cfc7f5c3c
btrfs subvolume delete $HASH
btrfs subvolume delete $HASH-init
```
