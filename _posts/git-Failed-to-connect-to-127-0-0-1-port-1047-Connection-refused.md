---
title: 'git Failed to connect to 127.0.0.1 port 1047: Connection refused'
date: 2018-09-20 16:33:57
tags:
- debug
- git
---

使用服务器 `git clone oh-my-zsh` 时报错：`git Failed to connect to 127.0.0.1 port 1047: Connection refused`，查询后了解到原因在于 git 设置了 代理，而对应端口不能用。

查询是否使用代理：`git config --global http.proxy`

取消代理：`git config --global --unset http.proxy`
