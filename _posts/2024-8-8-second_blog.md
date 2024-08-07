---
layout: post
title: "ATS原理"
date:   2024-8-7
tags: [Concept]
comments: true
author: DHF1999SD
---

<!-- more -->
## ATS的概念


## 检测网卡是否支持
### 查看网卡是否支持软硬件时间戳


## LinuxPTP
### 下载命令
    sudo git clone git://git.code.sf.net/p/linuxptp/code linuxptp
    cd linuxptp
    sudo make
    sudo make install

### 使用方法
终端输入命令，查看使用方式

    ptp4l -h    

## 程序运行

### 软件时间戳，主从模式测试

服务端（主钟）：
    sudo ptp4l -i enp0s31f6 -m -S (网卡名按照对应连接的实际网卡名进行修改)
客户端（从钟）：
    sudo ptp4l -i eno1 -m -S -s  (网卡名按照对应连接的实际网卡名进行修改)

### 硬件件时间戳，主从模式测试

服务端（主钟）：

    sudo ptp4l -i enp0s31f6 -m -H   （区别在-H）

客户端（从钟）：

    sudo ptp4l -i eno1 -m -H -s