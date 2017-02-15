---
title: Riding the Rails with Heroku
date: 2011-09-20 19:56:18 Z
categories:
- NOT.NET
- heroku
- rails
- ruby
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '669'
---

Continuing on from last weekends Ruby on Rails fun I've continued working the
core functionality of [Pixel Publish][1]. Ruby on Rails definitely lives up to
the hype of being _fun_ to use So far I've managed to do almost everything I've
needed using preexisting gems, leaving most of _my_ work focused on getting a
solid domain model designed and working and gluing all the pieces together.
This is just a brief review of some of my experiences so far.

<!--more-->

### The Good

One really cool almost OOTB feature of Rails/Heroku is the [delayed job][2]. It
is basically a very, very, simple job queue, (word of warning though DJ is
technically using SQL as a Queue [which is bad][3]). Basically it makes it easy
to queue up longer running tasks that should not be performed by the web
service (which is a huge relief as I was really not happy with calling
ActiveMailer in process). Now a job queue really isn't anything new, not having
to build one yourself or do any real work to get it running in production is
seriously frakin' amazing. It's even straightforward to auto-scale the worker
dyno meaning that you aren't paying if there is no work to do.  


This is really the anti-thesis of my experience with Azure. Aside from not
having any real built in modular functionality that can just simple be _had_
(sure Azure Queues, but they don't actually do anything unless you make it
yourself) just try auto-scaling with Azure. Even if it were possible, prepare
to wait 10-20 minutes for that worker to start up.

For those thing yeah well DJ is a pretty naive job queue implementation, that's
probably true. However there are also far more robust alternatives like
[Resque][4] which is built on Redis and like DJ is easy to use with Heroku's
new Cedar stack. However, since I'm still experimenting _and_ since I'm already
using Postgres _and_ CouchDB I didn't feel like adding any more complexity I
didn't need... _yet_. The point here is simply that there are a lot of options
here to just getting s#%t done which is great.

### The Bad

As I stated last time getting started with Rails isn't without it's hiccups and
rails routing seems to be my arch nemesis. This [recent example][5] really
sticks out as something I find a bit kludgy and ugly. The worst part is how
hard it is to diagnose routing problems. There is almost no useful information
in the route error other than "No route matched". Try searching for that on
Stack Overflow (3000+ results). Rails routing is a brutal beast in desperate
need of some taming.

Though I'm no stranger to this experience, early on when I was learning .NET
MVC I found the built in .NET routing options just a bit stale and limited, but
quickly found going outside the box was [not without it's own pitfalls][6].
However, I persevered and eventually became a master of .NET MVC routing.  


The truth is, I've never particularly loved the _faux REST_ style of MVC
frameworks, Rails included. The fact is they corner you into a box under the
premise of "RESTfulness" and yet it couldn't be any further from actually being
REST. Besides if I'm using MVC I'm probably not building an API, I'm building
the site. I think it is still quite hard to do both at once and not compromise
one or the other (or both).

It's for this exact reason we currently use both [Nancy][7] and .NET MVC on the
project I'm working on. Honestly, nothing beats [Sinatra][sinatra] inspired
syntax for designing RESTful APIs. So, why am I not using Sinatra? Well because
I'm curious. Rumor has it that despite the routing snags, Rails is still the
getting s#!t done king of the hill and I want to make sure I've given it a full
and proper test drive. So far the good parts far outweigh the bad so trudge
along I will.

Next Time: My mixed emotions about unobtrusive javascript

   [1]: http://pixelpublish.com
   [2]: http://devcenter.heroku.com/articles/delayed-job
   [3]: http://www.engineyard.com/blog/2011/5-subtle-ways-youre-using-mysql-as-a-queue-and-why-itll-bite-you/
   [4]: http://blog.redistogo.com/2010/07/26/resque-with-redis-to-go/
   [5]: http://stackoverflow.com/questions/7465585/yet-another-no-route-matches-question
   [6]: http://lucisferre.net/2009/11/29/simplyrestfulrouting-wasne28099t-so-simple-for-me/
   [7]: http://github.com/NancyFx/Nancy

