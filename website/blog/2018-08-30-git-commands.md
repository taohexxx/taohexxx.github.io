---
layout: post
title: "Git Commands"
description: ""
category: linux
tags: [linux, git]
---
{% include JB/setup %}

## branch

### Basic

```sh
# show
git branch -vv
# delete
git branch -d <branch>
```

### checkout

```sh
# create a new branch as same as current branch
git checkout -b <new branch>
# switch to branch
git checkout <branch>
```

### rebase

```sh
git log
# rebase from tracking remote branch
git rebase
# rebase from branch
git rebase <branch>
# rebase start from a commit
git rebase -i <commit>
# rebase start from last but 4 commit
git rebase -i HEAD~4
# abort
git rebase --abort
```

### other

```sh
# 以当前分支状态新建并复制一个分支new_branch
git branch -B new_branch
# 将分支master用标签v0.8.0的状态覆盖当前状态（将分支master恢复到标签v0.8.0）
git branch -f master v0.8.0
# 用分支some_branch覆盖master
git branch -f master some_branch
# 重命名当前分支
git branch -m new_name
```

## remote

### Basic

```sh
# show
git remote -v
git remote show [remote]
# add
git remote add <remote> <remote url>
# fetch
git fetch [remote]
# pull
git pull [remote]
# push
git push [-f] [remote] [branch]
```

### merge from remote

```sh
git fetch origin
# rebase远程origin仓库的master分支到本分支
git rebase origin/master
```

## submodule

update from remote

```sh
git submodule update --remote --merge
git commit
```

## revert

Discard commit A,B,C by creating a new commit

```sh
git revert --no-commit C B A
git revert --continue
```

Discard changes in working directory. Recover file to last commit

```sh
git checkout -- <file>
```

Checkout file from another branch

```sh
git checkout <branch> -- <file>
```

Undo 'git add'

```sh
git reset <file>
```

Undo a commit

```sh
git reset HEAD~  # HEAD~ is the same as HEAD~1
```

## Tool

### cherry-pick

```sh
git cherry-pick <commit>
# abort
git cherry-pick --abort
```

### mergetool

```sh
git mergetool
```

### assume-unchanged

作用：远程仓库大小写敏感，但本地文件系统大小写不敏感，导致部分文件一直是modified状态，需要忽略。

忽略

```sh
git update-index --assume-unchanged <file>
```

不忽略

```sh
git update-index --no-assume-unchanged <file>
```

## SVN

### Basic

```sh
# only first time
git svn clone -r [commit:]HEAD <svn remote url>
# fetch and rebase
git svn rebase
# push
git svn dcommit
```

### SVN as master and git as branch

```sh
git svn clone -r [commit:]HEAD <svn remote url> [--username=yyy]
git remote add <git remote> <git remote url>
git fetch <git remote>
git checkout -b <git local branch> <git remote branch>
```

Example

```sh
git svn clone -r HEAD https://xxx.com/xxx/trunk/navim --username=yyy
cd navim/
git remote add tencent https://github.com/taohexxx/navim
git fetch tencent
git checkout -b tencent_master tencent/master
```

### Git as master and SVN as branch

```sh
git clone <git remote url>
git svn init <svn remote url>
git svn fetch -r [commit:]HEAD [--username=yyy]
# checkout to local branch
git checkout -b <svn local branch> <svn remote branch>
```

Example

```sh
git clone https://github.com/taohexxx/navim
cd navim/
git svn init https://xxx.com/xxx/trunk/navim
```

`.git/config` should looks like this:

```sh
[svn-remote "svn"]
  url = https://xxx.com/xxx/trunk/navim/navim
  fetch = :refs/remotes/git-svn
```

Then

```sh
git svn fetch -r HEAD
git checkout -b svn_trunk remotes/git-svn
```

### Add another SVN remote

```sh
# edit `.git/config` and copy the `svn` section and rename
git svn fetch -r HEAD <another svn remote>
git checkout -b <another svn local branch> <another svn remote branch>
```

Example

Edit `.git/config` and copy the `svn` section like this:

