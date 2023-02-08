Title: VIM Thoughts
Date: 2023-01-30
Tags: Programming
Slug: vim-thoughts
Author: Anthony Herrera
Summary: Thoughts on using VIM


# VIM

Vim is such an interesting tool in the year 2023. It was originally created in the 70's or 
something for people who were using mainframe computers. Surely there are have been better tools 
created in the previous 50 year - and there is. VIM is also very difficult to use, being a modal 
editor that doesn't use the mouse at all. It's not very intuitive and doesn't look that great by default. So, why use it?

VIM has been a bit of a secret tool that I have used for the past 7 years or so. I started to 
learn it because it seemed like a magical tool that had been around since the dawn of ~~time~~ 
computers. It was originally used before the mouse existed, it could increase your efficiency by
removing the need to use the mouse at all! What could be better? Depending on your personal 
preferences, "Not spending a week learning the basics" might be better. So I set out and learned
the basics. With quite a bit of difficulty, I learned how to move around, how to type, how to
remap keys so make exiting easier and more. This introduced me to learning through the command 
line, setting up the environment and running code with minimal assistance. At the time I didn't
realize how important all of this would be in my life, but over the years it has really paid off.

# The Good

## Programming Close To The Machine

I call this method of programming "close to the machine" because there is very little between you
and the machine that the code is running on. Without an IDE you are forced to set the environment
up exactly as it should be. Which mean, when you're deploying code you're already a step ahead.
No wondering how to set environmental variables or what the environment should look like - you've
done it already! When learning how to set up a Postgres database or create a CI/CD pipeline, the
hard work is already done, it just becomes a matter of translating what you've already done to 
what the deployment pipeline expects.

## Lightweight Programming

VIM is lightweight. I mean, really lightweight. The program can run on most anything that has 
some form of Linux on it. You can set it up on a Raspberry Pi, SSH into the system and run it 
through the tunnel. While sure, it might sound impressive, the real value is that you have your
environment no matter what you are programming on. Once again, setting up servers, editing files
on a remote machine - all your tools are there and ready to go.

## Efficiency

This one is perhaps a little more debatable than the other points. When you're in the groove, 
programming, testing, and debugging it feels amazing. No touching that mouse, you can just think
and BAM! code comes out. Want to change something? Done. Want to make the same change 1000 times?
Easy. Want to refactor that value across multiple files? Google it, kind of figure it out, hey,
shut up. Combine the key-based editing system with the command line interface set up the same way 
and you can move between windows, across projects and more without touching that dirty, dirty 
mouse.

I mean, just look at how easy it is to open a tab in the terminal open a project and split the
terminal into a text and running area:

```
ctrl+b, ", ctrl+b, /, jk, k, 
```

Okay, I'm not so out of touch to think that looks easy. But it *feels* amazing.

## Life Time Editor

It hasn't gone away in 50 years, it probably won't be going away in the next few years either. 
That's it. Passionate users, installed on many, many systems and has at least two competing 
versions. Learn it once and use it forever.

## Plugins

So many plugins turn a complex, hard to use text editor into a complex, hard to use text editor
with pretty colors and more functionality that requires readying more documentation to figure out.
Really good stuff. In all seriousness, the plugins allow the text editor to much more than what
it does by default - show git changes, code linting, better highlighting, auto completion and more!
But, like I said, getting it running and be difficult and involve reading more documentation and 
reading github issues to diagnose.

# The Bad


## Learning Cliff

Very hard to pick up, very hard to learn, takes forever to learn. This may be literal, I actually 
don't know. After programming with it for 7 years, I still learn new things all the time. In fact, 
I encourage myself to occasionally go through and reconfigure plugins to see what I'm still using 
and what needs to go.

## Windows

Who needs Windows? Well, a lot of people. While VIM technically runs on Windows, it just feels 
wrong. It's difficult to explain exactly what is wrong - I think that without the terminal to 
provide the extra efficiency, it's reduced to a fancy text editor and nothing else. It's not a
member of a suite of amazing tools, just a hard to use text editor. And that makes me sad.

## Learning Cliff Again

I can't emphasis how difficult VIM is enough. 

# Conclusions

I think the best way I can sum up VIM is to say: VIM won't make you a good programmer, but by the
time you get good enough at it to argue the benefits with others, you'll have become a good 
programmer. It's a tool the requires dedication to learn, a pretty good understanding of computers 
to make useful (No play button to run code) and by the time you get a setup running with plugins
you like, you'll have plunged the depths of Github, getting burned by dead projects and 
incompatibilities, crashed your computer a few times, rebuilt from scratch, rebuilt again, 
misconfigured a key binding and bricked your setup, installed conflicting versions of VIM and
NeoVIM somehow, lost your SSH key and more! 

But in the end, those struggles make you a programmer worth paying that really understands the
systems and challenges.

Or, you know, use VSCode and be happy. That's good too.


Thanks for reading!