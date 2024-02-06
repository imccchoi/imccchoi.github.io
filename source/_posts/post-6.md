---
title: 嵌入式 Linux 开发环境初步搭建
date: 2023-12-13 23:13:01
index_img: /img/post-6.png
tags: 环境搭建
---

> 对于嵌入式 Linux 开发，第一步的难题就是搭建起一个方便易用的开发环境，说着是易用，配置的过程也是较为繁琐，Linux 相比 Windows 之下就是一匹需要驯服的野马。所以本文汇总我使用 iMX6ULL 开发板搭建开发环境的方法和步骤。

## VMware Ubuntu 虚拟机系统安装与配置

1. 安装 VMware Workstation 和 下载准备 Ubuntu 系统镜像（这里采用 22.04 LTS 发行版）。
2. 更换 apt sources.list 软件源（阿里源、清华源等）以改善软件包管理网络访问质量和速度。

## 软件准备

除了VMware 和 Ubuntu.

在Windows下 我们还需要



## 开发环境概要

1. Windows 平台：使用 Visual Studio Code 编辑源代码；利用 SMB 便捷管理与 Linux 文件和其传输。
2. Ubuntu 平台：使用 VMware 虚拟机系统模拟 Linux 开发环境，最主要是在这个交叉编译环境中生成程序文件。
3. iMX6ULL 平台：当然，操作系统也是 Linux，但通过 Windows 使用 MobaXterm 串口通信访问开发板终端操作开发板。

## 开发局域网环境配置

上面介绍的三个平台，那么如何通过 MobaXterm 整合连接这三个平台？
我们采取的方法是配置这三个平台的网络设备在同一个局域网下。

1. 我住所大致的网络拓扑如图所示。（主路由的LAN口连接到二级路由器LAN口并开启中转桥接模式，充当交换机，二级路由在书桌旁边，这样也方便有线连接开发板。）

   {%asset_img 1.png %}

   <br/>

   <br/>

2. Windows：直接连接二级路由器，一般我们就是这样访问互联网，这个不作太多说明。

3. Ubuntu：注意在 Ubuntu 虚拟机系统中，需要配置两种网络设备，第一种是虚拟机系统默认的 NAT 连接方式，Ubuntu 通过 Windows 访问互联网，这个很好理解。第二种就是单独为 Ubuntu 添加虚拟网卡设备，具体配置方式如下。配置完成后，相当于 Ubuntu 的虚拟网卡也成为了局域网中的网络设备。

4. iMX6Ull: 开发板也是直接使用网线连接 ETH 网口（以太网）到二级路由器，与台式主机一样。

最后接线完成后，这三个平台的设备就处于同一局域网中了。
另外建议在主路由器后台管理中，绑定三个设备对应的 MAC 地址，避免 DHCP 频繁变更分配到的 IP 地址。

在 Windows 和 Ubuntu 终端中分别使用 ipconfig 和 ifconfig 命令可以检查对应 IP 地址。
假设三个设备分配的 IP 如下：
Windows: 192.168.0.1
Ubuntu: 192.168.0.2
iMX6ULL: 192.168.0.3

接下使用 ping 命令检查相互之间是否能正常连接即可。

## 交叉编译工具链

## Ubuntu 配置 Samba + Windows 映射路径访问 Ubuntu 文件

## Ubuntu 配置 SSH + MobaXterm SSH 终端访问

## 开发板连接 Windows + MobaXterm Serial 终端访问

## Ubuntu 配置 NFS 挂载目录
