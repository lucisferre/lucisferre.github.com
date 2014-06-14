---
author: Chris Nicola
date: '2009-11-22 13:55:00'
layout: post
slug: aspnet-mvc-blog-engine-part-1-of-x
status: publish
title: 'ASP.NET MVC Blog Engine: Part 1 of X'
comments: true
wordpress_id: '50'
categories:
- .NET
- ASP.NET
- MVC
- nHibernate
---

![MVC8x6_209DEC49][1]

This will hopefully be my first post of many on ASP.NET MVC using [S#arp Architecture][2] (which I am going to abbreviate #A from this point on) and the [Spark][3] view engine.  I really wanted to start getting into MVC and #A provides a very quick and easy to use framework to build your site.  It also combines tools I would choose to use anyways like nHibernate and Windsor IoC.  If you have yet to look into #A and you are planning on doing any .NET MVC work in the future I would recommend giving it at least a first look.

<!--more-->

For my part, I've decided to pick the most cliche programming task there is, I am building a blogging engine.  Now, I'm not necessarily planning on doing this so I don't have to use BlogEngine.NET, in fact I like BE.NET for the most part.  I chose a blog engine for learning MVC for the same reason everyone likes using blog engines in examples, they have a very simple and well understood domain model and clearly defined functionality.  This leaves me free to focus on learning the archnitecture instead of on designing the software.

If anyone is interested in following along interactively, I have [put my code up on GitHub][4], so you can check it out, fork it, and generally do whatever you want.  So far I've managed the following:

  * Setup the S#arp Architecture project 
  * Changed the view engine to Spark 
  * Created the first domain entity: **Post**
  * Created the basic views to: 
    * List posts 
    * Create/Edit posts 
  * Created two #Arch "areas" one root and one for admin (creating/editing posts) 
  * Organized my views using the "three-pass" logic of Spark 
  * Added a jquery calendar component for editing dates 
  * Added CKEditor for editing the HTML content of posts 

Once you have checked out the code you simply need to create a local SQLExpress database called "Graphite", nHibernate will do the rest when you run the web project.

   [1]: /images/MVC8x6_209DEC49.png (MVC8x6_209DEC49)
   [2]: http://www.sharparchitecture.net/
   [3]: http://sparkviewengine.com
   [4]: https://github.com/lucisferre/Graphite

