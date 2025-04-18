---
title: 记录旧笔记本数据迁移过程
date: 2025-03-27
top_img: /img/notebook.jpg
cover: /img/notebook.jpg
tags:
  - 笔记本
categories:
  - 技术
---

# 迁移旧笔记本数据

## 背景

换了一台m4 mbp做便携开发机，淘汰掉旧的msi 7700+1060，把重要资料备份，然后删了数据重新格式化，然后挂咸鱼。

## 迁移步骤

### scoop应用列表

MSI上安装了这堆scoop应用，不管有没有用，先mark着。

```text
Name            Version               Source Updated             Info
----            -------               ------ -------             ----
7zip            24.09                 main   2024-12-09 00:40:23
adb             35.0.2                main   2024-12-09 00:40:26
adopt8-hotspot  8u292-b10             java   2021-06-10 23:41:49
android-sdk     4333796               extras 2021-09-04 11:45:24
arduino         2.3.4                 extras 2024-12-09 00:41:51
audacity        3.7.3                 extras 2025-03-14 22:58:41
corretto11-jdk  11.0.26.4.1           java   2025-03-07 21:42:16
corretto21-jdk  21.0.6.7.1            java   2025-03-10 21:02:20
cpu-z           2.15                  extras 2025-03-20 20:31:14
crystaldiskinfo 9.6.3                 extras 2025-03-12 20:39:59
cwrsync         6.4.1                 main   2025-03-27 19:53:02
dbeaver         25.0.0                extras 2025-03-10 21:05:55
depends         2.2                   extras 2022-10-05 11:48:04
everything      1.4.1.1026            extras 2024-08-03 23:43:15
fastcopy        5.8.1                 extras 2025-03-10 21:06:16
ffmpeg          7.1.1                 main   2025-03-14 20:30:06
flutter         3.29.2                extras 2025-03-23 01:16:47
go              1.24.1                main   2025-03-10 21:24:27
hdtunepro       5.7.5                 zapps  2021-06-12 22:25:40
hxd             2.5.0.0               extras 2021-07-13 00:50:30
idea            2024.3.5-243.26053.27 extras 2025-03-23 01:23:32
innounp         2.64.3                main   2025-03-23 01:23:35
joplin          3.2.13                extras 2025-03-10 21:32:04
llvm            20.1.0                main   2025-03-10 21:34:35
mobaxterm       25.1                  extras 2025-03-23 01:24:07
mqttx           1.11.1                extras 2024-12-20 23:52:22
ninja           1.12.1                main   2024-06-23 01:50:36
nodejs          23.10.0               main   2025-03-14 23:01:46
notepadplusplus 8.7.8                 extras 2025-03-10 21:36:41
Obsidian        1.7.7                 extras 2024-12-21 00:01:26
pandoc          3.6.2                 main   2025-01-18 00:36:36
perl            5.40.0.1              main   2024-09-16 20:10:16
postman         11.28.4               extras 2025-01-18 20:59:14
potplayer       241211                extras 2024-12-21 00:04:28
processhacker   2.39                  extras 2021-05-05 12:25:17
rustup          1.27.1                main   2024-06-29 00:46:25
scrcpy          3.1                   main   2024-12-21 00:04:53
sing-box        1.10.7                main   2025-01-18 00:38:21
snipaste        2.10.4                extras 2025-01-18 00:38:48
spacesniffer    1.3.0.2               extras 2021-06-12 22:30:12
springboot      3.4.1                 extras 2024-12-26 21:46:18
temurin8-jdk    8.0.432-6             java   2024-12-26 21:47:38
terminus        1.0.143               extras 2021-06-30 01:07:35
v2rayn          6.45                  extras 2024-06-21 23:42:23
vlc             3.0.21                extras 2024-06-29 00:48:39
vncviewer       7.13.1                extras 2024-12-26 21:47:50
wixtoolset      5.0.2                 main   2024-10-07 00:58:32
wxhexeditor     0.24                  extras 2021-05-05 12:31:07
xnviewmp        1.8.3                 extras 2024-12-26 21:52:00
```

## 大文件同步

有个game文件夹压缩包相当大，达到了60g+。直接拷非常慢，但是又懒得接网线。于是采取rsync同步，可以续传，上面安装的cwrsync就是为了这个目的。命令如下：

```shell
rsync --progress --partial  "GAME.zip" rsync://NAS/inbox/
```
