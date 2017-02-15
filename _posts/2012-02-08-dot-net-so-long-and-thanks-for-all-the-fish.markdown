---
title: ".NET: So long and thanks for all the fish"
date: 2012-02-08 10:11:00 Z
categories:
- ".NET"
- rant
layout: post
comments: true
---

<img src="/images/dolphin.jpg" class="center" title="Dolphins are smart animals, they've left .NET, shouldn't you?">

Shortly after I return back from recharging my batteries in Maui there will be some big changes for me. First, I begin a new chapter in my career at a local startup called [Tictalking][tictalking] and second, this will mark the end of my days as a .NET developer for the foreseeable future. Now obviously I'm very excited about joining a startup for the first time, but I'm also excited---perhaps even a bit relieved---to be moving out of the Microsoft, Windows and .NET development stack. 

You'd probably have to have been living under a rock to have missed all the previous [blog][6] [posts][1] from [others][2] [leaving][3] .NET for greener pastures---or this very satirical one on [returning][4] to it. This type of posts is clearly quite cliche, and I should try to be above all of that, but I'm not and so I'm not leaving without firing at least a couple parting shots. So without further ado, here is my in depth "review" of .NET.

<!--more-->
###Boy meets .NET

.NET and I have had some good times. I think the first thing I really loved was Visual Studio. Between the project templates and intellisense I was quickly able to learn and do just enough to be dangerous. I had a simple web app running in a week and my first real desktop/database application, an insurance database, took a few months. But when I say "dangerous" I mean it. Like a fool I used everything the IDE (VS2008) could provide me with. From Winforms and Webforms to databinding with Strongly-Typed Datasets and all using every single visual designer surface I could find to build software---do *not* try this at home.

<img src="/images/looknocode.jpg" width="400" class="right">

Fortunately there was redemption early on for me. After reading a lot of blogs, attending DevTeach and ALT.NET Canada and becoming exposed to ReSharper I started to take my game to a whole new level. ReSharper's suggestions helped me understand the amount of redundancy that was lurking in my code. It improved my C# coding style even taught myself LINQ from R# suggesting when I could convert a loop to a LINQ statement. I learned how to use nHibernate, started writing unit tests, and a lot more after that.

Since those dark times, I've used just about everything *but* the kitchen sink: Webforms, MVC2, MVC3, LINQ2SQL, WPF, Silverlight, WCF, WTF... I've worked with many great open source .NET frameworks like nHibernate, Nancy, Machine.Specifications and Windsor and I've contributed pull requests to a good number of projects. 

Working with open source certainly lends a much appreciated boost to productivity but also, by way of providing solid and open examples of well designed code, can be an excellent teacher. I've learned much of what I know about C# and .NET from both using and contributing to OSS.

So .NET has been pretty good to me but not long ago I began to see the point some from the ex-.NET crowd was making. How some aspects of .NET and specifically C# are limiting and even sometimes counter-productive. This thinking eventually started me looking into the alternatives.

###It's not about the community

Now it's been a pretty common pattern to suggest the .NET community isn't as "good as the X community" because it's not as dedicated to open source or just not as open, honest or collaborative. For my part though, I've only had great experiences with the .NET community. 

