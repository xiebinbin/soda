---
title: ubuntu_16.04LTS网卡设置记录
date: 2017-01-18 11:02:16
tags:
- ubuntu
categories:
- linux
---
问题描述:
```
ifconfig // 命令 指列出了lo网卡 没有eth0网卡。
```
问题排查:
```
ifconfig -a // 列出所有网卡发现 enp0s3、lo两个网卡
```
3.问题解决
```
ifconfig enp0s3 up//开启网卡

sudo dhclient enp0s3 //更新网卡ip地址
```
编辑文件
```
sudo vi /etc/network/interfaces

# The loopback network interface (配置环回口)

auto lo # 开机自动激lo接口
iface lo inet loopback # 配置lo接口为环回口
# The primary network interface #配置主网络接口

auto eth0 #开机自动激活eth0接口

iface eth0 inet dhcp #配置eth0接口为DHCP自动获取

或者配置eth0为静态地址

# The primary network interface (配置主网络接口)

auto eth0 #开机自动激活eth0接口

iface eth0 inet static #配置eth0接口为静态地址

address 192.168.1.10

gateway 192.168.1.254

Netmask 255.255.255.0

network 192.168.1.0
broadcast 192.168.1.255
```
重启网络服务

```
$sudo /etc/init.d/networking restart 
```

配置DNS
```
#配置DNS服务器的地址，最多可以使用3个DNS服务器

$sudo vim /etc/resolv.conf

nameserver 202.96.134.133
nameserver 202.96.128.68
nameserver 202.96.128.166
```