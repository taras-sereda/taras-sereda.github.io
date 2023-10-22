---
layout: post
title: Setting Up a minimal vimrc for Python Development 
tags: vim, software-engineering 
---

I have tried many code editors and vim remains my top choice for both writing code and taking notes.
In this blog post I'll share how I setup vim as Python Development Environment. By adding a handful of plugins and bringing some keyboard shortcuts of your choice you can get a pretty decent development experience even on headless machines. Notably vim consumes fewer resources than PyCharm and it will not spend ridiculous amount of time scanning and indexing your projects. Vim just works!

1. First important aspect is plugin manager, that serves as a package managers for you editor. I like to use [vim-plug](https://github.com/junegunn/vim-plug) It allows you to explicitly define all plugins you need in a single vim-centric config file `.vimrc`. With vim-plug you need to add corresponding git url inside plug's begin/end block or even use a shorthand of a form `<git-user>/<repo-name>`. Example: *~~https://github.com/~~avidhalter/jedi-vim*

    Please consult official manual on how to install vim-plug. Bellow is minimal `.vimrc` for python developement that uses single amazing plugin [jedi-vim](https://github.com/davidhalter/jedi-vim). With jedi-vim you'll have features like go-to-definition, variables renaming, and code completion.
 
    ```
    " Enable syntax highlighting
    syntax enable
    " Display line numbers to the left
    set number

    set backspace=indent,eol,start

    call plug#begin("~/.vim/plugged")

    Plug 'davidhalter/jedi-vim'
    call plug#end()
    ```

    > Tip: Don't forget to activate virtual environment in a tab where you are launching vim, jedi-vim needs to have access to installed packages, if it can't find a package it will not be able to do code completions.

2. Vim with jedi is good enough to start with, though my productivity improved significantly by bringing up several more plugins. Another important feature of modern IDEA is linting and fixing of sources. This can be achieved by using great [ALE](https://github.com/dense-analysis/ale) plugin, that supports Language Server Protocol(LSP). I like to use [pyright](https://github.com/Microsoft/pyright) for linting and [black](https://github.com/psf/black), [isort](https://github.com/PyCQA/isort) as my fixers. 

    - Employing fixers of your choice you wouldn't need to care about signle or double quotes for strings, code formating and improts reoredings. 

    - By using linters you'll get `Warnings` and `Errors` in comments to corresponding lines, so you'll be able to fix your code before trying to execute it.

    > Tip: Don't forget to `pip install isort black pyright`

    If you'll consult `ALEFixSuggest` you'll see that it offers general purpose fixers that can be used for any files. Namely `trim_whitespace` and `remove_trailing_lines`. By adding these fixers call to `:ALEFix` will do a mundane job for you.

3. Another important aspect of productive coding is to search files quickly. For this I use [fzf.vim](https://github.com/junegunn/fzf.vim) that allows to browse your project root and open files in current buffer or next tab. For convenience I keep several key board shortcuts in my `.vimrc` to quickly search necessary files.

4. I like to observe git changes to the current file I'm working on. And [vim-gitgutter](https://github.com/airblade/vim-gitgutter) is a plugin I use to bring git related semantics to the editor, namely ability to see changes, deletions, additions, as well as features like  staging/unstaging of code hunks and navigation between them.

Bellow is the more feature complete version of `.vimrc`. Also you can grab my ever-changing `.vimrc` configured for both rust and python development from [here](https://github.com/taras-sereda/dotfiles/blob/main/.vimrc)
 
    ```
    " Enable syntax highlighting
    syntax enable
    " Display line numbers to the left
    set number
    filetype plugin indent on
    set backspace=indent,eol,start

    call plug#begin("~/.vim/plugged")

    Plug 'dense-analysis/ale'
    let g:ale_linters = {'python': ['pyright']}
    let g:ale_fixers = {'*': ['trim_whitespace', 'remove_trailing_lines'], 'python': ['black', 'isort']}

    Plug 'davidhalter/jedi-vim'
    Plug 'ervandew/supertab'
    Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
    Plug 'junegunn/fzf.vim'
    Plug 'airblade/vim-gitgutter'

    call plug#end()

    nnoremap <silent> <C-f> :Files<CR>
```

I hope this guide will help you in setting up your own development environment, so that you'll have full control on features you have in the editor.

With love from Kyiv, Ukraine.
