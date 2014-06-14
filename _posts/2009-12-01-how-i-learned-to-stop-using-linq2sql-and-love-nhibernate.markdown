---
author: Chris Nicola
date: '2009-12-01 19:55:00'
layout: post
slug: how-i-learned-to-stop-using-linq2sql-and-love-nhibernate
status: publish
title: How I learned to stop using LINQ2SQL and love nHibernate
comments: true
wordpress_id: '53'
categories:
- .NET
- Software Development
- linq
- linq2sql
- nHibernate
---

![NHLogoSmall][1]

Ok I will admit up front that it is entirely possible I have no idea how to use LINQ2SQL correctly, and I will further caveat this post by noting that I’m not saying you should never, ever use L2S, what I am saying is _if_ you choose to use L2S in any serious production environment that decision will almost certainly come back and haunt you… in the middle of your dreams… physically (and not in a totally awesome, _Ray Stanz in ghostbusters,_ kind of physically).  I don’t use L2S often, I use nHibernate, and the times I have used it have been when I need a simple and quick query on tables in a legacy database and I want _objects_.  Always query-only mind you, no CRUD of course (for the love of God, never use L2S for CRUD).  I will even admit I have a habit of *shudder* just dragging the tables into the designer *lightning cracks outside my window*.

<!--more-->

One of the reasons I use L2S at all is that I am pretty much a LINQ junkie and_ _I love writing LINQ queries, I think they are awesome, maybe not quite generics awesome but pretty damn awesome all the same.  I will take any opportunity to query the database in a strongly typed manner because it pretty much “whoop’s Batman’s ass,” and this is also the reason why I use LINQ2nHibernate even though it isn’t 100% functional.  L2nH is more than functional enough for any queries you should need to write and it’s LINQ, so basically it wins.

Anyways, today I found out why I only ever use LINQ2SQL to do small tasks in largely throw-away pieces of test code (assuming I bother to use it even for that again).

What I was trying to do, was a simple bulk load of data from an old database into a new web app.  We wanted to get some sample data from this database which will eventually be the source for the data this web app shows, but for now we just want to load it.  The web app uses nHibernate, in fact, it uses S#arp Architecture which I’ve previously mentioned is awesome, but I figured I would just use some throw away L2S mapping to get the data I needed and some LINQ to create the nHibernate entities from it.

This, for the most part worked fine, until I hit a table that had over 1.6 million records, which is a good chunk, but I’m not sure I would even call that the high end of “a lot” in larger databases.  No matter what I did or how I batched the loading of data from this table I could not get through all the records in this table without getting a DB timeout error. 

I started trying by using a single L2S _DataContext_ and calling something like:
    
    db.Transactions.Skip(i * 100000).Take(100000).ToList();

in a loop until I hit the end of the table.  There was a little more to it than that but pretty much that.  The memory usage continued climbing and I was trying to figure this out, then after about 700k transactions a _CommandTimeout_ exception was thrown (sorry I don’t have the exact exception anymore).  I thought that perhaps keeping the same _DataContext_ open for too long could be the problem (though I’ve never had a problem with that before) so I started to create a new context in a using block each time, I figured this might also help with the memory problem… no luck.  Amazingly exactly the same result, memory was not clearing and the things still timed out about the same point.

I figured just about anything could be causing it, I was using a local database on my dev machine that might have been to slow, I could have been using L2S incorrectly for this scenario too, in the end I de

cided using L2S in this scenario was bad (bulk loading of SQL should typically be done with ADO.NET but I hate it) so I got out the old _SqlDataReader_, called .Read() in a loop and voila! Perfection, and quite fast too, all while keeping the memory usage well below 150MB.  Impressive, considering the fact that I was just building one giant transaction for for nHibernate during this whole time with all 1.6 million inserts waiting to happen _and_ I was running this code in the R# 4.5 nUnit test runner.

Now I want to point out that I was very, very, wrong to use L2S for a bulk loading scenario, it is not it’s intended function, in fact, it is also bad practice to use nHibernate in this type of scenario, something that the nHibernate contributers have stressed every time someone comes in with benchmarks comparing nHibernate’s bulk loading capability to ‘X’.  However, what I want to point out is that nHibernate (using _StatelessSession_) was able to perform _1.6million inserts in 2.3 minutes _without batting an eye, but L2S couldn’t even handle _reading_ through 1.6 million records, I would shudder to think what would happen if I tried to do inserts.

Well that’s it for now, I just wanted to rant a bit after struggling with L2S.  While I may still use it for easy simple things, I have learned why I should never use it in any serious development scenario.  It is a **bad time** waiting to happen.  Also nHibernate StatelessSession is awesome and fast and you should use it… that is all.

   [1]: /images/NHLogoSmall1.gif (NHLogoSmall)

