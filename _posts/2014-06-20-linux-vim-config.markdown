---
layout: post
title: "rails vim 开发环境配置"
categories: linux
date: 2014-06-20
---

## rails vim 使用的插件
vim插件管理：[pathogen](https://github.com/tpope/vim-pathogen)
在其管理下使用到的一些插件：
1. 目录跳转 [The Nerd Tree](https://github.com/vim-scripts/The-NERD-tree)
2. html编辑器 [emmet-vim](https://github.com/mattn/emmet-vim)
3. rails.vim [vim-rails](https://github.com/tpope/vim-rails)
4. 配色方案 [vim-color](https://github.com/altercation/vim-colors-solarized)

## .vim 文件夹目录结构
.   
├── autoload   
│   └── pathogen.vim   
└── bundle    
    ├── emmet-vim   
    ├── The-NERD-tree   
    ├── tlib_vim   
    ├── vim-addon-mw-utils   
    ├── vim-bundler   
    ├── vim-colors-solarized   
    ├── vim-rails   
    ├── vim-snipmate   
    └── vim-snippets   
    
## .vimrc 配置
配置家目录下的.vimrc文件（没有需新建）

```bash
set fenc=gbk
set guifont=Monaco:h10       " 适合Ruby开发的字体 && 字号
set gfw=幼圆:h10.5:cGb2312     " 幼圆 中文字体
set tabstop=2                " 设置tab键的宽度
set shiftwidth=2             " 换行时行间交错使用4个空格
set autoindent               " 自动对齐
set backspace=2              " 设置退格键可用
set cindent shiftwidth=2     " 自动缩进4空格
set smartindent              " 智能自动缩进
set ai!                      " 设置自动缩进
set nu!                      " 显示行号
set mouse=a                  " 启用鼠标
set ruler                    " 右下角显示光标位置的状态行
set incsearch                " 查找book时，当输入/b时会自动找到
set hlsearch                 " 开启高亮显示结果
set incsearch                " 开启实时搜索功能
set nowrapscan               " 搜索到文件两端时不重新搜索
set nocompatible             " 关闭兼容模式
set vb t_vb=                 " 关闭提示音
set hidden                   " 允许在有未保存的修改时切换缓冲区
set list                     " 显示Tab符，使用一高亮竖线代替
set listchars=tab:\|\ ,
set tabstop=2
set shiftwidth=2
set expandtab
execute pathogen#infect()
syntax enable                " 打开语法高亮
syntax on                    " 开启文件类型侦测
filetype indent on           " 针对不同的文件类型采用不同的缩进格式
filetype plugin on           " 针对不同的文件类型加载对应的插件
filetype plugin indent on    " 启用自动补全
let NERDTreeShowBookmarks = 1
let NERDChristmasTree = 1
let NERDTreeWinPos = "left"
map <F8> :NERDTree<CR>
set background=dark
colorscheme solarized
let g:snipMate = {}
let g:snipMate.scope_aliases = {}
let g:snipMate.scope_aliases['ruby'] = 'ruby,ruby-rails,ruby-1.9'
let g:user_emmet_mode='a'
```
