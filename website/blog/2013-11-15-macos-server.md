---
layout: post
title: "macOS Server"
description: ""
category: macos
tags: [macos, php, mysql]
---
{% include JB/setup %}

## PHP

[Nginx](/blog/nginx/)

## MySQL

### Install MySQL

```sh
brew options mysql
brew install mysql --enable-debug
```

### Let launchd Start MySQL at Login

**CAUTION:** mysqld takes up 500 MB memory!

```sh
ln -sfv /usr/local/Cellar/mysql/5.6.17_1/*.plist ~/Library/LaunchAgents/
```

### Load MySQL

Option 1 (suggested)

```sh
sudo gem install lunchy
lunchy list
lunchy stop mysql
lunchy start mysql
```

Option 2

```sh
launchctl unload -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
launchctl load -w ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
```

[Brew’ing PHP, MySQL & Nginx on Mac OS X](http://rtcamp.com/tutorials/mac/osx-brew-php-mysql-nginx/)

## Redis

```sh
brew install redis
```

To have launchd start redis at login:

```sh
ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
```

Then to load redis now:

```sh
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
```

Or, if you don't want/need launchctl, you can just run:

```sh
redis-server /usr/local/etc/redis.conf
```
