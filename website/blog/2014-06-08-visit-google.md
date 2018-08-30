---
layout: post
title: "上Google的方法"
description: ""
category: linux
tags: [linux, nslookup, gfw, google]
---
{% include JB/setup %}

## 方法

<a href="https://s3-us-west-2.amazonaws.com/google2/index.html" target="_blank">https://s3-us-west-2.amazonaws.com/google2/index.html</a>

Google镜像，可以直接搜索。

如果搜索结果加载较慢，可以在搜索结果出来之后，把浏览器地址栏中的IP地址（注意只是数字表示的IP地址，例如`123.123.123.123`，不是整个网址）复制下来。以后直接在浏览器输入这个IP地址，回车，就能上Google了。等到这个IP地址被封了之后，再来重新生成一个。

## 其它地址

<a href="https://s3-ap-southeast-1.amazonaws.com/google.cn/index.html" target="_blank">https://s3-ap-southeast-1.amazonaws.com/google.cn/index.html</a>

<a href="https://s3-us-west-1.amazonaws.com/google3/index.html" target="_blank">https://s3-us-west-1.amazonaws.com/google3/index.html</a>

<a href="https://s3-eu-west-1.amazonaws.com/google4/index.html" target="_blank">https://s3-eu-west-1.amazonaws.com/google4/index.html</a>

<a href="https://s3-ap-northeast-1.amazonaws.com/google5/index.html" target="_blank">https://s3-ap-northeast-1.amazonaws.com/google5/index.html</a>

<a href="https://s3-ap-southeast-2.amazonaws.com/google6/index.html" target="_blank">https://s3-ap-southeast-2.amazonaws.com/google6/index.html</a>

<a href="https://s3-sa-east-1.amazonaws.com/google7/index.html" target="_blank">https://s3-sa-east-1.amazonaws.com/google7/index.html</a>

<a href="https://s3.amazonaws.com/google./index.html" target="_blank">https://s3.amazonaws.com/google./index.html</a>

<a href="https://s3-ap-southeast-1.amazonaws.com/google.cn/index.html" target="_blank">https://s3-ap-southeast-1.amazonaws.com/google.cn/index.html</a>

<a href="http://wen.lu/" target="_blank">wen.lu</a>

<a href="http://chetxia.co/" target="_blank">chetxia.co</a>

<a href="http://luxtarget.co/" target="_blank">luxtarget.co</a>

<a href="http://1kapp.co/" target="_blank">1kapp.co</a>

[Google全球IP地址库](http://my.oschina.net/haodut/blog/278535)

[Google全球IP地址库](http://www.zmrbk.com/tool/google.html)

[科学的使用Google](http://zhangziyou.sinaapp.com/919.html)

## ~~通过IP地址直接上Google~~（本方法已经失效）

~~在<a href="http://88.159.13.209/" target="_blank"/>88.159.13.209</a>到<a href="http://88.159.13.215/" target="_blank"/>88.159.13.215</a>范围内的IP地址，都是Google服务器。在浏览器上输入其中任意一个IP就可以上Google了。比如，输入<a href="http://88.159.13.209/" target="_blank"/>88.159.13.209</a>，按回车。把这个地址添加都收藏夹，以后上就方便了。~~

## ~~寻找更多可用的IP地址~~（本方法已经失效）

~~在macOS或者Linux（Windows需要安装nslookup相关软件）中打开Terminal，输入~~

```sh
nslookup www.google.cn
```

~~它会列出几个可用的IP地址。~~
