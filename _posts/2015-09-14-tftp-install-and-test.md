---
layout: post
title: "在ubuntu下安装tftp服务"
description: ""
tags: [tftp, 安装]
---
{% include JB/setup %}

#### 安装

ubuntu下安装特别简单

    sudo apt-get install tftpd tftp openbsd-inetd

#### 配置

配置文件在`/etc/inetd.conf`

    tftp dgram udp wait nobody /usr/sbin/tcpd /usr/sbin/in.tftpd /srv/tftp 

默认的文件路径为`/srv/tftp`，可以进入`/srv`
    
    sudo ln -s ~/your-dir tftp

建立软连接

#### 重启服务

    sudo /etc/init.d/openbsd-inetd restart

#### 测试

查看69端口是否打开`netstat -an | grep udp`

    udp 0 0 0.0.0.0:69 0.0.0.0:*

说明已开启。

传输测试`echo some-word > ~/your-dir/a-file.txt`

    mkdir testtftp
    cd testtftp
    tftp localhost
    tftp> get a-file.txt
    Received 11 bytes in 0.1 seconds
    tftp> quit
    cat a-file.txt
    some-word

成功。
