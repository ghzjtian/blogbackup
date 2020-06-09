---
title: Git 的学习
tags: [Git]
categories: [Git]
date: 2020-04-01 17:46:19
---

> 记录 Git 一些常用的命令.

<!-- more -->

## 0.参考
* 1.[Git scm](https://git-scm.com/)
* 2.[git-stash用法小结](https://www.cnblogs.com/tocy/p/git-stash-reference.html)
* 3.[GUI Clients](https://git-scm.com/download/gui/mac)


## 1.Git stash

#### 1.显示 Git Stash 列表

```
$ git stash list
stash@{0}: On develop: ABC-613
stash@{1}: On develop: Try to improve the network logic.
stash@{2}: On develop: Add agm/core component.

```

#### 2.Git save

* 1.保存

```
$ git stash save "ABC-602 front-end company UI"
Saved working directory and index state On develop: A3S-602 front-end company UI

```

* 2.然后列表情况.

```
$ git stash list
stash@{0}: On develop: ABC-602 front-end company UI
stash@{1}: On develop: ABC-613
stash@{2}: On develop: Try to improve the network logic.
stash@{3}: On develop: Add agm/core component.

```

#### 3.应用缓存的 stash 

```
# 不删除 stash
$ git stash apply stash@{1}

```

#### 4.移除 stash

```
$ git stash list
stash@{0}: On develop: ABC-602 front-end company UI
stash@{1}: On develop: ABC-613
stash@{2}: On develop: Try to improve the network logic.
stash@{3}: On develop: Add agm/core component.

$ git stash drop stash@{1}
Dropped stash@{1} (c68cea2c0f6e246ac6c8021ef512516491960df9)

$ git stash list
stash@{0}: On develop: ABC-602 front-end company UI
stash@{1}: On develop: Try to improve the network logic.
stash@{2}: On develop: Add agm/core component.

```

## 2.Git 

