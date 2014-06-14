---
author: Chris Nicola
date: '2010-04-29 07:58:00'
layout: post
slug: chris-nicola-blade-runner
status: publish
title: Chris Nicola… Blade Runner
comments: true
wordpress_id: '78'
categories:
- .NET
- blogengine
- blogging
- spam
---

Well I promised I'd follow up on [this post][1] however it will be my last post on blog spam I think (I don't want this to turn into a blog about dealing with spam).  So far, my [experiment][2] with confusing the bots has been fairly successful.  I’ve added a simple hidden form field disguised as the e-mail field.  Some JavaScript validation will block any submission where that field contains any input.  This scheme has managed to reduce the number of spam comments received from and average 37 comments a day to 21 (about 45%).  Even better is that it seems to have fooled the most successful bots at beating the Akismet and Typepad filters, as the amount of comments getting through the filters went down from 2 a day to less than 2 a week (either that or those filters have gotten better lately).  

![bladerunner-voight-kampff1][3]

Overall it has been a resounding success, and has reduced the spam deluge to more of a trickle.  I still think I can still tweak the form trap to make it better, but I'm happy with the level of automated protection I've managed to achieve without resorting to annoying captcha style human verification or implementing a full fledged VK test.

   [1]: http://lucisferre.net/2010/03/29/the-continuing-adventures-of-a-spam-assassin/
   [2]: http://blogengine.codeplex.com/Thread/View.aspx?ThreadId=208176
   [3]: /images/bladerunner-voight-kampff1.jpg (bladerunner-voight-kampff1)

