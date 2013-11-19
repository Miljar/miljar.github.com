---
layout: post
title: "VIM for IDE-people - part 2"
date: 2013-09-24 16:25
comments: true
categories: [vim, macvim]
---

# Color Scheme: Solarized Plugin

Let's start with an easy one. When working with Netbeans, I had the [Solarized color scheme](http://ethanschoonover.com/solarized) installed. I was really used to this color scheme, and the first thing I did, was install it for Vim.

Installation is quite simple thanks to Pathogen: go to your `~/.vim/bundle` folder and `git clone git://github.com/altercation/vim-colors-solarized.git`

Then edit your `~/.vimrc` configuration file. Add the following if you like working with a dark background:

``` vim
syntax enable
set background=dark
colorscheme solarized
```

If you like the lighter background, then use this configuration:

``` vim
syntax enable
set background=light
colorscheme solarized
```

If you want to reload your `~/.vimrc` configuration in your current Vim instance, hit `:` and type `so ~/.vimrc` + enter.

{% img center /images/2013-09-24/screen-php-dark.png Vim Solarized %}

