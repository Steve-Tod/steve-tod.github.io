---
title: 解决'X11 forwarding request failed on channel 0'
date: 2018-10-02 15:25:42
tags:
- debug
- X11
---

今天试图将服务器上的图片用 `feh` 和 XQuartz 转发到本地显示的时候，发现转发不了，报错：

```
feh ERROR: Can't open X display.  It *is* running, yeah?
```

在网上找了半天也没找到解决方案，后来仔细观察，发现

```bash
$ ssh -CAXY user@server
```

登录之后，会报错：

```
X11 forwarding request failed on channel 0
```

<!--more-->

上网调研后发现解决办法：首先在服务器上修改配置：

```bash
$ sudo vi /etc/ssh/sshd_config
```

修改对应代码，变成以下两行的样子（原来没有就插入 ）：

```
X11Forwarding yes
X11UseLocalhost no
```

之后必须要重启 ssh 服务：

```bash
$ sudo /etc/init.d/ssh restart
```

这样就可以了，解决方案原始网页在[这里](http://ask.xmodulo.com/fix-broken-x11-forwarding-ssh.html)
