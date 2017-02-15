---
title: The continuing adventures of a spam assassin
date: 2010-03-29 01:04:00 Z
categories:
- ".NET"
- blogengine
- blogging
- spam
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '74'
---

Yet another post about spam, I suppose I find spam kind of fascinating in some ways.  I mean, it is largely an automated, yet adaptive attack on a system and so the process of improving that system so that it is more resilient to the attacker is interesting to me.

First some good news is that the latest Hg checkins on the Codeplex site include [my changes][1] to both the spam filters and the whitelist/blacklist behaviour.  Even better news is that Codeplex is using Hg now! (I would have preferred Git, but beggars can't be choosers)

<!--more-->

I've been tracking my spam since adding the Type Pad filter and after 415 spam messages here are some results:

| Filter  | Spam Checked | Spam Missed | Spam Caught | % Spam Remaining |
| ------  | ------------ | ----------- | ----------- | ---------------- |
| SFS     | 414          | 241         | 173         | 58.2%            |
| Akismet | 241          | 45          | 193         | 10.9             |
| TypePad | 45           | 22          | 23          | 5.3%             |

The filters use a fall-through approach.  Any approved messages are passed onto the next filter in the list.  If any filter detects spam it is simply marked as spam and is not passed on further. 

Now the checked/caught numbers are a bit inflated since the blacklist was turned off (actually I broke it) which meant some of this is obvious repeat spamming.  The key metric is what made it all the way through to the last filter.  It managed to catch 50% of the remaining spam that was previously getting through.  Now this data doesn't suggest that Type Pad is better than Akismet, merely that using both together can cut down the missed spam by 50%.

This also shows that Stop Forum Spam is basically useless as a spam filter.

The second thing that really stands out is the shear volume of the spam, 525 comments in two weeks!  This unfortunately means I'm still deleting an average of 2 comments a day :(.  I now want to try a couple of things to see if I can slow this deluge.  One simple thing I did was to remove some site text containing some keywords and phrases I was noticing in my Analytics.  These were search terms that were clearly being used by spam bots to locate BE.NET sites as well as find the comment form.  These will henceforth only be referred to as "the-keywords-that-shall-not-be-named".

I'm also trying something creative with the comment form, and idea I stole from [here][2].  I've added an invisible email address field.  The hope is it will trick the spam bots to entering data there.  The validation will then refuse to process the form if the field contains any data, effectively stopping the bot in it's tracks.  It almost seems _too_ simple, so it will be interesting to see if this cheap trick is effective.  Check back here in a week to find out.

   [1]: http://lucisferre.net/2010/03/16/improving-blogenginenet-spam-filtering/
   [2]: http://www.webmasterworld.com/webmaster/3322243.htm

