---
layout: post
title: "Docusaurus"
description: ""
category: linux
tags: [linux, ubuntu, macos, docusaurus, github]
---
{% include JB/setup %}

## Install Docusaurus

[Docusaurus](https://github.com/facebook/docusaurus)
<https://docusaurus.io/docs/en/commands>

```sh
docusaurus-init
# checkout source branch
git co source
# go to website directory
cd website/
# publish from source branch to master branch and send to GitHub
GIT_USER=taohexxx yarn run publish-gh-pages
```
