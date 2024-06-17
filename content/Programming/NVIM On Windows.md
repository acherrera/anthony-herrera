---
tags:
  - programming/windows
creation_date: 2024-06-14 08:47
summary: How to use in them on Windows
---
# NVIM on Windows

This is something that I've been struggling with for a very long time. I am a fairly advanced user of NVIM on Ubuntu; however, I've never quite gotten a good workflow working with Windows.

After starting with a fresh Windows install, I figured it would be as good a time as any to really crack this problem open. To do that, I only have NVIM as my text editor and will be updating it as required.

## NVIM Init File

This is the key to updating settings in VIM - the init file. This file is located at:
`~\AppData\Local\nvim\init.vim`

It is possible to update your settings to use `~/_vimrc` by adding this to the above init file:

```
set runtimepath+=~/vimfiles,~/vimfiles/after
set packpath+=~/vimfiles
source ~/_vimrc
```

Found from [here](https://vi.stackexchange.com/questions/13505/where-do-i-put-my-vimrc-file-for-neovim-on-windows).

## Plug Installation

I had to download a file to `~\AppData\Local\nvim\autoload\` for it to work! Plug is used to install plugins in NVIM. For the Windows install, I removed all the plugins and slowly added them back in as I confirmed they worked.

## Microsoft Copilot Installation

Located [here](https://github.com/github/copilot.vim) - just need to install NodeJS and then run the PowerShell command shown in the README. After that, running `:Copilot setup` worked.

## Other Things

Navigating the file system is a little awkward as I have to use the NVIM built-in commands for the most part. I am used to having the terminal open and using the terminal commands to navigate the file system. This isn't a showstopper, but it will require some new techniques. It might mean going to PowerShell and learning a few commands or just suffering through the GUI file explorer.
