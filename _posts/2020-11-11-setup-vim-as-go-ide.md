---
layout: post
title: Setup VIM as a Go IDE
categories: [setup]
---

## Autocompletion

`vim-lsp` plugin is a one-for-all autocompletion plugin. For Go, it uses `gopls` as the backend. The main reason I choose this over other similar plugins such as `vim-go` or `CoC` is due to its performance. The latter two plugins, I always ran into issue where autocompletion candidates need few seconds to pop-up. To setup `vim-lsp`, you need following plugins in your `vimrc`

```
Plug 'prabirshrestha/vim-lsp'
Plug 'mattn/vim-lsp-settings'
Plug 'prabirshrestha/asyncomplete.vim'
Plug 'prabirshrestha/asyncomplete-lsp.vim'
```

Once plugins is setup, you can open any Go file and type this to setup `vim-lsp` for Go.

```
:LspInstallServer
```

To keep autocompletion pop-up always showing up, you can setup:

```
let g:asyncomplete_auto_popup = 1
```

You can also install `supertab` plugin to invoke autocompletion by TAB.

## Syntax Checking

`syntastic` plugin gives the ability to run Syntax check on the fly. The recommended `vimrc` configuration is shown as below.

```
" Syntastic
let g:syntastic_always_populate_loc_list = 1
let g:syntastic_auto_loc_list = 1
let g:syntastic_check_on_open = 1
let g:syntastic_check_on_wq = 0

" use goimports for formatting
let g:go_fmt_command = "goimports"

" turn highlighting on
let g:go_highlight_functions = 1
let g:go_highlight_methods = 1
let g:go_highlight_structs = 1
let g:go_highlight_operators = 1
let g:go_highlight_build_constraints = 1

let g:syntastic_go_checkers = ['go', 'golint', 'errcheck']
```