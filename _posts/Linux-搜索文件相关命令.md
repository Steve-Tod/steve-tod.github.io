---
title: Linux 搜索文件相关命令
date: 2018-10-06 10:07:10
tags:
- 工具
- Linux
---

## whereis

简单快速，从数据库中查询，`whereis` 只能搜索二进制文件(-b)，man 帮助文件(-m)和源代码文件(-s)。

<!--more-->

## locate

快而全，通过“ /var/lib/mlocate/mlocate.db ”数据库查找，不过这个数据库也不是实时更新的，系统会使用定时任务每天自动执行 `updatedb` 命令更新一次，所以有时候你刚添加的文件，它可能会找不到，需要手动执行一次 `updatedb` 命令。

## which

小而精，从 PATH 路径里面去搜索

## find

精细搜索

`find` 的格式 `find [path] [option] [action]`

option:

- `-name` 要搜索的内容
- `-mtime` 最后修改的时间
 - `-mtime n`：n 为数字，表示为在 n 天之前的“一天之内”修改过的文件
 - `-mtime +n`：列出在 n 天之前（不包含 n 天本身）被修改过的文件
 - `-mtime -n`：列出在 n 天之内（包含 n 天本身）被修改过的文件
 - `-newer file`：file 为一个已存在的文件，列出比 file 还要新的文件名 
