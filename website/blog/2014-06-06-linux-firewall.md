---
layout: post
title: "Linux Firewall"
description: ""
category: linux
tags: [linux, centos, ubuntu, firewall, iptables, ufw]
---
{% include JB/setup %}

## CentOS

### Clear and init

```sh
iptables -X
iptables -F
service iptables save
service iptables restart
```

### Disable

```sh
sudo service iptables stop
```

### Add rule

```sh
iptables -A INPUT -s 101.227.4.21 -j DROP
service iptables status
service iptables save
service iptables restart
```

### Enable

```sh
sudo service iptables restart
```

## Ubuntu

### Disable

```sh
sudo ufw disable
```

### Enable

```sh
sudo ufw enable
```
