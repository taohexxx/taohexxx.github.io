---
layout: post
title: "Linux Server"
description: ""
category: linux
tags: [linux, server]
---
{% include JB/setup %}

## AWS

```sh
ssh -i ~/workspace/amazon/taohe-key-pair-us-west-2.pem ec2-user@ec2-50-112-154-81.us-west-2.compute.amazonaws.com
```

## [CentOS](/blog/centos/)

## Ubuntu Server

### network

```sh
vim /etc/networks/eth0
static ip 192.168.0.81
```

参考[Redhat与Ubuntu系统的网卡配置方法比较](http://net.zdnet.com.cn/network_security_zone/2008/0704/964525.shtml)

### install desktop (not vnc)

```sh
sudo apt-get install --no-install-recommends ubuntu-desktop
sudo apt-get install language-pack-zh-hans
```
