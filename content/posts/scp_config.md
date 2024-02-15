---
title: "Using `scp` with existing `ssh/config` file"
date: 2024-02-14T22:08:08-05:00
draft: false
---

Recently I have been needing to run various scripts/migrations on
a VM. Connecting via `ssh` using my `~/.ssh/config` file has been
really nice, as I can keep my port mappings and identity in a single spot.

One problem I ran into was needing to get the file contents from a tmux session
standard output that I was able to get using `tmux capture-pane`. Since copy/paste
isn't configured the same on this VM as my local machine, I wanted to quickly
copy the file over to my local and then get to copy/paste to my hearts content. But,
who wants to fill out the not fun host information in the `scp` command.

## Solution

Low and behold, `scp` has a `-F` option, allowing you to use host names specified in
your `.ssh/config` file!

```sh
scp -F ~/.ssh/config [Host]:/remote/path /local/path
```

Nothing ground breaking, but did want to share as I didn't find anything online when searching.
