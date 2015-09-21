---
layout: post
title: "ssh基本设置，及authorized_keys"
description: ""
category: 
tags: [ssh]
---
{% include JB/setup %}

主要是Linux下的配置，win下请出门左转Google。

#### sshd

远程服务器安装ssh server后，可以从本地`ssh remote_user@remote_host`连接至远程服务器。

##### 安装sshd

ubuntu 一句话

    #在远程服务器上 
    sudo apt-get install openssh-server

##### 配置启动项

推荐使用`sysv-rc-conf`，安装：

    #在远程服务器上 
    sudo apt-get install sysv-rc-conf

界面是这样的：

    ┌ SysV Runlevel Config   -: 关闭服务  =/+: 开启服务  h: 帮助  q: 退出              ┐
    │                                                                              │
    │ service      1       2       3       4       5       0       6       S       │
    │ ---------------------------------------------------------------------------- │
    │ killprocs   [X]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]      │
    │ rsyslog     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]      │
    │ saned       [ ]     [X]     [X]     [X]     [X]     [ ]     [ ]     [ ]      │
    │ sendsigs    [ ]     [ ]     [ ]     [ ]     [ ]     [X]     [X]     [ ]      │
    │ single      [X]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]      │
    │ speech-di$  [ ]     [X]     [X]     [X]     [X]     [ ]     [ ]     [ ]      │
    │ ssh         [ ]     [X]     [ ]     [ ]     [ ]     [ ]     [ ]     [ ]      │
    │                                                                              │
    └──────────────────────────────────────────────────────────────────────────────┘
    ┌──────────────────────────────────────────────────────────────────────────────┐
    │ Use the arrow keys or mouse to move around.      ^n: 后一页     ^p: 前一页     │
    │                        space: 切换服务开关                                     │
    └──────────────────────────────────────────────────────────────────────────────┘

##### 制作密匙

    #在本地
    ssh-keygen -t rsa -C username@RemoteHost -f ~/.ssh/RemoteHost_rsa

`-t`指定了密匙种类，`username@RemoteHost`是密匙注释，`-f`指定了密匙和公匙存放位置。

该命令生成了一组公匙`RemoteHost_rsa.pub`和密匙`RemoteHost_rsa`。

##### 上传公匙

将`RemoteHost_rsa.pub`加入远程服务器的`~/.ssh/authorized_keys`，就可以实现ssh免密码直接登录。

    #在本地
    #将公匙上传至远程服务器
    scp ~/.ssh/RemoteHost_rsa.pub username@RemoteHost:
    #登录远程服务器
    ssh username@RemoteHost
    #将公匙添加至authorized_keys
    #注意authorized_keys权限必须为 600
    cat RemoteHost_rsa.pub >> ~/.ssh/authorized_keys
    #删除公匙
    rm RemoteHost_rsa.pub
    exit

这样做很麻烦，后面讲一个简单的。

##### 设置别称

每次登录远程服务器都要在本地输入`ssh username@RemoteHost`这样比较麻烦，ssh提供了更简单的方法，可以直接`ssh alternate_name`。

    #在本地
    vim ~/.ssh/config

添加

    Host alternate
        HostName RemoteHostOrIP
        User username
        IdentityFile ~/.ssh/RemoteHost_rsa.pub
    
保存

以后就可以使用`ssh alternate_name`直接登录了

##### 禁止密码登录

现在已经给远程服务器添加了用户公匙、别名，就不用使用密码登录了，安全起见，可以禁止远程服务器使用密码登录。

    #在本地
    ssh alternate_name
    vim /etc/ssh/sshd_config

将`PasswordAuthentication`设置为`no`。

    ssh username@RemoteHost
    #Permission denied (publickey).

#### 远程服务器添加authorized_key

在本地生成公匙密匙对后，将公匙加入`authorized_keys`的过程比较繁琐，可以用这一行命令来“简单”的添加。

    cat ~/.ssh/RemoteHost_rsa.pub | ssh remote "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"

根据该命令制作脚本，向`~/bin`中加入`ssh-copy-pub`。注意，此时远程服务器上必须有`~/.ssh/authorized_keys`文件，否则要创建该文件，并设置权限`chmod 600 ~/.ssh/authorized_keys`。

    #!/bin/bash
    
    if [ $# == 1 ]
    then
        echo "cat ~/.ssh/id_rsa.pub | ssh $1 \"mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys\""
        $(cat ~/.ssh/id_rsa.pub | ssh $1 "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys")
    elif [ $# == 2 ]
    then
        echo "cat ~/.ssh/${1}_rsa.pub | ssh $2 \"mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys\""
        $(cat ~/.ssh/${1}_rsa.pub | ssh $2 "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys")
    else
        echo ssh-copy-pub [user@host]
        echo append "~/.ssh/id_rsa.pub" to {user@host}:/.ssh/authorized_keys
        echo
        echo ssh-copy-pub [pub user@host]
        echo append "~/.ssh/{pub}_rsa.pub" to {user@host}:/.ssh/authorized_keys
    fi

保存

    #将id_rsa.pub拷贝至alternate_name的authorized_keys中
    ssh-copy-pub alternate_name
    
    #将id_rsa.pub拷贝至username@RemoteHost的authorized_keys中
    ssh-copy-pub username@RemoteHost
    
    #将RemoteHost_rsa.pub拷贝至alternate_name的authorized_keys中
    ssh-copy-pub RemoteHost alternate_name
    
    #将RemoteHost_rsa.pub拷贝至username@RemoteHost的authorized_keys中
    ssh-copy-pub RemoteHost username@RemoteHost
    
全文完。
