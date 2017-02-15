---
title: LINQ is elegant part 2
date: 2010-12-08 12:48:15 Z
categories:
- ".NET"
- C#
- linq
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '257'
---

I still love using LINQ and lambda syntax in C#. It enables some really cool opportunities to write really concise and neat code really easily. Combining this with static extension methods allow you to extend LINQ for your own fun and profit. For some this is probably a bit of a simple example but if you are unfamiliar with any of these concepts I think it illustrates how they work in a nice elegant way. 

{% gist 733892 %}

A winning combination of `Func<>1` for lambda/LINQ syntax, yield return for delayed evaluation wrapped in a static extension method. This is the bread and butter of C#. If this is all looking a bit Greek, it is pretty much the same as normal LINQ `.Select()` except that it will operate only on a random subset of size ‘count’ of the original collection.  There is in fact a small bug in the code I’ve just noticed (depending on what you wanted it to do) which I’ll leave and see if anyone points it out. Also you’ll notice I’m using Gist for code snippets now. I’m also planning on using Flickr for all images. This should make migrating blogs much easier and avoid some of the problems with other methods of posting code. Use the best tool for the job, right?
