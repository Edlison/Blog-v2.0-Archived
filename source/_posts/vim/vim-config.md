---
title: Vim配置
categories: 
- Vim
tags: 
- vim
- config
- linux
---

# Vim配置文件

Vim有两个配置文件

- /etc/vim/vimrc
- ~/.vimrc

第一个全局有效，第二个针对当前用户有效。

# Vim插件

## Vundle插件管理插件

下载到~/.vim/bundle/Vundle.vim

```sh
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

### 配置文件

```
set nocompatible              " be iMproved, required
filetype off                  " required
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
Plugin 'VundleVim/Vundle.vim', {'rtp': 'vim/'} " 指定用户/仓库. 还可以指定run_time_path到仓库的子目录
Plugin 'http://github.com/xxx/xxx.git', {'name': 'nerdtree'} " 指定仓库url, 还可以指定名字
Plugin 'file:///home/xxx/xxx' " 指定本地插件
" ...
call vundle#end()            " required
filetype plugin indent on    " required
```

### 安装插件

`:PluginInstall`

### 删除插件

`:PluginClean`

或在配置文件下注释掉







----

https://www.cnblogs.com/zhaodehua/articles/14174336.html

https://blog.csdn.net/weixin_38815998/article/details/103589090

