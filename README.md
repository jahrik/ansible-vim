## Vim

[![CICD](https://github.com/jahrik/ansible-vim/actions/workflows/cicd.yml/badge.svg)](https://github.com/jahrik/ansible-vim/actions/workflows/cicd.yml)

Install vim, vundle, plugins, and configure vimrc

## Requirements

- git

## Role Variables

    vundle:
      - "The-NERD-tree"
      - "vim-flake8"
      - "surround.vim"
      - "vadv/vim-chef"
      - "vim-syntastic/syntastic"
      - "plasticboy/vim-markdown"
      - "mzlogin/vim-markdown-toc"
      - "ekalinin/Dockerfile.vim"
      - "https://github.com/hashivim/vim-terraform.git"
      - "https://github.com/wtfbbqhax/Snort-vim.git"
      - "https://github.com/pearofducks/ansible-vim.git"
      - "https://github.com/powerline/powerline.git"
      - "https://github.com/ngmy/vim-rubocop.git"
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
    map:
      - <c-j> <c-w>j
      - <c-k> <c-w>k
      - <c-l> <c-w>l
      - <c-h> <c-w>h
      - <leader>td <Plug>TaskLisk
      - "<leader>g :GundoToggle<CR>"
      - "<F2> :NERDTreeToggle<CR>"
      - "<F3> :setlocal spell! spelllang=en_us<CR>"
    nmap:
      - "<F4> :SyntasticToggleMode<CR>"
      - "<f5> :set number! number?<cr>"
    syntax:
      - "on"
    colorscheme:
      - slate
    filetype:
      - "on"
      - plugin indent on
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
      - g:syntastic_chef_checkers = ['cookstyle']
      - g:ansible_ftdetect_filename_regex = '\v(playbook|site|main|local|requirements)\.ya?ml$'
      # - g:syntastic_ignore_files = ['\m^roles/']
    command:
      - Fuckyou w !sudo tee %
    autocmd:
      - FileType ruby,eruby set filetype=ruby.eruby.chef

## Dependencies

none

## Example Playbook

    - hosts: local
      roles:
         - { role: jahrik.vim, colorscheme: desert }

## License

GPLv2

## Author Information

jahrik@gmail.com

https://homelab.business/
