---
layout: post
title:  "Rayland-SOLO主板如何接两个网卡"
date:   2017-10-29 07:00:00 +0800
categories: SLA
tags: SLA DLP Rayland-SOLO SongLei
author: Song Lei
---

* content
{:toc}
`Rayland-SOLO` 是我们针对`DLP`应用出的一款主板，有`三个USB口`，所以肯定可以接`3个USB网卡`，目前可以确定的是一定要接两个的。一个是接H3，另一个是接大屏幕。





接好之后，在cmd里面运行
```sh
#adb shell
```
能看到如下的输出
```sh
Z:\下载\adb>adb shell
root@astar-y3:/ # netcfg
netcfg
ip6tnl0  DOWN                                   0.0.0.0/0   0x00000080 00:00:00:
00:00:00
tunl0    DOWN                                   0.0.0.0/0   0x00000080 00:00:00:
00:00:00
sit0     DOWN                                   0.0.0.0/0   0x00000080 00:00:00:
00:00:00
gre0     DOWN                                   0.0.0.0/0   0x00000080 00:00:00:
00:00:00
eth0     UP                               192.168.199.4/24  0x00001043 00:e0:4c:
36:24:cf
eth1     UP                                192.168.99.4/24  0x00001043 00:e0:4c:
36:06:20
lo       UP                                   127.0.0.1/8   0x00000049 00:00:00:
00:00:00
root@astar-y3:/ #
```
接着运行 busybox route命令，可以看到很完美的，网络被分成了两个网段分别是192.168.99.0网段和
192.168.199.0网段，子网掩码都是24个1。
```sh
Z:\下载\adb>adb shell
root@astar-y3:/ # busybox route
busybox route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
192.168.99.0    *               255.255.255.0   U     0      0        0 eth1
192.168.199.0   *               255.255.255.0   U     0      0        0 eth0
root@astar-y3:/ #
```
如果我们把h3连接到eth0上面，并且把h3自己的地址设置为192.168.199.3。
如果我们把大平板连接到eth1上面，并且把大平板自己的地址设置为192.168.99.3。
那么就可以分别访问。可以用ping来验证一下。
这里会用到一个ping的新参数
```sh
#ping -I eth0 192.168.199.3
#ping -I eth1 192.168.99.3
```
用-I参数来指定ping的端口

