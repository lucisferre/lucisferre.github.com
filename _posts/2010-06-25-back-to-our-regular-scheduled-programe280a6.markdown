---
title: Back to our regular scheduled program…
date: 2010-06-25 00:54:00 Z
categories:
- ".NET"
- photography
- travel
- blogging
- MEF
- nCQRS
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '80'
---

![tv static][1]

Well, not exactly.  I was really hoping not to do a blog post about why I have not been posting, it has become a bit of a cliché. Still, it's what I've got on my mind right now so why not?

The problem isn't that I have nothing to post about, but it is partly that I don't have anything brief.  I have been doing a lot lately, CQRS, DDD, TDD, Silverlight, WCF (ugh!), some ALT.NET stuff (well going out for beers anyways).  I just recently got back from Africa, we are in the middle of a kitchen renovation (going disastrously, thanks for asking) and work has been fairly hectic last month.

In lieu of anything more interesting you can check out my Picasa album of Africa pics.

[http://picasaweb.google.ca/chnicola/Africa?authkey=Gv1sRgCNPYt7KQ1dfE8wE#][2]

[http://picasaweb.google.ca/chnicola/Christopher?authkey=Gv1sRgCLeT-4WC4qXyxwE#][3]

<!--more-->

I am planning a couple of posts though, and this weekend I hope to get out at least one or two and post date them for the next couple of weeks.  I have a couple of code sample/tutorial style posts planned and I am eager to get them done so hopefully they get done soon to, but here is what I'm thinking about

### MAF -> MEF Sample Application

Right before I went to ALT.NET Seattle I was tasked with reviewing and documenting a add-ins framework at work that used MAF (managed add-ins framework).  Now the code behind this was pretty good, but MAF feels awkward, bloated and like an unfinished painting.  To even use it you have to download this [Pipeline Builder][4] from codeplex that MS never even finished properly.  Someone who used MAF had to fix it up themselves to get it working again.  _Note to Microsoft, open source software is not a licence to half-ass everything._

Anyways, I decided to do something about it and began to look into the _managed extensibility framework** -**_ MEF (you wouldn't believe how many confusing conversations I've had at work talking about MAF and MEF in the same discussion) which is an altogether different beast from MAF, but nonetheless serves some similar purposes in that it allows you to make application design more "pluggable" or in their words "extensible".

Well I started a conversation on Twitter with team lead Glen Block (@gblock) and started to ask about the possibility of switching one from the other.  Fortune had it we would both be at ALT.NET Seattle so we took about an hour or so during a Code-Dojo session and refactored one of the sample apps replacing MAF entirely with MAF.  It was quite easy in fact, so easy the when I got back from the conference I took some of my 'scheduled "documentation" time to replace it in our production app.  Successfully, I might add, the change is already in production and even fixed a couple of hard to pinpoint performance issues.

While Glen and I were doing this, I kept my Git workflow going and synced up with a Github repo so that everyone could follow our process.  So for anyone who has worked with a MAF application and is interested in what MEF has to offer please [feel free to pull it down][5].  A quick warning though, we did this in a hurry and did not have time to clean up (and there was a lot to clean up), or wire up all of the features of the calculates (the graphing part isn't working yet).

### NCQRS Quickstart

I have had the opportunity to start building a CQRS/DDD based system as part of my work (I know right!).  It's been quite an experience, I am finally beginning to grok the concepts of DDD much more clearly in the framework of a CQRS design.  I started "rolling my own" but after reviewing the code and design of the very new NCQRS framework I decided it would be much better to use and support NCQRS than to continue down my own path.  I plan to put together a simple getting started post soon, but I need time to put the code together.

   [1]: /images/tv-static.jpg (tv static)
   [2]: http://picasaweb.google.ca/chnicola/Africa?authkey=Gv1sRgCNPYt7KQ1dfE8wE# (http://picasaweb.google.ca/chnicola/Africa?authkey=Gv1sRgCNPYt7KQ1dfE8wE#)
   [3]: http://picasaweb.google.ca/chnicola/Christopher?authkey=Gv1sRgCLeT-4WC4qXyxwE# (http://picasaweb.google.ca/chnicola/Christopher?authkey=Gv1sRgCLeT-4WC4qXyxwE#)
   [4]: http://pipelinebuilder.codeplex.com/
   [5]: http://github.com/lucisferre/MAFtoMEFSample

