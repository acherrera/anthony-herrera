---
tags:
  - productivity_moc/note_taking
creation_date: 2023-11-30 07:50
summary: Notes on how Obsidian works
---
# Obsidian Setup


# Plugins

### Dataview

Used this in the past but decided it was not for me. The Markdown files become tied to Obsidian which kind of 
defeats my original purpose behind using Obisidian (File portability and longevity.)

### Templater 

Create better templates as things can be filled out within the note.

Make sure that templater runs when new files are created.
### QuickAdd

Create new notes quickly with information filled in based on title. [[templates/people_template.md|people_template]] updates the fields in the note when the title is filled out.

Then I go into the setting and map the templates to quick add functionality
### Vimrc support

Use this to enable vimrc. Primarily to remap `jk` to escape. This file should be located at `.obsidian.vimrc`. If interested, see [[NeoVim Init File]] for things I use in the full implementation. I would recommend just the following:

NOTE - the `Esc` part is case sensitive here.
```
imap jk <Esc>
" Yank to system clipboard 
set clipboard=unnamed
```