---
title: Linux 命令执行顺序控制与管道
date: 2018-10-13 00:08:59
tags:
- 工具
- Linux
---

## 命令的顺序执行

### 简单顺序执行

用 `;` 分割命令即可。

<!--more-->

### 条件顺序执行

``` bash
$ which cowsay>/dev/null && cowsay -f head-in ohch~
```

上面的 `&&` 表示如果前面的命令执行结果返回 0 则执行后面的，否则不执行。可以从 `$?` 环境变量获取上一次的返回结果。

对应的， `||` 表示果前面的命令执行结果返回非 0 则执行后面的。

## 管道

管道是一种通信机制，通常用于进程间的通信（也可通过socket进行网络通信），它表现出来的形式就是将前面每一个进程的输出(stdout)直接作为下一个进程的输入(stdin)。

在命令行中用 `|` 分割符表示，如：

```bash
$ ls -al /etc | less
```

这样就可以一行一行地查看 `ls` 的输出。

### cut 命令

打印 `/etc/passwd` 文件中以 `:` 为分隔符的第1个字段和第6个字段分别表示用户名和其家目录：

```bash
$ cut /etc/passwd -d ':' -f 1,6
```

打印 `/etc/passwd` 文件中每一行的前 N 个字符：

```bash
# 前五个（包含第五个）
$ cut /etc/passwd -c -5
# 前五个之后的（包含第五个）
$ cut /etc/passwd -c 5-
# 第五个
$ cut /etc/passwd -c 5
# 2到5之间的（包含第五个）
$ cut /etc/passwd -c 2-5
```

### grep 命令

`grep` 的一般形式为：

```bash
grep [命令选项]... 用于匹配的表达式 [文件]...
```

```bash
$ grep -rnI "txt" ~
```

上面这是在家目录下搜索所有包含 txt 的文件，`-r` 参数表示递归搜索子目录中的文件，`-n`表示打印匹配项行号，`-I` 表示忽略二进制文件。

```bash
# 查看环境变量中以 "lxd" 结尾的字符串
$ export | grep ".*lxd$"
```

### wc 命令

`wc` 命令用于统计并输出一个文件中行、单词和字节的数目。

```bash
# 行数
$ wc -l /etc/passwd
# 单词数
$ wc -w /etc/passwd
# 字节数
$ wc -c /etc/passwd
# 字符数
$ wc -m /etc/passwd
# 最长行字节数
$ wc -L /etc/passwd
```

### uniq 命令

`uniq` 命令可以用于过滤或者输出重复行。

我们可以使用history命令查看最近执行过的命令（实际为读取${SHELL}_history文件,如我们环境中的~/.zsh_history文件），不过你可能只想查看使用了哪个命令而不需要知道具体干了什么，那么你可能就会要想去掉命令后面的参数然后去掉重复的命令：

```bash
$ history | cut -c 8- | cut -d ' ' -f 1 | uniq
```

因为 `uniq` 命令只能去连续重复的行，不是全文去重，所以要达到预期效果，我们先排序：

```bash
$ history | cut -c 8- | cut -d ' ' -f 1 | sort | uniq
# 或者$ history | cut -c 8- | cut -d ' ' -f 1 | sort -u
```
