---
author: Chris Nicola
date: '2010-03-16 19:20:00'
layout: post
slug: improving-blogenginenet-spam-filtering
status: publish
title: Improving BlogEngine.NET spam filtering
comments: true
wordpress_id: '71'
categories:
- .NET
- blogengine
- blogging
- spam
---

BlogEngine.NET isn't a bad little blog engine, I mean I still intend to finish Graphite and move over too that, but in the meantime BE is doing it's thing for me.  I do have a few pet peeves though and a big one is spam.  BE attracts a ton of spam.  I think part of the problem is that it is popular enough to be a target and doesn't make it very hard for spammers to find the comment submit form (I see search keywords on my analytics page like "<removed search phrase for obvious reasons>").

<!--more-->

By default, it comes with two filters, StopForumSpam, which is so bad at detecting spam you may as well disable it (except for some reason you can't disable it) and Akismet.  Now Akismet does a decent job of filtering spam, but I'm still only getting about 85% of it and like I said BE attracts _a lot of spam_.  As another issue BE.NET has some [pretty stupid whitelist behavior][1] which can cause it to whitelist spammers.  Finally, the Akismet filter doesn't use the "FallThrough" property correctly, so if Akismet runs before StopForumSpam does then it allows SFS to re-approve it's spam. (a problem that is easily fixed by running them the other way around).

The other day I found out that TypePad's spam filter uses exactly the same api as Akismet and since BE.NET already has an Akismet filter, I just created a second filter that used the Akismet filter's api class.  I've posted the code as an [issue][2] over at the BE.NET codeplex site so I am hoping it will make it into the next release.  If you can't wait for better spam protection for blogengine then feel free to just [grab the code yourself][2].

I am hoping that between TypePad and Akismet I will be able to reduce the spam flow to barely a trickle.

   [1]: http://blogengine.codeplex.com/WorkItem/View.aspx?WorkItemId=11884
   [2]: http://blogengine.codeplex.com/WorkItem/View.aspx?WorkItemId=11885

