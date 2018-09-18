---
title: vim使用（2）编辑
categories:
    - vim
tags:
    - vim
    - Linux
date: 2018-05-23 23:44:13
---

## 窗口
1. 创建窗口
```
水平分割窗口
:split             当前窗口一分为二，两个窗口显示相同内容。  
:10split           新窗口的高度10行
:split otherfile   新窗口中打开otherfile   
:new               功能和split一样  
:sp                split的缩写形式  
ctrl+w+s           分割窗口的快捷方式

垂直分割窗口
:vsplit 以上所有命令都适用于打开垂直分割窗口，只要在前面加v(vetical)
```

2. 关闭窗口
```
q  或 close   #关闭当前窗口
 only      #保留当前窗口，关闭其它所有窗口
 qall(qa)      #退出所有窗口
 wall      #保存所有窗口
```


3. 窗口切换
:ctrl+w+j/k，通过j/k可以上下切换，或者:ctrl+w加上下左右键，还可以通过快速双击ctrl+w依次切换窗口。

4. 窗口大小调整
```
纵向调整
:ctrl+w + 纵向扩大（行数增加）
:ctrl+w - 纵向缩小 （行数减少）
:res(ize) num  例如：:res 5，显示行数调整为5行
:res(ize)+num 把当前窗口高度增加num行
:res(ize)-num 把当前窗口高度减少num行
横向调整
:vertical res(ize) num 指定当前窗口为num列
:vertical res(ize)+num 把当前窗口增加num列
:vertical res(ize)-num 把当前窗口减少num列
```

5. 窗口重命名
:f file

6. 文件浏览
```
:Ex 开启目录浏览器，可以浏览当前目录下的所有文件，并可以选择
:Sex 水平分割当前窗口，并在一个窗口中开启目录浏览器
:ls 显示当前buffer情况
```

7. vi与shell切换
```
:shell 可以在不关闭vi的情况下切换到shell命令行
:exit 从shell回到vi    
```

## 页签

```
:tabnew [++opt选项] ［＋cmd］ 文件            建立对指定文件新的tabta
:tab <文件>  新页签打开指定文件
:tabc       关闭当前的tab
:tabo       关闭所有其他的tab
:tabs       查看所有打开的tab
:tabp      前一个
:tabn      后一个
标准模式下：
gt , gT 可以直接在tab之间切换。
ngt 切换到指定的窗口。
更多可以查看帮助 :help table ， help -p
```

## 编译运行
vim 环境下，在命令模式中输入下命令：
```
:!python %
```

## NERDTree命令
安装
git clone https://github.com/scrooloose/nerdtree.git ~/.vim/bundle

:h NERDTree 查看帮助文档

```
?                         查看所有命令

窗口切换
ctrl + w + h              光标移动到左侧树形目录
ctrl + w + l              光标移动到右侧文件显示窗口
ctrl + w + w              光标自动在左右侧窗口切换
ctrl + w + r              移动当前窗口的布局位置

打开文件
o                         在已有窗口中打开文件、目录或书签，并跳到该窗口
go                        在已有窗口 中打开文件、目录或书签，但不跳到该窗口
t                         在新 Tab 中打开选中文件/书签，并跳到新 Tab
T                         在新 Tab 中打开选中文件/书签，但不跳到新 Tab
i                         split 一个新窗口打开选中文件，并跳到该窗口
gi                        split 一个新窗口打开选中文件，但不跳到该窗口
s                         vsplit 一个新窗口打开选中文件，并跳到该窗口
gs                        vsplit 一个新 窗口打开选中文件，但不跳到该窗口
!                         执行当前文件
O                         递归打开选中 结点下的所有目录
x                         合拢选中结点的父目录

D                         删除当前书签

P                         跳到根结点
p                         跳到父结点
K                         跳到当前目录下同级的第一个结点
J                         跳到当前目录下同级的最后一个结点
k                         跳到当前目录下同级的前一个结点
j                         跳到当前目录下同级的后一个结点

C                         将选中目录或选中文件的父目录设为根结点
u                         将当前根结点的父目录设为根目录，并变成合拢原根结点
U                         将当前根结点的父目录设为根目录，但保持展开原根结点
r                         递归刷新选中目录
R                         递归刷新根结点
m                         显示文件系统菜单
cd                        将 CWD 设为选中目录

I                         切换是否显示隐藏文件
f                         切换是否使用文件过滤器
F                         切换是否显示文件
B                         切换是否显示书签

A                         全屏显示开关
q                         关闭 NerdTree 窗口
:NERDTreeToggle           打开 NerdTree 窗口
```

## vimrc
```
" *********************************************
" Vbundle插件管理
" *********************************************
set nocompatible              " required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'gmarik/Vundle.vim'
Plugin 'scrooloose/nerdtree'
Plugin 'majutsushi/tagbar'
Plugin 'vim-scripts/indentpython.vim'


" Add all your plugins here (note older versions of Vundle used Bundle instead of Plugin)
" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required

" *********************************************
" 分割布局相关
" *********************************************
set splitbelow
set splitright
"快捷键，ctrl+l切换到左边布局，ctrl+h切换到右边布局
"ctrl+k切换到上面布局，ctrl+j切换到下面布局
nnoremap <C-J> <C-W><C-J>
nnoremap <C-K> <C-W><C-K>
nnoremap <C-L> <C-W><C-L>
nnoremap <C-H> <C-W><C-H>

" 开启代码折叠功能
" 根据当前代码行的缩进来进行代码折叠
set foldmethod=indent
set foldlevel=99

" *********************************************
" NERD插件属性
" *********************************************
au vimenter * NERDTree      " 开启vim的时候默认开启NERDTree
map <F2> :NERDTreeToggle<CR>    " 设置F2为开启NERDTree的快捷键


" tagbar  启动时自动focus
let g:tagbar_auto_faocus =1
" 启动指定文件时自动开启tagbar
autocmd BufReadPost *.cpp,*.c,*.h,*.hpp,*.cc,*.cxx call tagbar#autoopen()

" *********************************************
" python代码风格PEP8
" *********************************************
au BufNewFile,BufRead *.py set tabstop=4 |set softtabstop=4|set shiftwidth=4|set textwidth=79|set expandtab|set autoindent|set fileformat=unix
au BufNewFile,BufRead *.js, *.html, *.css set tabstop=2|set softtabstop=2|set shiftwidth=2
```
