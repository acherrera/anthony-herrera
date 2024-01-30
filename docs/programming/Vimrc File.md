---
Title: Vimrc File
Date: 2023-12-29 11:07
tags:
  - linux
  - ubuntu
  - terminal
  - vim
  - neovim
Slug: vimrc_file
Author: Anthony Herrera
Summary: My vimrc set up
---

# Vimrc File

This is the file that controls my VIM setup. I find these kind of interesting since they are kind of like a glimpse into the mind of the person who uses them.

Technically this is a neovim configuration, but it's the exact same.

```vimrc
set nocompatible
filetype off
set shell=bash

" avoid configuration surprises
let g:python3_host_prog = '/home/tony/miniconda3/bin/python3'

imap jk <esc>
set clipboard+=unnamedplus
let mapleader=","
 
" Remove mouse entirely - used to fix an issue when sshing and pasting code.
set mouse=

""""""""""" Plugins """""""""""""
call plug#begin('~/.vim/plugged')

" Git Stuff ====================
Plug 'tpope/vim-fugitive'

" Auto Completion ===============
Plug 'neoclide/coc.nvim', {'branch': 'release'}

" Easy Motion ==================
Plug 'easymotion/vim-easymotion'

" File browser =================
Plug 'preservim/nerdtree'

Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

" Completion tools =============================
" Tabnine
Plug 'codota/tabnine-nvim', { 'do': './dl_binaries.sh' }

" Configuration ==============
" no idea what this does
Plug 'neovim/nvim-lspconfig'

" Navigation =================
Plug 'christoomey/vim-tmux-navigator' " ctrl+h and ctr+j movement

" Time Tracking ==============
Plug 'wakatime/vim-wakatime'

" Colorschemes ==============
Plug 'EdenEast/nightfox.nvim' " Vim-Plug

" Better highlighting ===========
Plug 'nvim-treesitter/nvim-treesitter', {'do': ':TSUpdate'}

" Fuzzy finder
Plug 'nvim-lua/plenary.nvim'
Plug 'nvim-telescope/telescope.nvim', { 'tag': '0.1.0' }

" all done
hi Normal guibg=NONE ctermbg=NONE
call plug#end()

"""""""""""""""""""""""""

" Set colorscheme to nightfox
colorscheme nightfox


" Interesting colors
" terafox
" nightfox
" carbonfox
set relativenumber

" quick vertical splits
nnoremap <silent> vv <C-w>v

" Python debugger - `+d` with add "pudb; pudb.set_trace()"
nnoremap +d Oimport pudb; pudb.set_trace()<esc>0j

set foldmethod=indent

" This is for tab spacing =====================================
set tabstop=4
set softtabstop=4
set shiftwidth=4
set textwidth=120
set expandtab
set autoindent
" set fileformat=unix

" Colors!
syntax enable
set background=dark
set termguicolors

set number
let python_highlight_all=1
syntax on
set incsearch
set nohlsearch

" Spelling check on markdown files
autocmd FileType markdown setlocal spell

" Find files using Telescope command-line sugar. =========
" THIS IS AMAZING and I forgot about it
nnoremap <leader>ff <cmd>Telescope find_files<cr>
nnoremap <leader>fg <cmd>Telescope live_grep<cr>
nnoremap <leader>fb <cmd>Telescope buffers<cr>
nnoremap <leader>fh <cmd>Telescope help_tags<cr>


" Easy motion config ================
" " <Leader>f{char} to move to {char}
map  <Leader>f <Plug>(easymotion-bd-f)
nmap <Leader>f <Plug>(easymotion-overwin-f)

" s{char}{char} to move to {char}{char}
nmap s <Plug>(easymotion-overwin-f2)

" Move to line
map <Leader>L <Plug>(easymotion-bd-jk)
nmap <Leader>L <Plug>(easymotion-overwin-line)

" Move to word
map  <Leader>w <Plug>(easymotion-bd-w)
nmap <Leader>w <Plug>(easymotion-overwin-w)


" CoC tab completion ======================
function! CheckBackspace() abort
  let col = col('.') - 1
  return !col || getline('.')[col - 1]  =~# '\s'
endfunction

" For Coc
inoremap <silent><expr> <TAB>
      \ coc#pum#visible() ? coc#pum#next(1) :
      \ CheckBackspace() ? "\<Tab>" :
      \ coc#refresh()
inoremap <expr><S-TAB> coc#pum#visible() ? coc#pum#prev(1) : "\<C-h>"

inoremap <silent><expr> <CR> coc#pum#visible() ? coc#pum#confirm()
                              \: "\<C-g>u\<CR>\<c-r>=coc#on_enter()\<CR>"
							
" For tabnine
" ctrl+a: accept
" ctrl+r: reject

lua <<EOF
require('tabnine').setup({
  disable_auto_comment=true,
  accept_keymap="<C-a>",
  dismiss_keymap = "<C-r>",
  debounce_ms = 800,
  suggestion_color = {gui = "#808080", cterm = 244},
  exclude_filetypes = {"TelescopePrompt", "NvimTree"},
  log_file_path = nil, -- absolute path to Tabnine log file
})
EOF

set noswapfile
```

That's it! Hope you  enjoy!