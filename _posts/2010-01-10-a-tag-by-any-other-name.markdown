---
title: A Tag by any other name.
date: 2010-01-10 16:39:00 Z
categories:
- blogging
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '61'
---

![tagcloud][1]

I have really enjoyed working on Graphite lately, which has actually made it hard to blog about it since I am having trouble tearing my self away from the code long enough to write a post.  The progress has been pretty good so far.  Just to update from last time I have added an RSS and Atom feed that uses the System.ServiceModel.Syndication API.  I have also added tagging to posts (thinking about tags for comments too).

One of the thing that occurred to me while implementing tags was that I didn’t really need both categories _and_ tags.  While it is true that most blogs support both categories and tags, I honestly can’t tell you functionally what’s the difference.  What functionality does the category provide that the tag doesn’t or can’t? - That isn’t a rhetorical question, if anyone can answer it please do.

<!--more-->

Tags seem almost like an evolution of categories.  Tags are intended to be easy to use, flexible and dynamic.  Tags tend to stay more relevant and current than static categories.  Most tag interfaces will let you type out new ones as you need them and usually auto-hint previously used tags so you don’t end up with C# and csharp _and “_c sharp” all as tags. 

BlogEngine.NET supports both tags and categories, you can select multiple categories for a post (just like tags) and even nest  your categories into a hierarchy.  You can see both a category list and a tag cloud on the right side of my site.   Technically the tag cloud could be a category list, with post counts and even RSS feeds for each tag but it isn’t.  That’s because in general we feel that a tag cloud is a more useful visual representation, if it wasn’t we would just display our tags like our categories.  Clearly we feel this is a superior way to organize blog posts.

Both RSS feeds and BlogML (which I am using as an import/export format) only define a categories item and not tags.  This generally means that if you are importing content from a blog that has both categories and tags you can only bring over one of the two, or merge them both.  Well, in that case I would choose my tags and by not implementing both I can ensure Graphite users will not ever have this problem.

Until someone can convince me otherwise categories will not be a feature in Graphite, only tags.  This will allow me to focus on providing better functionality built around tags alone, including tag management, and perhaps even _tag-categorization_ which I can see being potentially useful.

_Afterthought_: I just Googled this topic after writing this and found these two old posts debating the same thing. [Duncan Mackenzie][3] seems to agree and decided to do away with categories as well, while [Phil Haack][4] defends categories as “a structural element and navigational aid… …a way to group posts into large high-level groupings”

I’m not sure I really understand what Phil is trying to say there, but it seems like a lot to expect from a lowly category. I don’t really agree with the ‘use categories sparingly’ and ‘use tags liberally’ suggestions either.  Keeping a few categories with large numbers of posts doesn’t really help people find relevant content any more than liberally using tags which is likely to dilute their value and lead to a **tag fog**… [*ahem*][5].

In the end I don’t think it matters that much how we _define_ the difference but what practical examples tell us.  Do many blogs actually use both categories and tags in a way that separates them into two clearly definable functions?

   [1]: /images/tagcloud_thumb.jpg (tagcloud)
   [3]: http://www.duncanmackenzie.net/blog/categories-vs-tags-in-blogs-and-blog-editors/
   [4]: http://haacked.com/archive/2006/09/27/Categories_vs_Tags.aspx
   [5]: http://haacked.com/Tags/default.aspx

