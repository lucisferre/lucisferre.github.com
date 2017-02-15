---
title: Google Analytics Plugin for b2evolution 3.2.0
date: 2009-06-17 15:56:00 Z
categories:
- NOT.NET
- Software Development
- analytics
- blogging
- PHP
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '17'
---

When I upgraded my blog software recently it broke the GA plugin I was using. Which is sad cause I like to see all the pretty visitors. I found a [new plugin][1] that appeared to work shortly after, but for the past two weeks I have had no visitors (I know, sad panda).

I eventually figured it out. The new plugin I found however was also broken.  The plugin neglected (or Google changed the script) to add brackets around the GA user number. This was very small so it was hard to notice so I just assumed no one cared about my site (which is also entirely possible but there is no way to be sure when the plugin is broken).

There are several comments on the authors site with no answer so I assume he is not updating it at all anymore. So I have taken it upon myself to fix the plugin as well as package it property (getting b2evo to recognize this plugin requires renaming the file as it was not properly named).  GA plugins are not very complicated so it isn't much of a change but I will make it available to download here for those who want a trouble free GA plugin for b2evo.

**Update**: I have removed the download link as it is broken.  I'm not actually sure if I still have the files or if it even works with the latest B2Evo.  I’m a bit surprised nobody has created another one, and that this post is even getting traffic.  Seriously, if anyone is still using b2evo I’ve got to ask… why?

   [1]: http://www.travisswicegood.com/2008/01/26/google-analytics-plugin-for-b2evolution/

