---
comments: true
layout: post
title: "如何把Vim配置成PHP开发的IDE"
category: 求生技能
tags: [Vim, PHP, IDE]
date: 2011-06-07 23:31:35 +08:00
---
原文：http://hameedullah.com/howto-setup-vim-ide-for-php-development.html

作为一个PHP开发者在IDE上有非常多的选择。一些图形界面的会有很多像调试、代码补全、自动缩进、高亮、代码检查和很多其他的内建功能。但是这些IDE用起来都太笨重，太慢了。我知道有很多像我这样离了Vim就不能活并且把Vim用在各种类型的文字输入上的人，无论是写代码，编辑配置文件或者创建一些其他的简单文本文件。

最近我发了一张我Vim的截图，然后就有很多人发邮件问我是怎么把Vim配置成那样的。所以我希望这篇文章可以很简单的让大家把Vim配置成PHP的IDE。简单起见，我假设你是在Linux或者类Linux系统上使用Vim。如果你是Windows用户的话，这篇指导同样适用你，但是我不会提供Windows下相关的详细信息。

为了让Vim成为一个完整的IDE，下面是我们要添加、启用的一些功能：
<ul>
	<li>代码补全</li>
	<li>标签列表</li>
	<li>项目管理</li>
	<li>语法高亮</li>
	<li>代码检查</li>
</ul>
首先我们要下载下面这些Vim脚本，并解压倒~/.vim目录
<ul>
	<li><a title="Project" href="http://www.vim.org/scripts/script.php?script_id=69" target="_blank">Project</a></li>
	<li><a title="Taglist" href="http://vim.sourceforge.net/scripts/script.php?script_id=273" target="_blank">Taglist</a></li>
	<li><a title="BuffExplorer" href="http://www.vim.org/scripts/script.php?script_id=42" target="_blank">BuffExplorer</a></li>
</ul>
按顺序下载上面的脚本并安装。安装它们只需要把它们解压你的Home目录下的.vim目录。现在我们需要下载一些文件到我们的~/.vim/plugin目录。
<ul>
	<li><a title="PHP-Doc" href="http://www.vim.org/scripts/script.php?script_id=1355" target="_blank">PHP-Doc</a></li>
	<li><a title="ProjTags" href="http://www.vim.org/scripts/script.php?script_id=1873" target="_blank">ProjTags</a></li>
	<li><a title="PerDirVimrc" href="http://www.vim.org/scripts/script.php?script_id=2792" target="_blank">PerDirVimrc</a></li>
</ul>
现在我们安装了上面那些脚本，差不多快完成了。你只需要把下面的内容复制倒你Home目录下的.vimrc里。
{% highlight vim %}
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
" ~/.vimrc                                                      "
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"                                                               "
" Version: 0.1                                                  "
"                                                               "
"                                                               "
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""

" Hightlight the ifs and buts
syntax on

" Plugins and indentation based on the file type
filetype plugin indent on

" Don't remember source of this, i think it was already in my .vimrc
" Tell vim to remember certain things when we exit
"  '10 : marks will be remembered for up to 10 previously edited files
"  "100 : will save up to 100 lines for each register
"  :5000 : up to 5000 lines of command-line history will be remembered
"  % : saves and restores the buffer list
"  n... : where to save the viminfo files
set viminfo='10,\"100,:5000,%,n~/.viminfo

" omnicomplete from: http://vim.wikia.com/wiki/VimTip1386
set completeopt=longest,menuone
inoremap <expr> <CR> pumvisible() ? "\<C-y>" : "\<C-g>u\<CR>"
inoremap <expr> <C-n> pumvisible() ? '<C-n>' :
  \ '<C-n><C-r>=pumvisible() ? "\<lt>Down>" : ""<CR>'

"###########################
"##       PHP             ##
"###########################
" The php doc plugin
" source ~/.vim/php-doc.vim
inoremap <C-P> <ESC>:call PhpDocSingle()<CR>i
nnoremap <C-P> :call PhpDocSingle()<CR>
vnoremap <C-P> :call PhpDocRange()<CR>

" run file with PHP CLI (CTRL-M)
:autocmd FileType php noremap <C-M> :w!<CR>:!/usr/bin/php %<CR>

" PHP parser check (CTRL-L)
:autocmd FileType php noremap <C-L> :!/usr/bin/php -l %<CR>

" Do use the currently active spell checking for completion though!
" (I love this feature :-)
set complete+=kspell

" disable tabs
set expandtab
set shiftwidth=4
set softtabstop=4

