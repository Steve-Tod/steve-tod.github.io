---
title: 魅族 pro6 安装迅雷无效解决办法
date: 2018-09-20 23:36:11
tags:
- debug
- Android
---

之前自己的 pro6 安装迅雷总是安装后没有任何反应，也找不到应用。

试图安装旧版本，提示要删除原有的迅雷，但是并不能找到。因此尝试了一波 adb 删除。

首先用 `adb shell pm list packages | grep xunlei` 查看是否已经安装迅雷，然而并没有找到。

于是从官网下载 apk 后，直接 `adb install ~/Downloads/OfficialSite_MobileThunder2.apk` 强行安装，提示

```
adb: failed to install /Users/lxd/Downloads/OfficialSite_MobileThunder2.apk: Failure [INSTALL_FAILED_ALREADY_EXISTS: Attempt to re-install com.xunlei.downloadprovider without first uninstalling.]
```

于是直接尝试 `adb uninstall com.xunlei.downloadprovider` ，成功后再直接 adb 安装，就可以啦！
