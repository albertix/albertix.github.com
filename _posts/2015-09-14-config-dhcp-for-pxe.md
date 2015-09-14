---
layout: post
title: "config dhcp for pxe"
description: ""
category: 
tags: []
---
{% include JB/setup %}

#### 安装dhcp

ubuntu 14.04中，安装`dhcp`只需

    sudo apt-get install isc-dhcp-server

#### 配置

有两个配置文件分别是`/etc/default/isc-dhcp-server`，为`eth0`添加dhcp服务，将`INTERFACES=""`改为`INTERFACES="eth0"`。

和`/etc/default/isc-dhcp-server`，配置subnet

    subnet 192.168.1.0 netmask 255.255.255.0 {
      range 192.168.1.210 192.168.1.220;
      option routers 192.168.1.201; #网关   
      next-server 192.168.1.201;
      filename "pxelinux.0";
    }

更改`eth0` ip为`192.168.1.201`。

#### 启动

    sudo service isc-dhcp-server start
