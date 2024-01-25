---
title: "Move to Neovim as my editor"
date: 2024-01-25T13:16:30-05:00
draft: false
---

## My Full Setup

I am maintaining my full development `.config` on github, so feel free to check it out: 
https://github.com/willemmirkovich/.config

## Intro

After experimenting with it for many months throughout 2023,
I have finally moved almost all of my professional/personal
development to [neovim](https://neovim.io/).

When I initially started out with neovim, I initially set things
up with an out of the box setup provided by another developer.
While this was helpful at first, it was based on what they needed
for development, not myself. So many plugins that I was configuring
either didn't make a ton of sense to me at first or were a bit overkill.
This led me to not really learning how the `init.lua` file was set, or
being bogged down by the shear weight of the setup.

## Successful move to Neovim

On my second go around, I took a different approach and went bottom up
on my own. This allowed me to practice the basics of `vim` again,
and think about the biggest things I needed from my editor, which are:

- syntax highlighting
- smart autocomplete suggestions
- quick code traversal (goto definition, search)
- auto-formatting

Once I knew what I needed, I was able to search for plugins, and add them
iteratively. Adding them one by one proved really helpful, I could learn
about their uses individually without having everything all at once and 
not connecting which plugin was doing what.

In addition to `neovim`, I have also moved to using [tmux](https://github.com/tmux/tmux)
and [alacritty](https://alacritty.org/), awesome projects that have brought
my setup all together. Now that I am at a 'version 1', I feel my development has
increased signifigantly, mostly due to some amazing individual plugins/packages:

- [telescope](https://github.com/nvim-telescope/telescope.nvim) needs no introduction, find
files/references throughout your repos in seconds
- [mason-lspconfig](https://github.com/williamboman/mason-lspconfig.nvim) enables quick
and easy setup of all the major `lsp` packages you will want for languages
- [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter) for great syntax highlighting
as well as other features I have yet to really dig into
- [nvim-tmux-navigator](https://github.com/alexghergh/nvim-tmux-navigation) for seamless transitioning
between panes/buffers between nvim & tmux

## Future Improvements

Here are some of the things I would like to do with my setup in the future:

- split up my `init.lua` file, as it is currently a little bloated
- find a good way to use `jupyter notebooks` with this setup, as I still
use `vscode` for this currently :(
