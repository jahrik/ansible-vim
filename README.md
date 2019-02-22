Vim
=========

vim, vundle, plugins, vimrc

Role Variables
--------------

Available variables are listed below, along with default values (see defaults/main.yml):

Vundle plugins

    vundle:
      - "The-NERD-tree"
      - "vim-flake8"
      - "surround.vim"
      - "vadv/vim-chef"
      - "vim-syntastic/syntastic"
      - "plasticboy/vim-markdown"
      - "ekalinin/Dockerfile.vim"
      - "https://github.com/hashivim/vim-terraform.git"
      - "https://github.com/wtfbbqhax/Snort-vim.git"
      - "https://github.com/pearofducks/ansible-vim.git"
      - "https://github.com/powerline/powerline.git"
      - "https://github.com/ngmy/vim-rubocop.git"

:set

    set:
      - laststatus=2
      - showtabline=2
      - noshowmode
      - foldmethod=indent
      - foldlevel=99
      - background=dark
      - completeopt=menuone,longest,preview
      - colorcolumn=101
      - cursorcolumn
      - cursorline
      - expandtab
      - tabstop=2
      - shiftwidth=2
      - spell
      - spelllang=en
      - spellfile=$HOME/.vim/en.utf-8.add
      - number
      # syntastic
      - statusline+=%#warningmsg#
      - statusline+=%{SyntasticStatuslineFlag()}
      - statusline+=%*


:map

    map:
      - <c-j> <c-w>j
      - <c-k> <c-w>k
      - <c-l> <c-w>l
      - <c-h> <c-w>h
      - <leader>td <Plug>TaskLisk
      - "<leader>g :GundoToggle<CR>"
      - "<F2> :NERDTreeToggle<CR>"
      - "<F3> :setlocal spell! spelllang=en_us<CR>"


:nmap

    nmap:
      - "<F4> :SyntasticToggleMode<CR>"
      - "<f5> :set number! number?<cr>"

:syntax

    syntax:
      - "on"

:colorscheme

    colorscheme:
      - slate

:filetype

    filetype:
      - "on"
      - plugin indent on


:let

    let:
      - g:pyflakes_use_quickfix = 0
      - g:NERDTreeDirArrows=0
      - g:syntastic_always_populate_loc_list = 1
      - g:syntastic_auto_loc_list = 1
      - g:syntastic_check_on_open = 1
      - g:syntastic_check_on_wq = 0
      - g:syntastic_ruby_checkers = ['rubocop']
      - g:syntastic_python_checkers = ['pylint', 'flake8']
      - g:syntastic_ansible_checkers = ['ansible_lint']
      - g:syntastic_chef_checkers = ['foodcritic', 'cookstyle']
      - g:syntastic_ignore_files = ['\m^roles/']

:command

    command:
      - Fuckyou w !sudo tee %

:autocmd

    autocmd:
      - FileType ruby,eruby set filetype=ruby.eruby.chef

Dependencies
------------

- geerlingguy.git

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: local
      roles:
         - { role: jahrik.vim, colorscheme: desert }

License
-------

GPLv2

Author Information
------------------

jahrik@gmail.com

https://homelab.business/
