---
title: 在已有的 git 仓库里添加 .gitignore
date: 2018-10-31 11:36:31
tags:
- 工具
- git
---

- 首先 commit 所有的更改
- 然后运行 `git rm -r --cached .`，这条命令会删除目录里的所有东西
- 最后 `git add .` 然后 commit 就可以了
