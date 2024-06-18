---
Title: Obsidian Pelican Setup
Date: 2023-12-28 08:50
Slug: obsidian_pelican_setup
tags: programming, website
Summary: How I've setup Obsidian to work with Pelican
---
# Obsidian and Pelican: A Powerful Combination

**NOTE** - I have moved away from this method and keep it here for reference only. I highly recommend https://quartz.jzhao.xyz/ 

Obsidian and Pelican are two versatile tools, enabling efficient note-taking in Markdown and seamless website publication from Markdown files, respectively. This integration can significantly enhance productivity and organization for users who seek a static website system in Python coupled with a robust note-taking platform.

## Pelican: Creating Static Websites

Pelican functions by converting Markdown files into HTML, enabling the creation of static websites. Additionally, it can serve these pages as a website for testing purposes.

## Obsidian: Advanced Note-Taking

Obsidian is a comprehensive note-taking application designed for Markdown files. Noteworthy features include automatic link updating and a visual representation of note relationships. These tools collectively form a potent system for creating a well-organized note-taking structure.

## Combining the Two Systems

Obsidian gives the ability to see how pages link together:

![[assets/obsidian_graph.png]]

While Pelican can put these files onto the web for others to see.

Since both Obsidian and Pelican utilize Markdown, merging them should ideally be straightforward. However, there are some challenges in the process that need attention.

## Addressing Compatibility Issues

**File Links**

While Obsidian primarily uses wikilinks or relative links, Pelican handles links differently:

Obsidian's link format. Ignore the `\` character:

```markdown
[[some_file.md]\]
[[../some_file.md|some_file]]
```

Pelican's link format:

```markdown
[[{filename}/some_file.md|some_file.md]]
```

To bridge this gap, a helpful plugin has been developed: Pelican-Obsidian Plugin. It resolves the discrepancies between the two systems, ensuring smoother integration. Additionally, an update for parsing pages has been proposed here.

**Handling Tags**

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

### Organizational Strategies

Various note-taking strategies exist within Obsidian, such as:


* Hierarchy of Notes (HON): Establishing links among notes, creating a hierarchical structure.

* Map of Content: Linking notes via tags without explicit hyperlinks.
* Pages and Posts: Utilizing website concepts for longer-term pages and time-ordered notes.

I personally structure pages as the primary hierarchy level, linking to all posts. This method deviates from conventional practices but suits my low-frequency posting style, preventing buried and hard-to-find posts.

Tags serve well for connecting related topics with minimal effort. Additionally, I utilize Obsidian's graph functionality to maintain a visual overview and prevent information from getting lost. (The graph can be visualized similarly to the homepages depicted as red dots.)

## Miscellaneous Configuration

To streamline my workflow, I've set up an ignored `drafts` folder and a `templates` folder in the Pelican configuration file. These folders are excluded when building the website, allowing focused work on specific content.

In conclusion, while the integration of Obsidian and Pelican presents some challenges regarding formatting and linking, the combined power of these tools offers a robust system for efficient note-taking and website creation.