" highlt matches
set hlsearch

" Taken from http://peterodding.com/code/vim/profile/vimrc
" Make Vim open and close folded text as needed because I can't be bothered to
" do so myself and wouldn't use text folding at all if it wasn't automatic.
set foldmethod=marker foldopen=all,insert foldclose=all

" Enable enhanced command line completion.
set wildmenu wildmode=list:full

" Ignore these filenames during enhanced command line completion.
set wildignore+=*.aux,*.out,*.toc " LaTeX intermediate files
set wildignore+=*.jpg,*.bmp,*.gif " binary images
set wildignore+=*.luac " Lua byte code
set wildignore+=*.o,*.obj,*.exe,*.dll,*.manifest " compiled object files
set wildignore+=*.pyc " Python byte code
set wildignore+=*.spl " compiled spelling word lists
set wildignore+=*.sw? " Vim swap files

" Enable completion dictionaries for PHP buffers.
autocmd FileType php set complete+=k~/.vim/dict/PHP.dict

" PHP Autocomplete
autocmd FileType php set omnifunc=phpcomplete#CompletePHP
set ofu=syntaxcomplete#Complete

" You might also find this useful
" PHP Generated Code Highlights (HTML & SQL)
let php_sql_query=1
let php_htmlInStrings=1
let g:php_folding=2
set foldmethod=syntax

" --------------------
" Project
" --------------------
map <A-S-p> :Project<CR>
map <A-S-o> :Project<CR>:redraw<CR>/
nmap <silent> <F3> <Plug>ToggleProject
"let g:proj_window_width = 30
"let g:proj_window_increment = 150

nnoremap <silent> <F8> :TlistToggle<CR>
let Tlist_Exit_OnlyWindow = 1     " exit if taglist is last window open
let Tlist_Show_One_File = 1       " Only show tags for current buffer
let Tlist_Enable_Fold_Column = 0  " no fold column (only showing one file)
let tlist_sql_settings = 'sql;P:package;t:table'
let tlist_ant_settings = 'ant;p:Project;r:Property;t:Target'

" auto change directory from: http://vim.wikia.com/wiki/Set_working_directory_to_the_current_file
autocmd BufEnter * if expand("%:p:h") !~ '^/tmp' | lcd %:p:h | endif

" when we reload, tell vim to restore the cursor to the saved position
augroup JumpCursorOnEdit
 au!
 autocmd BufReadPost *
 \ if expand("<afile>:p:h") !=? $TEMP |
 \ if line("'\"") > 1 && line("'\"") <= line("$") |
 \ let JumpCursorOnEdit_foo = line("'\"") |
 \ let b:doopenfold = 1 |
 \ if (foldlevel(JumpCursorOnEdit_foo) > foldlevel(JumpCursorOnEdit_foo - 1)) |
 \ let JumpCursorOnEdit_foo = JumpCursorOnEdit_foo - 1 |
 \ let b:doopenfold = 2 |
 \ endif |
 \ exe JumpCursorOnEdit_foo |
 \ endif |
 \ endif
 " Need to postpone using "zv" until after reading the modelines.
 autocmd BufWinEnter *
 \ if exists("b:doopenfold") |
 \ exe "normal zv" |
 \ if(b:doopenfold > 1) |
 \ exe "+".1 |
 \ endif |
 \ unlet b:doopenfold |
 \ endif
augroup END

" PHP code sniffer
" If code sniffer is installed you can run it on current php file by running
" :Phpcs
function! RunPhpcs()
    let l:filename=@%
    let l:phpcs_output=system('phpcs --report=csv --standard=YMC '.l:filename)
"    echo l:phpcs_output
    let l:phpcs_list=split(l:phpcs_output, "\n")
    unlet l:phpcs_list[0]
    cexpr l:phpcs_list
    cwindow
endfunction

set errorformat+=\"%f\"\\,%l\\,%c\\,%t%*[a-zA-Z]\\,\"%m\"
command! Phpcs execute RunPhpcs()
{% endhighlight %}

好了，设置完毕。下面这些快捷命令来让你开始适用刚才启用的所有功能。

**F3**：打开项目管理

**\C**：在打开项目管理后，用这个新建项目

**F8**：打开标签列表

**Ctrl+L**：在你的PHP文件上运行语法检查

**Ctrl+P**：在类或方法上添加PHP doc风格的注释

**":PhpCs"**：在你的PHP脚本上打开PHP代码嗅探（需要安装code sniffer）

**Ctrl+n**：自动补全

