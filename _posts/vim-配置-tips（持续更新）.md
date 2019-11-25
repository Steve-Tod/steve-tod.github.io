---
title: vim 配置 tips（持续更新）
date: 2018-09-15 10:42:20
tags:
- 工具
- vim
---

# 目录

- [基本配置](#basic)
- [插件](#plugins)
  - [vim-vue](#vim-vue)

<!--more-->

## <a name="basic"/> 基本配置
vim 的配置都在 ~/.vimrc 中。我的基本配置如下

```vim
set nocompatible                        " 去除VI一致性,必须
set fileformat=unix                     " 会显示 dos 下的 \r\n 为 ^M
set shiftwidth=2 | set expandtab        " 把自动缩进变成空格，并且是 2 个
syntax on                               " 开启语法高亮
set nu                                  " 显示行号
```

## <a name="plugins"/> 插件

我用 vundle 来管理插件。vundle 是 vim 的插件管理工具，它能够搜索、安装、更新和移除 vim 插件，再也不需要手动管理 vim 插件。vundle 配置如下

```vim
" 配置 vundle 开始
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
Plugin 'VundleVim/Vundle.vim'
Plugin 'dpelle/vim-LanguageTool'
Plugin 'scrooloose/nerdtree'
Plugin 'posva/vim-vue'
call vundle#end()
filetype plugin indent on    " 自动检测文件类型并加载插件
" 配置 vundle 结束
```

注意，修改 .vimrc 文件后，记得运行如下代码安装插件

```bash
git clone https://github.com/gmarik/vundle.git ~/.vim/bundle/vundle
vim +PluginInstall +qall
```

### <a name="vim-vue"> vim-vue

安装 vim-vue 插件时，遇到一个问题就是装了之后 .vue 文件还是没高亮，查看 issue 后发现大概是文件类型识别的问题，应在 .vimrc 中加入如下命令解决。

```vim
au BufNewFile,BufRead *.vue setf vue
```
