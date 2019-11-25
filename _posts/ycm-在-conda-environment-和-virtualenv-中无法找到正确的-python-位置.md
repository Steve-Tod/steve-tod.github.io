---
title: ycm 在 conda environment 和 virtualenv 中无法找到正确的 python 位置
date: 2018-11-02 19:57:40
tags:
- 工具
- ycm
- vim
---

在 anaconda 的虚拟环境中使用 vim 时，发现 ycm 插件只能跳转到 python 内置的函数和库，在 ycm 的 GitHub 仓库文档里找到解决办法：

在项目的根目录创建 .ycm\_extra\_conf.py 文件，写入以下内容即可：

```Python
def Settings( **kwargs ):
    return {
      'interpreter_path': '/path/to/virtual/environment/python'
    }
```

这里 'interpreter\_path' 的值就是虚拟环境的 python 路径。