```sh
[svn-remote "svn_dev"]
  url = https://xxx.com/xxx/branches/navim/navim_dev
  fetch = :refs/remotes/git-svn_dev
```

Then

```sh
git svn fetch -r HEAD svn_dev
git checkout -b svn_dev remotes/git-svn_dev
```

### Delete SVN remote

Delete `svn` section in `.git/config`

```sh
rm -rf .git/svn/refs/remotes/git-svn/
```

### 实例

```sh
# 把svn分支作为本地svn_trunk分支，把git的master分支作为本地的master分支
git clone https://github.com/taohexxx/navim
cd phxrpc/
cat .git/config
git branch -a
# 设置git-svn路径
git svn init https://xxx.com/xxx/trunk/navim
cat .git/config
# 从svn上fetch代码到remotes/git-svn分支
git svn fetch -r HEAD
# 新建一个本地分支svn_trunk，与svn的remotes/git-svn分支对应
git checkout -b svn_trunk remotes/git-svn
git branch -a


# 把svn作为本地master分支，把git的master分支作为本地的git_master分支
git svn clone -r HEAD https://xxx.com/xxx/trunk/navim
cd phxrpc/
cat .git/config
# 设置git路径
git remote add origin https://github.com/taohexxx/navim
cat .git/config
# 从git上fetch代码到remotes/origin/master分支
git fetch origin
# 新建一个本地分支git_master，与git的remotes/origin/master分支对应
git checkout -b git_master remotes/origin/master
# 上面两步等价于这样：从git上pull代码到本地git_master分支
# git pull origin master:git_master
# git checkout git_master
git branch -a
```

<https://www.cnblogs.com/h2zZhou/p/6136948.html>
<http://www.ruanyifeng.com/blog/2014/06/git_remote.html>

## Third-party tools

### gitsome

A supercharged Git/GitHub command line interface (CLI). An official integration for GitHub and GitHub Enterprise

[gitsome](https://github.com/donnemartin/gitsome)

### git-recall

[git-recall](https://github.com/Fakerr/git-recall)

An interactive way to peruse your git history from the terminal

```sh
yarn global add git-recall
```

## remove file from last commit

moving the mistakenly committed files back to the staging area from the previous commit, without cancelling the changes done to them

```sh
# restore to state HEAD~1 (newest - 1 commit) and save changes to index
git reset --soft HEAD~1
# restore to state HEAD (newest commit) and save changes to workspace
git reset HEAD <unwanted_file>
# restore to state HEAD (newest commit) and DROP changes
git reset --hard HEAD <drop_file>
git commit -c ORIG_HEAD
```

## break a previous commit into multiple commits

```sh
git rebase -i 4949c7cbcc0df57f130bafe6bde0
# 1. find the commit you want to break apart
# 2. at the beginning of that line, replace pick with edit (e for short)
# 3. do the same thing as [remove file from last commit](#remove-file-from-last-commit)
# 4. repeat 3 for more commits
git rebase --continue
```

<https://stackoverflow.com/questions/6217156/break-a-previous-commit-into-multiple-commits>

# unstage a deleted file

```sh
# this restores the file status in the index
git reset -- <file>
# then check out a copy from the index
git checkout -- <file>
```

<https://stackoverflow.com/questions/12481639/remove-files-from-git-commit>

## Patch

```sh
git format-patch HEAD~2
git am --ignore-whitespace --ignore-space-change XXX.patch
```

## push

```sh
git push -u origin <branch>
# -u (short for --set-upstream)
git push -f
# force
```

## `HEAD~1` and `HEAD^1`

`HEAD~1` equals to `HEAD^1`

If a node have 2 father, `HEAD~1` or `HEAD^1` is the left father, `HEAD^2` is the right father

```sh
git log --graph
git log 3ff32cccd14a25cf57312e9c61^2
```

## GitHub

### Creating a clean gh-pages branch

```sh
cd /path/to/repo-name
git symbolic-ref HEAD refs/heads/gh-pages
rm .git/index
git clean -fdx
echo "My GitHub Page" > index.html
git add .
git commit -a -m "First pages commit"
git push origin gh-pages
```

