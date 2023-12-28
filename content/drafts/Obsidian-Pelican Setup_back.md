---
Title: Obsidian Pelican Setup
Date: 2023-12-28 08:50
Slug: obsidian_pelican_setup
tags: programming, website
Author: Anthony Herrera
Summary: How I've setup Obsidian to work with Pelican
---

# Obsidian and Pelican

Obsidian is a program that is used to take notes in Markdown and Pelican is a tool
that is used to  publish websites from Markdown files. This sounds like a match made in heaven! 

While I'm not going to go over the two system in depth, I highly recommend checking them out if you want 
a static website system in Python and / or a note taking system.

## Pelican

This creates static websites. It takes Markdown files, converts them to HTML and can even serve the pages as a website
for testing

## Obsidian

Allows for note taking on Markdown files. It also allows for links that update automatically and the
ability to look at how the notes relate to each other. These two tools are very powerful 
for creating a coherent note system.


## Combining the two

Because both of these system use Markdown, it _should_ be pretty easy to combine them. There are a few hiccups along the way however.

#### File Links

The default links in Obsidian is either wikilinks or relative links. Both of which work okay. Example for linking to "../some_file.md"

```markdown
[[some_file.md]]
[some_file](../some_file.md)
```

However, Pelican handles links slightly differently

```markdown
[some_file.md]({filename}/some_file.md)
```

This can be solved by using this handy little plugin: https://github.com/jonathan-s/pelican-obsidian

It did not have support to also parses pages, so that was added in this PR: https://github.com/jonathan-s/pelican-obsidian/pull/6

But, if you don't want to wait for that to be merged in (if it ever is), the updated code can be found here: https://github.com/acherrera/pelican-obsidian/tree/main

#### Tags

This is an issue that came up when using the link converter. The tags need to be in a specific format or else it just doesn't work right. This issue was raised here: https://github.com/jonathan-s/pelican-obsidian/issues/5

Short version is that tags have to be in this format:

```markdown
name: anthony
tags: tag1, tag2, tag3
summary: some summary
```

instead of

```markdown
tags:
- tag1
- tag2
- tag3 
```

I'm not sure why both of those won't work, but may look into it more later.

### Organization

You can lose yourself in all the different types of note taking and how to use them. Very, very briefly

- Hierarchy of Notes (HON): Notes are linked together with a "higher" level note having multiple children
- Map of Content: Notes are linked together via tags, but not explicitly linked with hyperlinks
- Pages: Website concept of a longer term page
- Posts: Website concept of a time ordered note. Typically shorter than a page. Like a post on Facebook / Twitter / X / Myspace / whatever.

I have pages as the top level of the hierarchy that link to all the posts - this is not exactly how it's supposed 
to be done, but I'm not making a million posts a day, 
and having the posts ordered as a time series is going 
to result in them being buried and hard to find very quickly.

Tags are used to relate items with little though to what the other posts are. This works well if you just want to find related topics.

Finally, I use the graph functionality in Obsidian to make sure that nothing gets lost. Below you can see the homepages as red dots.

![[obsidian_graph.png]]


#### Ignored Files

Finally, to give myself the ability to work on things I have both a `drafts` folder and a `templates` folder that are ignored when building the website. These are set in the Pelican configuration file.


Obsidian and Pelican: A Powerful Combination

Obsidian and Pelican are two versatile tools, enabling efficient note-taking in Markdown and seamless website publication from Markdown files, respectively. This integration can significantly enhance productivity and organization for users who seek a static website system in Python coupled with a robust note-taking platform.
Pelican: Creating Static Websites

Pelican functions by converting Markdown files into HTML, enabling the creation of static websites. Additionally, it can serve these pages as a website for testing purposes.
Obsidian: Advanced Note-Taking

Obsidian is a comprehensive note-taking application designed for Markdown files. Noteworthy features include automatic link updating and a visual representation of note relationships. These tools collectively form a potent system for creating a well-organized note-taking structure.
Combining the Two Systems

Since both Obsidian and Pelican utilize Markdown, merging them should ideally be straightforward. However, there are some challenges in the process that need attention.
Addressing Compatibility Issues
File Links

While Obsidian primarily uses wikilinks or relative links, Pelican handles links differently:

Obsidian's link format:

```markdown
[[some_file.md]]
[some_file](../some_file.md)
```

Pelican's link format:

```markdown
[some_file.md]({filename}/some_file.md)
```

To bridge this gap, a helpful plugin has been developed: Pelican-Obsidian Plugin. It resolves the discrepancies between the two systems, ensuring smoother integration. Additionally, an update for parsing pages has been proposed here.
Handling Tags

An issue arises with tag formatting compatibility between the systems. Obsidian and Pelican interpret tag formats differently:

Correct tag format:

```markdown

name: anthony
tags: tag1, tag2, tag3
summary: some summary
```
This format works, whereas the following format does not:

```markdown
tags:
- tag1
- tag2
- tag3 
```

Further investigation might clarify the reasons behind this discrepancy.
Organizational Strategies

Various note-taking strategies exist within Obsidian, such as:


* Hierarchy of Notes (HON): Establishing links among notes, creating a hierarchical structure.

* Map of Content: Linking notes via tags without explicit hyperlinks.
* Pages and Posts: Utilizing website concepts for longer-term pages and time-ordered notes.

I personally structure pages as the primary hierarchy level, linking to all posts. This method deviates from conventional practices but suits my low-frequency posting style, preventing buried and hard-to-find posts.

Tags serve well for connecting related topics with minimal effort. Additionally, I utilize Obsidian's graph functionality to maintain a visual overview and prevent information from getting lost. (The graph can be visualized similarly to the homepages depicted as red dots.)
Miscellaneous Configuration

To streamline my workflow, I've set up an ignored drafts folder and a templates folder in the Pelican configuration file. These folders are excluded when building the website, allowing focused work on specific content.

In conclusion, while the integration of Obsidian and Pelican presents some challenges regarding formatting and linking, the combined power of these tools offers a robust system for efficient note-taking and website creation.