I was first introduced to the .NET developer community by way of DevTeach and ALT.NET Canada years back. I've enjoyed being part of and helping to organize the local ALT.NET community in Vancouver since then. I've also really enjoyed and benefited from the hospitality of ALT.NET Seattle's open space---in fact, some of us here in Vancouver hope to reciprocate this hospitality with by organizing the upcoming 
[Polyglot Conference](http://polyglotconf.com). 

It has been because of and through this community that I started to develop and grow my knowledge of the wider world of software development. In fact, in a somewhat ironic way it is due in part to what I've learned from participating in the .NET and ALT.NET community, rather than because of any deficiencies with it, that I've now chosen this path away from .NET.

###I'll have my cake and eat it too, thank you

Productivity matters a lot to me, and having fun and enjoying my work matters just as much. When it comes to these things I fully intend to have my cake and eat it too.

The Visual Studio IDE, once a wonder for me, has become almost a banal chore. With each release it does arguably get better in terms of features and yet each day I like it less and less. It's slow, it's clumsy and it regularly gets in my way. I now find Intellisense to be clumsy and and it causes me grief when I have to constantly delete whatever it "intelligently" inserted for me. 

It seems as though the whole philosophy of .NET centres on catering to the novice, but even as the novice evolves into the "advanced beginner" they inevitably find they want and need more. More flexibility, more power, more *leverage*. .NET, and specifically C#, both coddles you and then scolds you at every turn. It starts to get in your way. Even modern language features that were designed to increase it's flexibility, like Generics and `dynamic`, fall way short of the mark in my opinion.

Writing extensible "open-closed" code almost always seems to require some complex use of patterns or techniques that are often muddy and verbose. Even then, the result is still often quite inflexible and brittle. By contrast, I've found solutions like Ruby's mixins perfect for providing clear understandable extensibility. I find languages like Ruby provide many paths to choose from when we design. C# by comparison seems to be littered with pitfalls and dead-ends.

I'll admit MS has made some effort to improve. Embracing both solutions from the open source ecosystem and creating some of their own. Things like Node.js, F#, PowerShell, Nuget even the IRON languages---despite the [minimal fanfare][8]---are all good efforts.  But, is it too little and too late? 

There is a lot of legacy built up in Windows, Visual Studio and .NET, not to mention a cultural momentum which makes significant change hard. Among .NET organizations there is a strong cultural impetus towards backwards compatibility and ensuring new technologies are "mature enough". There is a tendency to shy away from anything close the cutting edge. I suppose we'll just have to see what happens over the next couple of years. On the desktop front at least, there are certainly some interesting developments coming with Windows 8, the app marketplace and HTML5/JS applications.

Now I'm digressing a bit so let me just get to the bottom line. I just want languages, tools and frameworks to help me get work done and then get out of my way. Is Rails a clean code panacea? Is Javascript elegant? No. However, am I more productive, less burdened and do I have more options and flexibility? Absolutely. I am far less frustrated trying to produce elegant, clear, extensible and maintainable solutions in these dynamic languages. I also find that dynamic languages, modern web frameworks and \*nix is just more fun. That's all just my opinion obviously, but while were on the subject, I'll just drop in this quote from Alan Kay, the considered "father of OOP".

> Until real software engineering is developed, the next best practice is to develop with a dynamic system that has extreme late binding in all aspects.

> -- [Alan Kay](http://c2.com/cgi/wiki?AlanKaysDefinitionOfObjectOriented)

###Environment and Culture

Now, I *could* rant for a bit about the little things that seem to pervasive in  the .NET ecosystem. Things like the Microsoft "professional engineer" who advises you at your invitation, and then sends e-mails behind your back to executives to inform them of his concern about the "business risks" of non-Microsoft technology choices.

We could discuss the problem of the "enterprise software" fallacy which insists that "enterprise" is somehow different from other types of software development. How this becomes a common excuse to toss aside everything and anything others have found can improve the software development process---*we can't do continuous deployment here because this is enterprise software*.

I could go on to suggest that Microsoft is largely responsible for encouraging these attitudes through their marketing and FUD. I could wax philosophical about how counter productive this is, since it works against the best parts of the .NET community who are trying hard to encourage innovation and create a strong ecosystem to support developers.

I *could* go on about all this and the some... but I won't ;P.

Suffice to say, while I could suggest that I'm making this decision for a litany of reasons like this, I'm not. This has a lot more to do with what my *gut* tells me is the right decision for me at this time than any of that "grass is greener" whining does.

Finally, I do really want to thank iQmetrix for the opportunites they have afforded me. I've been given a lot of freedom. I've had the opportunity to work with a modern technology stack, the freedom to choose the best frameworks and tools and quite a lot of support. I've also worked with some very talented, hard working and intelligent people and I've learned a lot from them and from this experience.

<img class="right" src="/images/dont-panic.jpg" width="400">

Now I'm just excited about getting my feet wet in Vancouver's burgeoning startup ecosystem. Like I've previously mentioned [it is an exciting time to be a software developer][7] and my advice, if you are so inclined and are willing to take some risks, is to just make sure you don't let it pass you by.

[1]:http://whatupdave.com/post/1170718843/leaving-net
[2]:http://hkarthik.me/blog/2011/11/11/my-reasons-for-leaving-net/
[3]:http://osherove.com/blog/2011/1/2/the-journey-begins-and-why-it-starts-with-ruby.html
[4]:http://mecodegood.tumblr.com/post/14956918719/there-and-back-again-my-return-to-net-development
[6]:http://darrencauthon.posterous.com/i-didnt-leave-net-net-left-me
[7]:/2011/11/10/the-startup-rush/
[8]:http://blog.scottbellware.com/2010/04/ironruby-drops-does-it-make-sound.html
[tictalking]:http://tictalking.com
