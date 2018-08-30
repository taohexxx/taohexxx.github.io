---
layout: post
title: "Backtrack"
description: ""
category: linux
tags: [linux, backtrack, vlc, chromium]
---
{% include JB/setup %}

## vlc

```sh
apt-get install vlc vlc-plugin-pulse mozilla-plugin-vlc
cp /usr/bin/vlc /usr/bin/vlc.bak
hexedit /usr/bin/vlc
```

Use "Tab" key to start ascii string editing. Then press "/" key. Input the search keyword below.

change from

```sh
geteuid._libc_start_main
```

to

```sh
getppid.libc_start_main
```

Press "Ctrl+X" to save and exit.

[Some useful command for hexedit](http://linux.die.net/man/1/hexedit)

## chromium

```sh
apt-get install chromium-browser
```

Then modify the hex of "/usr/lib/chromium-browser/chromium-browser", which is almost the same as vlc.
