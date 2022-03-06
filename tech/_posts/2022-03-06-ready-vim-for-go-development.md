---
layout: default
title: Ready Vim for Go development
---

## Language Server Protocol (gopls)

### Plugins
```
Plug 'prabirshrestha/vim-lsp'
Plug 'mattn/vim-lsp-settings'
Plug 'prabirshrestha/asyncomplete.vim'
Plug 'prabirshrestha/asyncomplete-lsp.vim'
```

### Configurations
Keybindings:
```
map <silent> <C-LeftMouse> :LspHover<CR>
map <silent> <Leader>d :LspDefinition<CR>
map <silent> <Leader>h :LspHover<CR>
```

Auto-completion:
```
let g:asyncomplete_auto_popup = 1
set completeopt = menuone, noinsert, noselect, preview
autocmd! CompleteDone * if pumvisible() == 0 | pclose | endif
```
