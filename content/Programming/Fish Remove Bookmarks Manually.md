---
Title: Manually Remove Fish Bookmarks
Date: 2023-11-12
Slug: manual_remove_fish_bookmarks
Tags: programming, programming/linux, programming/fish_terminal, terminal
Summary: How to remove bookmarks from the fish terminal when directory is already removed.
---

# Manually Remove Bookmarks From Fish 

Specifcally, this is using the `dangerous` theme.

## Background

This note is about how to fix an issue where a directory is removed, but fish (specifically the dangerous theme) still has it in the bookmarks. The error can look something like this:

```
cd: The directory “/home/tony/Downloads/drive-download-20230212T143225Z-001” does not exist
/usr/share/fish/functions/cd.fish (line 30): 
    builtin cd $argv
    ^
in function 'cd' with arguments '/home/tony/Downloads/drive-download-20230212T143225Z-001'
        called on line 743 of file ~/.config/fish/functions/fish_prompt.fish
from sourcing file ~/.config/fish/functions/fish_prompt.fish
        called on line 1 of file -
from sourcing file -
        called on line 17 of file ~/.config/fish/config.fish
from sourcing file ~/.config/fish/config.fish
        called during startup
```

In the above case, a directory was stored as the startup directory, but then the directory was removed. 
Sometimes, just running `echo $bookmarks` will show directories that no longer exist and need to be removed.  This works for either situation


## How To Fix

I went through the source code and found where this was being set. If you know the name of the directory you want to remove you can use this to get ride of it:

```bash
set -e bookmarks[(contains -i /home/usr/some_dir/ $bookmarks)]
```

If you are not sure what the directory is, you can use `echo $bookmarks` to show all of the bookmarks that are being tracked.

For the original issue, see [this](https://github.com/oh-my-fish/theme-dangerous/issues/13) link.
