---
layout: post
title: "LinuxPTP实现时间同步（Ubuntu 2022.2）"
date:   2024-8-6
tags: [Technology]
comments: true
author: DHF1999SD
---

<!-- more -->
# PTP的概念
PTP域中的节点称为时钟节点，PTP协议定义了以下三种类型的基本时钟节点：
OC（Ordinary Clock，普通时钟）：只有一个PTP通信端口的时钟是普通时钟。
BC（Boundary Clock，边界时钟）：有一个以上PTP通信端口的时钟。
TC（Transparentclock，透明时钟）：与BC/OC相比，BC/OC需要与其它时钟节点保持时间同步，而TC则不与其它时钟节点保持时间同步。TC有多个PTP端口，但它只在这些端口间转发PTP协议报文并对其进行转发延时校正，而不会通过任何一个端口同步时间。TC包括以下两种类型：
E2ETC（End-to-End TransparentClock，端到端透明时钟）：直接转发网络中非P2P（Peer-to-Peer，点到点）类型的协议报文，并参与计算整条链路的延时。
P2PTC（Peer-to-PeerTransparent Clock，点到点透明时钟）：只直接转发Sync报文、Follow_Up报文和Announce报文，而终结其它PTP协议报文，并参与计算整条链路上每一段链路的延时。
一般链式的P2P网络选择E2E-TC，而从钟节点较多的网络考虑P2P-TC。因在 P2P 延时测量机制中，延时报文交互是在每条链路的两个端口间进行的，主钟只与直接相连的网络交换设备有延时报文交互，因此在 P2P TC 的延时测量机制中，没有对从钟数量的限制。
主时钟：一个PTP通信子网中只能有一个主时钟。

# 检测网卡是否支持
## 查看网卡是否支持软硬件时间戳
终端输入命令

    sudo ethtool -T eth0

软件时间戳需要包括参数

    SOF_TIMESTAMPING_SOFTWARE
    SOF_TIMESTAMPING_TX_SOFTWARE
    SOF_TIMESTAMPING_RX_SOFTWARE

硬件时间戳需要包括参数

    SOF_TIMESTAMPING_RAW_HARDWARE
    SOF_TIMESTAMPING_TX_HARDWARE
    SOF_TIMESTAMPING_RX_HARDWARE

# LinuxPTP下载
## 下载命令
    sudo git clone git://git.code.sf.net/p/linuxptp/code linuxptp
    cd linuxptp
    sudo make
    sudo make install

## 使用方法
终端输入命令，查看使用方式

    ptp4l -h    

# 程序运行

## 软件时间戳，主从模式测试

服务端（主钟）：
    sudo ptp4l -i enp0s31f6 -m -S (网卡名按照对应连接的实际网卡名进行修改)
客户端（从钟）：
    sudo ptp4l -i eno1 -m -S -s  (网卡名按照对应连接的实际网卡名进行修改)

## 硬件件时间戳，主从模式测试

服务端（主钟）：

    sudo ptp4l -i enp0s31f6 -m -H   （区别在-H）
    
客户端（从钟）：

    sudo ptp4l -i eno1 -m -H -s