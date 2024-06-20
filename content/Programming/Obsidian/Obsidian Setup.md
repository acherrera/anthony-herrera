---
tags:
  - productivity
  - Obsidian
creation_date: 2024-05-29 09:00
summary: How Obsidian is set up
---
# Obsidian Setup

How Obsidian is set up. This should help me figure out how to configure things for the next time I need to create Obisidan.

## Plugins

Important ones:
* QuickAdd: Allows for quickly adding pages
* Tag Wrangler: Allow for easy moving tags back and forth
* Templater: Run template code on files
* Vimrc Support: Allows `jk` to be escape

Less Important:

* Calendar: Shows daily notes on the Calendar
* Minimal Theme Settings: Edits Minimal Theme
* Dataview: Useful for doing some query things, but breaks the "only markdown" philosophy

### Vimrc Support

Vimrc support is important for the VIM commands to work as I want. Need to create a file call .obsidian.vimrc should contain:

```
:imap jk <Esc>
```

That's it, but it makes a world of difference.


## Daily Notes

Basic daily not template: 

```

---
tags:
  - daily_note
date: <% tp.date.now("yyyy-MM-DD HH:mm") %>
summary: Note for the day
---

# <% moment(tp.file.title,'YYYY-MM-DD').format("dddd, MMMM DD, YYYY") %>

<< [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD-dddd').subtract(1, 'd').format('YYYY-MM-DD-dddd') %>|Yesterday]] | [[<% fileDate = moment(tp.file.title, 'YYYY-MM-DD-dddd').add(1, 'd').format('YYYY-MM-DD-dddd') %>|Tomorrow]] >>


# 📝 Notes
- 

---
##### Accomplishments
* 
```

Daily note configuration:

Date format: `YYYY/MM-MMM/YYYY-MM-DD-dddd`

New file location: `Timestamps/Daily_notes`

Template File location: `templates/notes_daily_template`

## Pasting Images

Under `Settings > Files and Links` set the default location for new attachments to be "subfolder under current" and name the subfolder "assets".