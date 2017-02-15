---
title: Changing over to Blogengine.NET… for now
date: 2009-09-08 20:43:00 Z
categories:
- blogging
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '40'
---

Well I finally got fed up with a few things in b2evolution, specifically the following:

  * To many options to configure, way to much work just for a simple blog 
  * Everything required plugins, including code syntax highlighting and almost none of the plugins worked as expected.  I eventually ended up rolling my own plugins 
  * Having to write code in PHP and notepad++ to do anything 
  * I changed my mind about the merits of separating topics into multiple blogs using the multiblog feature 
  * No export to XML or BlogML or anything for that matter, which worried me.  The longer I stayed with b2evolution the more it would hurt to move away from it. 

<!--more-->

There are definitely other issues with it, but overall it isn't a bad blog engine, but I wanted to start exploring my options in .NET as it is something I understand and use a whole lot more than PHP.

I picked blogengine.net for now because it seemed the simplest design but to be fair I really haven't taken the time to evaluate dasBlog and Subtext.  It has an importer tool which was able to import my posts from b2evo via RSS (it isn't perfect but it did the trick and it is good enough).  BlogEngine.NET can also export the blogs in BlogML so I won't be stuck with it if I decide to change again later.

Adam suggested I "roll my own".  I think that is probably a bit more time than I really want to commit to my blog at this point, but it would be a good way to get some more experience with .NET web development.  Already I can see some chinks the armour for BE.NET.  It is extremely simple, but at the same time almost too simple.  The first thing that jumps out at me the total lack of unit tests in the downloaded source code, it makes me cringe in fear. 

The author made an effort to avoid any dependencies on third party libraries.  Unfortunately, this also means they have avoided using powerful tools like nHibernate, Castle Windsor (or other IoC containers), Monorail, etc. which can help make coding both cleaner and easier, but at the cost of learning how to work with these frameworks and understanding concepts like ORM, IoC and MVC... mmmm... TLAs ;-).  I suppose one could argue that a blog engine should be simple enough that it doesn't need those kinds of things, but I still think it would benefit greatly from them.

Anyways, maybe down the road I will roll my own, for now I am happy to be able to spend some time working with a blog engine that is coded in a language I understand well and can perhaps learn a bit from.  Then when I am ready I will consider perhaps either extending/contributing to the Blogengine.net project or writing my own from scratch.
