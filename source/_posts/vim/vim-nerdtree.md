---
title: Vim NerdTree
categories:
- Vim
tags:
- nerdtree
---

# NerdTree

### 配置文件

```
autocmd vimenter * NERDTree
map <F3> :NERDTreeMirror<CR>
map <F3> :NERDTreeToggle<CR>
```

- 打开vim自动开启NerdTree
- F3 开关NerdTree

### 基本操作

`o` 打开并转到页面

`go` 打开但不跳转

`t` 新tab中打开

`T` 新tab中打开但不跳转

`i` split新窗口打开

`gi` split新窗口但不跳转

`s` vsplit新窗口打开

`gs` vsplit新窗口但不跳转

`gt` 后一个tab

`gT` 前一个tab

### 分屏操作

`ctrl w w` 光标来回切换

`ctrl w h/j/k/l` 光标左/下/上/右切换