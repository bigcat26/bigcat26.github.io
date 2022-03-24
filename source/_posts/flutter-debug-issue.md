---
title: Flutter调试问题: SocketException: Failed to create server socket
date: 2022-03-19 10:00:00
tags: 
  - flutter
---

# Flutter调试问题: SocketException: Failed to create server socket

有段时间没有用flutter，但是一直保持sdk更新。今天用的时候调试报`Reason of Flutter error: SocketException: Failed to create server socket (OS Error: Failed to start accept), address = localhost, port = xxxx`。

这个问题查了下，先是stackoverflow找到一个解决方案，要`ipconfig`查本机ip，然后再用实际ip启动：

```shell
flutter run -d chrome --web-port=8080 --web-hostname= *the value of IPv4 Address*
```

但是没法在Android studio打断点debug。也查到说取消网卡绑定ipv6地址的，不过这个我没试。再试了把网卡ip改为127.0.0.1，也是可以启动的。与AS调试启动的不同只是在于，AS调试用的是localhost。

然后去`github.com/flutter/flutter`爬issue，找到了其原因，大概意思是某次win10更新后localhost默认解释成ipv6，试了下果然

```shell
正在 Ping localhost [::1] 具有 32 字节的数据:
来自 ::1 的回复: 时间<1ms
来自 ::1 的回复: 时间<1ms
来自 ::1 的回复: 时间<1ms
来自 ::1 的回复: 时间<1ms

::1 的 Ping 统计信息:
    数据包: 已发送 = 4，已接收 = 4，丢失 = 0 (0% 丢失)，
往返行程的估计时间(以毫秒为单位):
    最短 = 0ms，最长 = 0ms，平均 = 0ms
```

可能是flutter run不支持ipv6？再深懒得挖了。

那么解决方案

1. 去掉网卡IPv6，按理说ok，但我没试
2. 改debug configuration，加additional run args：`--web-port=8080 --web-hostname=127.0.0.1`。我用的是这个方法，works。

