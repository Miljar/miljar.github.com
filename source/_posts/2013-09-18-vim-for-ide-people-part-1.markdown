---
layout: post
title: "VIM for IDE-people - part 1"
date: 2013-09-18 17:59
comments: true
categories: [vim, macvim]
---

# Background

My first encounter with Vim was back in the year 2000, my first year of university. It didn't really appeal to me. Why on Earth bother with all these keyboard shortcuts when we have a mouse to point with?

Fast forward to 2008, my first PHP conference. The cool kids were all using one or another Linux flavor, and Vim for editing their source code.

We're 2013 now, and while I've been happy editing my source code in [Netbeans](http://netbeans.org "Netbeans") for the past 8 years, I'm not so happy with it anymore. Time to start learning and using Vim! (MacVim in my case)

# First things first: .vimrc

Where to start? I'm an IDE person and some things come with an IDE that don't come with an off-the-shelf Vim installation. Code completion, highlighting, folding, ... Luckily these things can be added to your Vim installation by means of plugins.

Before we start adding plugins, there are some essentials first:

* I'm not here to tell you how to install Vim. Just google it. I'm using [MacVim](https://code.google.com/p/macvim/ "MacVim").
* Most, if not all, configuration of your Vim editor happens through a file called `.vimrc`. If it's not there yet, just create a file with that name in your home folder.
* Plugins are installed in a special folder in your home folder: `~/.vim/bundle`, but more on that later.

Here's the `.vimrc` config that I started with. It's quite basic for now, but I want to learn the basics first, and then gradually extend my knowledge.

``` vim
syntax on
filetype plugin indent on

set number          " show line numbers
set lines=60        " make the editor 60 lines tall
set columns=150     " make the editor 150 columns wide

set tabstop=4       " hitting tab produces 4 columns space
set shiftwidth=4    " indent operations produce 4 columns space
set noexpandtab     " don't replace a <tab> with <space>'es
set softtabstop=4   " amount of columns to use when hitting <tab> in insert mode

set history=1000    " keep track of history for 1000 actions

set ruler          " show cursorposition
set cursorline     " highlight current line
set showcmd        " display incomplete commands
set incsearch      " incremental searching
set hlsearch       " highlight searchresult

set scrolloff=4    " keep at least 4 lines above or below the cursor

" fileformat stuff
set fileformats=unix,dos
set encoding=utf-8
set fileencodings=utf-8,ucs-bom,cp1250,iso-8859-1
```

These settings won't do anything fancy. Just play with them and see what suits you best.

# Plugins

Plugins extend the functionality of Vim far beyond its original capabilities. With the help of Vimscript, almost anything becomes possible with Vim.

The easiest way to install plugins... is with a plugin. Yes, [Tim Pope](https://github.com/tpope "Tim Pope on Github") created a plugin, Pathogen, which allows you to install other plugins with almost zero configuration.

For installation of Pathogen, head over to [https://github.com/tpope/vim-pathogen](https://github.com/tpope/vim-pathogen "Pathogen") and follow the instructions.

Add `execute pathogen#infect()` on top of your `.vimrc`, and voila! Now you're ready to install plugins.

# What's next?

We now have our base Vim configuration: We have a `.vimrc` file in place that we can add configuration options to. And we are ready to install some plugins.

As I'm learning and using Vim as we speak, I'll add more blog posts to tell you about my struggles, plugins I'm using and other stuff I've learned about Vim.

My goal is to stop using an IDE alltogether, and use Vim for all my source code editing. It won't be an easy task, but if others can manage it, then so can I. By blogging about my experiences, I hope I can help other people with their struggles.
