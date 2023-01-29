Title: Static vs Dynamic Websites
Date: 2023-01-29
Tags: Programming, Websites
Slug: static-dynamics-sites
Author: Anthony Herrera
Summary: Thoughts on statics vs dynamic websites

# Intro

This is a quick overview of static and dynamic websites, the difference between them 
and why I chose to build this website as a static website. Note - this article is 
for those creating websites from scratch. That is, if you are just going on 
Wordpress or Squarespace, creating a site and working on the GUI provided - no worries! 
You probably won't need to know any of this! While it might be nice to know the differences 
and how the systems work, those types of services take care of the hard part of setting 
everything and making sure it is secure.

If you're building a site from scratch or are interested in how they work, read on!

First, why should you care? The differences between the types of websites is an 
important first choice that you need to make when creating a site. Deciding after that 
face that you actually wanted a different type of website would be very challenging to switch.

I have built and started a variety of websites from simple Wordpress sites to more complicated 
"from scratch" builds in Python Django and Flask. After building a few of them, my advice is:
If you're building a website from scratch, use a static site if at all possible.

# Dynamic Websites

Put simply, these are sites that users can update and add content to. Think 
Facebook, Twitter, YouTube. The website grows and changes as users interact with it, 
each user can be shown a different page because the content is... dynamic. 

From a developer's point of view, this really means there is a database and a way
to interact with the database. Twitter is a good, simple example of this. The 
workflow looks a bit like this:

1. User logs in
2. User creates post
3. Post saved to database
4. Other users can see the post

You might have some tables `users`, `posts` and `comments` in your database. Posts are linked to users and comments 
are linked to users and posts. Point is, the user is able to interact and change the website. Great!

Note that there are many way to create a dynamic website so saying "It just has a database" is a gross simplification.
Facebook, YouTube and Twitter are all much more than "just a database" but that provide a decent starting point to
conceptualize the differences.

## Pros

* more interactive
* customize per user
* More expensive to run

## Cons

* More complex
* Must set up and maintain databases

# Static Websites

Static websites are just that - static. If one person views the site or a thousand 
people view it, the site will always be the same. An example of this is a simple 
review website. The author can make a review and "users" can see the review. That's 
it. No commenting, no saving to a profile, no likes, just the ability to see the review.

Unlike dynamic websites, the above is not too much of a simplification. While there are some odd things that *could* be
done to make it a little more dynamic, at the heart of it, the site is very much "what you see is what you get".


## Pros

* Easy to set up
* Easy to host
* Cheaper to run 
* faster to load

## Cons

* less flexible
* (potentially) less scalable

Let's talk about that last one. The static website can not scale functionality, but in terms of scaling for increased
user load, it seems like it should be more scaleable. Put simply, there is very little to actually scale. In the case of
the dynamic website, you have to scale the database size and the server. That static website is just a server and if
that is set up well, it could easily scale automatically.
