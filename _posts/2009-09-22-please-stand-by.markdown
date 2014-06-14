---
author: Chris Nicola
date: '2009-09-22 21:54:00'
layout: post
slug: please-stand-by
status: publish
title: Please stand by…
comments: true
wordpress_id: '41'
categories:
- blogging
---

I'm sure someone said once, "if something is worth doing, it's worth doing right".  I am firmly of the opinion this asshole had a lot of free time on his hands.  Nonetheless, once I start on something I tend to obsess till I either tire myself out or I am satisfied.  This has definitely been true of my decision to change blog software to a .NET blog (and as a prerequisite select a new hosting provider).  I have spent a bit more time than I probably should have to evaluate my options: Subtext, dasBlog and BlogEngine.Net.  I've installed them, skimmed the code, attempted to compile them from the trunk, broken them, failed to install them, tried out the skins, features, imported my blog posts, etc.  After all of it was said and done I came back to my first choice BlogEngine.Net.

<!--more-->

I even went so far as to get a second hosting provider and registered lucisferre.net to start testing these engines.  Entirely unnecessary I realize, I could have tested them here of course, actually to be honest the fact that I discovered very quickly that GoDaddy sucks balls probably motivated that change.  I've also decided I'm not sure I'm really an "org", but that's semantics really.

Unfortunately, all of this time spent has meant no actual blogging has been done.  Evaluating blog engines seems to be counter productive to the actual blogging process itself.  Even more unfortunately (at least for anyone now reading this) it also means I am going to do a post **_about_** my evaluation of said blogging engines.

Disclaimer:  I have in no way spent enough time with any of these packages to be considered an expert on any of them.  I don't want my criticism to be taken as any more than my initial impressions of each of them and why I came to my final decision.  I am not claiming I could do better, or than any of them "suck".  All that out of the way here were my impressions:

**Subtext:**

This is probably the package I spent the most time with.  It is a very popular blogging engine and has a good history and a good number of popular .NET related blogs use it.  It uses ASP.NET MVC framework, it uses IoC and has a lot of code behind it (and I mean that, too much perhaps).  It has some fairly decent looking skins, though I was a bit disappointed at the lack of "widgets" that could be quickly added or removed, this was a feature I really liked in b2evolution.  Subtext does now have a plugin framework, but it distinctly lacks actual plugins.

I was able to install it on my provider fairly easily.  Setup requires a bit of XML web.config editing, unless you have direct access to your server via RDP or something else, if you have shared hosting.  I have always found the web.config file a pain and when compared to configuration in PHP or Rails it comes up short.  I then I had to import my old blog data.  My first attempt to import via RSS to BlogEngine.NET left all the comments behind.  I downloaded the [RSS2BlogML][1] code and added my own provider for b2evolution (it works great and I will make the code available asap). 

I noticed my first [bug ][2]immediately after importing.  It's fixed in the trunk though, I just needed to get new stored proc's... wait... stored proc's?  So the first ding against subtext for me was that it uses stored proc's to interact with a database.  I then checked out the trunk and started to look at the code.  At least it seems to have a nice repository pattern going, wouldn't be to hard to write an nhibernate based provider really, so that's a plus.  I did notice almost every class was hundreds of lines long though and very hard to follow.  Hard to believe SRP is being followed here, but who am I to judge, you should see some of my code!!!

I played around with skins too, but that didn't work out any [better][3].  Restarting the application to updated configuration is one of my biggest beefs with .NET based web applications.  There are ways around it though and here is a perfect place for a feature request: "want to be able to add a new skin without restarting my web app".  I never got that skin to work.

Finally, I decided to try out the trunk, I figured maybe most of this is fixed in the trunk.  It took a while to get it working though, you can't actually "build" from the trunk easily,  they suggest you grab whatever is the latest successful build from they CCNET build server, which I did.  Trying to use the trunk was a poor choice though, the trunk seemed to have far more bugs than the latest release and was completely unusable for me.  Now a lot of you might be saying, "well duh, it's development stupid, only release is stable", and that tells me you don't fully understand what "continuous integration" implies.  Regardless as I had already discovered the release had enough bugs to turn me off using it I decided to finally dismiss it as my blog of choice despite coming very close.

Now I don't mean to be entirely negative about subtext, it's just that, well, my personal experience was definitely sub-par.  Clearly, it is a good product for most, Oren uses it and it has an excellent team of developers behind it.  I see many, many people with excellent subtext blogs out there.

**dasBlog:** 

I am not going to lie here, dB was probably the engine I spent the least time with.  I believe it is also an ASP.NET MVC web app, but I didn't spend enough time going through the code to be entirely certain.  It did seem to me the code was even more daunting than Subtext's, which was already pretty beefy.  I didn't have any problems installing it though, but I shouldn't really, it just uses XML files to store it's posts so that saves you having to set up a database.  To be honest this is probably a good thing, a SQL database is of questionable value when a blog engine is just a glorified XML generator anyways ;-).

I was again able to successfully import my BlogML backup so that made me happy, however I was not terribly happy with the configuration or admin interface.  I also couldn't settle on a skin I quite liked either.  Still dasBlog seemed to do the job, but having already setup BlogEngine.Net I was at a loss to define with either dasBlog or Subtext were doing that wasn't already working for me in BlogEngine.

**BlogEngine.NET:**

As I mentioned in my previous posts there are definitely warning bells for me when I look at BlogEngine.NET's code.  No unit tests, SQL queries in the code, a WebForms UI</SP  
an>, _too many notes..._ no wait, that's why I don't like Mozart.  Anyways, all of these things eat away at me as I sleep at night, but you know what, it just works.  Seriously, all this agile crap can get stuffed, if it doesn't work.  BlogEngine has all of the features (at least as far as I can tell) of any of any other .NET blog engine but it works quite smoothly.  Even despite its master page based WebForm interface the code seems clean and readable. 

That isn't to say BE.NEt is beter than Subtext or dasBlog, but I am saying after careful evaluation, I decided it was simpler and more comfortable for me personally to use.

Well, that's that,  I hope no one has any hurt feeling over what may have come off as a bit of a rant.  Personally, I feel the .NET blog engines have some distance to cover to catch up to the likes of Wordpress but they seem to be gaining speed.  Adam is still telling me to build my own blog engine, but I think that's because he wants to move away from blogger and he is hoping I'll do the heavy lifting ;-).  I do feel I would like to see a Monorail/Activerecord/Windsor based blog engine, so who knows?  For the short term I think I should try to keep my bloging time focused on actually writing blog posts and not so much on writing _or_ testing blog software.

As a result of all this, I now have a bit of a backlog of blog topics now in my head.  A few upcoming post ideas I have on the way:

  * Continuous Deployment: Using psake to automate click-once deployment via TeamCity's continuous integration server 
  * Creating fluent interfaces for the heck of it, or "all the cool kids are doing it"
  * Creating and using a simple message bus to separate service dependencies 

   [1]: http://blogml.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=171
   [2]: http://groups.google.com/group/subtext/browse_thread/thread/76103e706484b575
   [3]: http://groups.google.com/group/subtext/browse_thread/thread/fba7437ee59432f7

