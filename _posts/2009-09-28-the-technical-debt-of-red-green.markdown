---
author: Chris Nicola
date: '2009-09-28 09:15:00'
layout: post
slug: the-technical-debt-of-red-green
status: publish
title: The Technical Debt of Red Green
comments: true
wordpress_id: '42'
categories:
- agile
- rant
- technical debt
---
![images_thumb][2]

Recently, I have been seeing a lot of tweets and a couple of blog posts responding to [this post by Joel Spolsky][1] on “The Duct Tape Programmer”.  In it, Joel implies that agilists just don’t get the importance of having a “get ‘er done” attitude that the so-called “duct tape programmer” has.  Apparently, the “Red Green School of Software Development” has much to teach all these ivory tower agilists about how to wield "the handyman’s secret weapon."

<!--more-->

This, of course has met with a fairly poor response in the agile community.  A couple of the blogged responses can be seen [here][4] from Jeffery Palermo and [here][5] from “Uncle” Bob Martin.

Uncle Bob is actually pretty fair with Joel.  He agrees that the “spirit” of Joel’s post is right on the money if perhaps the “specifics” are a bit hazy.  Uncle Bob’s, post is excellent,  however I'm not sure I agree that by ignoring the specifics of Joel's post make it a "really good post."

For me, ignoring the _specifics_ of Joel’s post would be a bit like ignoring the _specifics_ of a software implementation as long as it gets the job done, which is exactly what Joel is suggesting we can do.

Joel says, tests can wait, if they are even needed at all, because mostly they just waste time.  He suggests that clean code and proper use of patterns and good practices are equivalent to “using complex coding techniques” that will “doom your project”.  Finally, he implies that getting the job done despite what to me sounds like lack of understanding of how to use said patterns or the value of unit tests shows great talent…  You see, Joel isn’t just suggesting we use a little duct tape, which is what Uncle Bob says (and rightfully so) is ok, he is suggesting we use it liberally if it will get the product out the door. 

This, I have a problem with, because for most companies and situations **the goal of software development should not be to simply produce a product but to provide a service**._  _By this, I mean, the software we design should be able to be constantly improved over time.  Each new version should provide increasing value to clients, otherwise, why are they still paying you?  I certainly hope they aren’t paying for you to fix all the places where you used duct tape.

Duct Tape solutions, may offer you a “tactical shortcut” but this inevitably incurs the cost of technical debt in the long run. [Adam][6] and I have recently been discussing how our backlog works and the need to separate technical debt and bug fixing from features and architecture.  As Adam explains, work done dealing with technical debt and bug fixing _does not count towards your velocity._  This is important because it means it is not productive work.  By using duct tape you reduce your future productivity, duct tape programming has diminishing returns the moment you use it.  Proper architecture, clean code and _test-first_ _design_ have steady increasing returns.

Being Canadian, I happen to know a thing or two about duct tape and it seems pretty clear, by Joel’s choice of analogy, that he has never seen the Red Green show.  Anyone who has, knows that Red has never once built anything that worked with his liberal use of “the handyman’s secret weapon,” duct tape.  In general, if you use duct tape instead of careful design to build complex things someone (usually Red) is gonna get hurt.

   [1]: http://www.joelonsoftware.com/items/2009/09/23.html
   [2]: /images/images_thumb_thumb.jpg (images_thumb)
   [3]: /images/images_thumb.jpg
   [4]: http://jeffreypalermo.com/blog/debunking-the-duct-tape-programmer/
   [5]: http://blog.objectmentor.com/articles/2009/09/24/the-duct-tape-programmer
   [6]: http://adventuresinagile.blogspot.com/

