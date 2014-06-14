---
author: Chris Nicola
date: '2010-01-27 18:01:00'
layout: post
slug: graphite-its-alive
status: publish
title: 'Graphite: It''s Alive!'
comments: true
wordpress_id: '64'
categories:
- .NET
- Software Development
- ASP.NET
- blogging
- graphite
- MVC
---

Today I managed to put about 5 hours in on Graphite and managed to finish off the BlogML importer.  While there are still a number of bugs the whole thing is pretty much working so I decided to deploy it for the first time.  I had yet to deploy it into a shared hosting environment so I wanted to make sure there wouldn't be any problems.

[Low and behold...][1]

It is still early days, but feel free to log in and look around.  You can use the User: admin Password: tester if you want to try it out while I am testing it.

<!--more-->

There is still a lot to do before I will consider moving my blog over to Graphite.  I have yet to add slug based links to the titles of each post and login/logout isn't quite working properly and a number of the views and links are not working right.  For the most part it is working pretty well though.  I did notice that the BlogML imported _all_ of the undeleted spam because I didn't bother to check the "approved" flag before importing.

So while there is a lot to do, it has come a long way in two months as a spare-time project.  Taking another look at  my laundry list from last time:

  * Can create/edit/delete blog posts
  * Can add comments to a post
  * Can create/edit/delete blog users
  * Users can login/logout and be authenticated to perform administrative tasks
  * Can show a standard RSS feed of posts
  * Can ping blogging services 
  * Can handle trackbacks from other blogs 
  * Can import from other blog engines (or from RSS feeds at least) 
  * Provide some form of comment spam protection (Askimet and/or Captcha) 
  * Some sort of twitter following plug-in 

The Atom feed seems broken though.  So now I need to work on the staple blog features like, trackbacks, pings, and comment spam protection.  I am going to add a couple more features to the queue now:

  * BlogML Import can migrate local files and images over (this could be a bit tricky) 
  * Ability to create themes/skins for the blog 
  * Simple UI for comment and post management 
  * OpenID based comment authentication (optionally enabled) 
  * DISQUS based comments (optionally enabled) 

    * Migrate tool to move local comments from the SQL DB to DISQUS 

Since I've used nHibernate, right out of the box Graphite should already support just about any database nHibernate supports, though I'll admit I have only tested with SQLExpress so far.  I also don't know if it will run under Mono, but my guess is the code should be compatible with Mono 2.6.

If you have any feature suggestions please leave them in the comments.

   [1]: http://graphite.lucisferre.net/

