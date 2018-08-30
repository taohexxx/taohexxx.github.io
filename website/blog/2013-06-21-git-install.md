---
layout: post
title: "Git"
description: ""
category: linux
tags: [linux, git]
---
{% include JB/setup %}

## macOS

Install git

```sh
brew install git
sudo xcodebuild -license
brew install tig
brew install fac  # Easy-to-use CUI for fixing git conflicts
brew install git-flow-avh
git flow init
git flow feature start rss-feed
git flow feature finish rss-feed
```

<https://github.com/petervanderdoes/gitflow-avh>

## 客户端配置

```sh
git config --global user.name "taohe"
git config --global user.email 102320170@qq.com
git config --global core.editor "vim"
git config --global diff.tool vimdiff
git config --global difftool.prompt false
# `git d` aliased to `git difftool`
git config --global alias.d difftool
```

简单明了的使用教程（推荐） [Git笔记-基础](http://omiga.org/blog/archives/1896)

大而全的教程 [open git - 开源书籍](http://opengit.org/open/)

GitHub使用教程 [github的使用](http://progit.org/book/zh/ch4-10.html)

## Linux客户端

### SmartGit（推荐）

[SmartGit](http://www.syntevo.com/smartgit/download.html)

界面不错，功能也比较全，不过可惜是商业软件。个人使用还是完全免费而且没有功能限制的。

### git-cola

Python写的，界面是这里的软件中最丑的，但是文件比较还是这个比较靠谱，在它的Diff主菜单下面有比较功能。

```sh
sudo apt-get install git-cola
```

### fugitive（Vim插件）（推荐）

主要用于本地工作目录和索引、本地仓库之间文件对比等操作

### gitg、QGit

gitg是Gnome界面，比较漂亮。QGit是Qt界面，比较丑。都可以显示文件修改，但是在一个窗口中显示的，很不直观。

```sh
sudo apt-get install qgit
```

### gitk

从名字可以看出来是gtk写的，不能显示文件修改。

```sh
sudo apt-get install gitk
```

## Windows客户端

Git官方客户端（包括GUI版和shell版）、TortoiseGit

## 服务器端配置

### Windows配置Copssh

打开防火墙22端口(非常重要)

安装msysgit、Copssh、TortoiseGit

如果需要用密钥登录，使用msysgit自带的控制台生成密码。

```sh
ssh-keygen -t rsa
```

生成的密码在`%HOME%\.ssh`下，私钥不要删除，把公钥`.pub`文件提交给git服务器管理员处理

### CentOS 配置gitosis

主要参考<http://scie.nti.st/2007/11/14/hosting-git-repositories-the-easy-and-secure-way>

另外可参考<http://www.jiangmiao.org/blog/1600.html>

其中有些地方已经不适用，本文基于CentOS 6.2作了相应的更正

#### 客户端（开发机）

##### 1. 生成~/.ssh/id_rsa.pub，如果已经有，可以直接下一步

```sh
ssh-keygen -t rsa -C "taohex@gmail.com"
```

##### 2. 上传到服务器

```sh
scp ~/.ssh/id_rsa.pub USER_NAME@SERVER_NAME:/tmp/
```

#### 服务器端

以下操作以root用户执行

##### 1. 安装必须软件

```sh
yum -y install git python-setuptools
```

##### *. 找个放git安装文件的地方

```sh
mkdir ~/dev/
cd ~/dev/
```

##### 2. 下载gitosis

```sh
git clone git://eagain.net/gitosis.git
```

##### 3. 安装gitosis

```sh
cd gitosis/
python setup.py install
```

安装成功会显示如下信息

```
...
Finished processing dependencies for gitosis==0.2
```

##### *. 查看增加用户命令的参数

```sh
useradd -h
```

##### 4. 增加git所用的用户

```sh
useradd -r -s /bin/sh -c "git version control" -U -d /home/git -m git
```

##### *. 如果写错了，userdel -f可以删除用户

参考<http://www.php100.com/html/webkaifa/Linux/2009/0803/3116.html>

##### 5. 以新增的用户初始化gitosis

```sh
sudo -H -u git gitosis-init &lt; /tmp/id_rsa.pub
```

成功会显示如下信息

```
Initialized empty Git repository in /home/git/repositories/gitosis-admin.git/
Reinitialized existing Git repository in /home/git/repositories/gitosis-admin.git/
```

##### 6. 修改权限

```sh
sudo chmod 755 /home/git/repositories/gitosis-admin.git/hooks/post-update
```

#### 客户端（开发机）
##### *. 找个地方放gitosis管理配置

```sh
mkdir ~/git/
cd ~/git/
```

##### 1. 把gitosis的配置clone下来

```sh
git clone git@SERVER_NAME:gitosis-admin.git
cd gitosis-admin
```

##### *. 检查`keydir/`下面有什么文件

##### 2. 编辑配置文件

```sh
vim gitosis.conf
```

复制粘贴

```sh
[group gitosis-admin]
writable = gitosis-admin
members = walnet@walnet-Rev-1-0
把group后边改成开发团队名字
把writable后面改成项目名字
members后面加空格再加其他成员名字（或者不变）
```

例如

```sh
[group project1-team]
writable = project1
members = walnet@walnet-Rev-1-0
```

##### 3. 上传配置

```sh
git commit -a -m "Allow members access to new project"
```

成功会显示如下信息

```
...
1 files changed, 4 insertions(+), 0 deletions(-)
```

push到服务器

```sh
git push
```

##### *. 找个地方新建项目文件夹

```sh
mkdir ~/git/
cd ~/git/
```

##### 4. 新建项目文件夹

```sh
mkdir project1/
cd project1/
```

##### 5. 建立git管理

```sh
git init
```

会显示如下信息

```
Initialized empty Git repository in /home/walnet/project1/.git/
```

现在试一下

```sh
touch test.txt
git add . $git commit -m "test.txt"
git remote add origin git@SERVER_NAME:project1.git
git push origin master:refs/heads/master
```

### Ubuntu配置gitosis

主要参考[ubuntu上配置git服务器](http://www.cnblogs.com/xl19862005/archive/2011/06/28/2092464.html)

更正一下

第4步应该是

```sh
su git
cd /home/repo
mkdir teamwork.git
cd teamwork.git
git init --bare
exit
```

第8.1步是多余的，8.3步是正确的

## submodule

update from remote

```sh
git submodule update --remote --merge
git commit
```
