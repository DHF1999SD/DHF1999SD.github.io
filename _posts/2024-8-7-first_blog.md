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
    sudo apt-get install linuxptp

# 检测网卡是否支持
## 查看网卡是否支持软硬件时间戳

    sudo ethtool -T eth0
    
