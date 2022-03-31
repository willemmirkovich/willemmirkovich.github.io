---
title: "Vscode Jupyter Vim Keymap"
date: 2022-03-30T21:09:43-06:00
draft: false
---

As of late, I have really enjoyed using [Jupyter Notebooks](https://jupyter.org/)
for my exploratory programming and data science. While immenesly user friendly in the browser, I have missed some of the following functionality I have come to use in my VSCode IDE:

- auto-complete
- syntax highlighting
- import warnings/missing package info

For this reason, I have moved all of my jupyter development to 
[VS Code's Jupyter Environment](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter). 
This extension comes baked with all the IDE utiliies I am 
accustomed to...

*But*, I am missing my Vim keybindings for maniupulating/navigating
notebook cells! The great [Jupyter Vim binding](https://github.com/lambdalisue/jupyter-vim-binding)
project has great vim support for editting notebooks in the browser, such as

- `y` to copy a cell
- `p` to paste a cell
- `j`/`k` for navigating up and down

among others.

So, to get this same functionality, I have implemented the 
[Juypter Vim Keymap Extension](https://marketplace.visualstudio.com/items?itemName=willemmirkovich.jupyter-vim-keymap), meant to work alongside the
great [Vim emulation Extension](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim).

This package is very much in its infancy as it currently stands, but 
includes the following functionality currently:

- `j` & `k` for navigating cells down/up respectively
- `y`, `x` & `p` to copy/cut/paste cells 
- `u` to undo 
- `gg` and `shift+g` to jump to top/bottom cell
- `o` and `shift+o` to open new cell below/above
- `i` to edit cell

As stated, this extension is very fresh and needs work to include more 
vim expected features. 

Hope someone gets some use out of this! Hoping to add a lot in the future.

Checkout the source => https://github.com/willemmirkovich/vscode-jupyter-keymap-vim 