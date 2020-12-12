---
title: git
---

## Basics
:PROPERTIES:
:heading: true
:background_color: rgb(73, 125, 70)
:END:
### 基本功能图解
:PROPERTIES:
:heading: true
:END:
#### ![图解1](http://marklodato.github.io/visual-git-guide/basic-usage.svg.png)
#### git add files 把当前文件放入暂存区域。 stage 注意： git add 同时具有添加到跟踪列表的功能
#### git commit 给暂存区域生成快照并提交history区
#### git checkout – files 把文件从暂存区域复制到工作目录，用来丢弃本地修改.(注意将会丢弃本地修改.)
### 合并图解
:PROPERTIES:
:heading: true
:END:
#### ![合并](http://marklodato.github.io/visual-git-guide/basic-usage-2.svg.png)
#### git commit -a 相当于运行 git add 把所有当前目录下的文件加入暂存区域再运行 git commit.
#####
#+BEGIN_NOTE
如果文件从没有被跟踪过（untracked）仍然需要用 git add . 先添加
#+END_NOTE
#### git checkout HEAD – files 回滚到复制最后一次提交.
###
## lazygit
## rebase
### 提取我们在A分支上的改动，然后应用在B分支的代码上，完成类似于补丁的功能
###
