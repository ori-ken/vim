# vim

1.
$ brew install neovim
もう一度実行すると、
「Warning: neovim 0.3.8 is already installed and up-to-date」
と出るのでOK。

2.
$ mkdir ~/.config/nvim
$ vi ~/.config/nvim/init.vim
適当にinit.vimにvimrcの内容を記述する。

3.
dein.vim
$ brew install python3
$ /usr/local/bin/pip3 install -U neovim

" reset augroup
augroup MyAutoCmd
    autocmd!
augroup END

" ENV
let $CACHE = empty($XDG_CACHE_HOME) ? expand('$HOME/.cache') : $XDG_CACHE_HOME
let $CONFIG = empty($XDG_CONFIG_HOME) ? expand('$HOME/.config') : $XDG_CONFIG_HOME
let $DATA = empty($XDG_DATA_HOME) ? expand('$HOME/.local/share') : $XDG_DATA_HOME

" Load rc file
function! s:load(file) abort
    let s:path = expand('$CONFIG/nvim/rc/' . a:file . '.vim')

    if filereadable(s:path)
        execute 'source' fnameescape(s:path)
    endif
endfunction

call s:load('plugins')
をinit.vimに記載

let s:dein_dir = expand('$DATA/dein')

if &runtimepath !~# '/dein.vim'
    let s:dein_repo_dir = s:dein_dir . '/repos/github.com/Shougo/dein.vim'

    " Auto Download
    if !isdirectory(s:dein_repo_dir)
        execute '!git clone https://github.com/Shougo/dein.vim ' . s:dein_repo_dir
    endif

    execute 'set runtimepath^=' . s:dein_repo_dir
endif

let g:dein#install_max_processes = 16
let g:dein#install_message_type = 'none'

if !dein#load_state(s:dein_dir)
    finish
endif

call dein#begin(s:dein_dir, expand('<sfile>'))

let s:toml_dir = expand('$CONFIG/nvim/dein')

call dein#load_toml(s:toml_dir . '/plugins.toml', {'lazy': 0})
call dein#load_toml(s:toml_dir . '/lazy.toml', {'lazy': 1})
if has('python3')
    call dein#load_toml(s:toml_dir . '/python.toml', {'lazy': 1})
endif

call dein#end()
call dein#save_state()

if has('vim_starting') && dein#check_install()
    call dein#install()
endif
" }}}
を.config/nvim/rc/plugins.vimに記載

[[plugins]]
repo = 'Shougo/dein.vim'

repo = 'itchyny/lightline.vim'
hook_add = '''
    let g:lightline = {'colorscheme': 'wombat'}
'''

# Toml
[[plugins]]
repo  = 'cespare/vim-toml'
を~/.config/nvim/dein/plugins.tomlに記述
