---
author: Chris Nicola
date: '2010-09-22 20:11:16'
layout: post
slug: my-pragma-just-killed-your-dogma
status: publish
title: My pragma just killed your dogma
comments: true
wordpress_id: '86'
categories:
- agile
- ASP.NET
- clean code
- git
- lean
- MVC
---

![beware-dogma][1]

_I realize that "pragma" isn't really a word outside of compiler directives but it is the root of "pragmatism" and it made for a catchy title._

In the world of software development there may be no statement that can so easily end an argument than to dub it a "religious debate."  I would tend to argue this is akin to [Godwin's Law][3] and could be expressed as:

> Arguments around programming often wind up with someone claiming hopelessness as it is clearly a "religious debate".  This introduces an artificial stalemate by suggesting that neither side will ever be swayed.  It further implies that both sides are right (and/or wrong) together.

<!--more-->

There are definitely a few “religious” debates in programming which one could fairly argue are really at a stalemate.  The age-old Vim v. Emacs is a classic, Python v. Ruby - the languages not the frameworks around them - is a hot one too.  There are certainly others, Java v. C# has kind of died down I think.  However, the generic assumption that any time two programmers disagree vehemently about something that must be "religious" is somewhat prevalent.

This has had me thinking about whether my opinions mostly center on issues that are merely "programming religion" and really have no discernable merit over the contrary.  Which in turn has caused me to question how much of what I believe to be the _right_ way or the _best_ tool is simply dogmatic - hence, the use of the religious metaphor for this particular stream of consciousness that's turning into a blog post.

I'm going to make the bold assertion that I do in fact regularly question my own assumptions - eventually anyway, since anyone who's had an argument with me before probably has doubts about that claim - and my end goal is always to find what works, but with an eye out for better ways to do things.  Challenging assumptions will always involve some amount of trying different ways of doing things. So I'd like to think I lean on the pragmatic side, but at the same time, there are certain things I certainly feel pretty dogmatic about.

Here are a few of my "religious" coding beliefs:

  1. I believe Git to be a superior SCM system in every respect even though I have only briefly tried Hg and only used SVN for a few years (longer than I've used Git though). 
  2. I believe in [K&R][4] and [concise coding style][5] and most (if not all) of what is preached in Uncle Bob's book.  And I feel most of the [Kernel style guide][6] still pretty much apply to all C-like languages: C#, Java, Javascript, etc (I've really never used C++ so I'll keep my mouth shut on that one). 
  3. I believe that consistent coding style is important, even if only for the sake of consistency, though should arguments arise regarding style, see #2. 
  4. I believe continuous integration and deployment should be one of the most important practices of any team working on code along with a strong dedication to TDD. 
  5. I believe text editors like Vim and Emacs are worth learning even if they offer a major learning curve but I don't believe I will ever care which one is best (though I've chosen to learn Vim). 
  6. I believe that being a polyglot (both in languages, frameworks and tools) is an important and necessary part of programming, and will be for the foreseeable future. 
  7. I believe that MVC is currently the _right _way to do web development in .NET and that anyone using WebForms when they have other options is kidding themselves.  At the same time I don't believe that .NET MVC is necessarily the _best _way to do web development but it works. 
  8. I believe that using open source frameworks to do software development is awesome in uncountable ways and should be embraced by all. 
  9. Finally, I believe that VB and VB.NET are fugly. that is all.

While I feel pretty strongly about all of these things, and you could even accuse me of a dogmatic devotion to them, I've developed these "beliefs" out of a practice of pragmatism and being willing to try new and sometimes better ways of doing things.  Typically, after taking the advice of others when they've suggested things they've found to be better.  Nothing will ever perfect, so we can safely assume that there will always be new and frequently better ways to do what we do available to us -though Vim and Emacs seem to be defying the odds.

I will freely admit that I do enjoy change and trying out new ways of doing my work.  For me, trying out a new FOSS framework is like getting a free Christmas present (one which occasionally sucks like getting a sweater when your 5).  Though to be fair, software development is still a fairly recent calling, so perhaps maybe I will embrace change less and become more dogmatic as time progresses.  I expect perhaps more dogmatic, but I doubt I will enjoy change and the newness of things any less.

One could argue - and many have - that too much change slows things, hampers efficiency and your ability to deliver working software.  However, I've almost only ever found the opposite to be true, and I can tolerate quite a lot of change.  Sure, you could take change to an unhealthy extreme, but change is still hard, time consuming and tiring.  I can't say I imagine there are a lot of people who have the energy to embrace that much change.

"Discovering better ways of developing software," as the Agile manifesto begins, is rewarding in so many ways.  It's what keeps me both excited and interested in what I do, and as a bonus it has always seemed to work out to increase both the efficiency and quality of the software I deliver.  While I would never be able to show it with numbers, I'd find it hard to believe that it has been wasted time.

   [1]: /images/beware-dogma_thumb.jpg (beware-dogma)
   [2]: /images/beware-dogma.jpg
   [3]: http://en.wikipedia.org/wiki/Godwin's_law
   [4]: http://en.wikipedia.org/wiki/The_C_Programming_Language_(book)
   [5]: http://www.paulgraham.com/power.html
   [6]: https://computing.llnl.gov/linux/slurm/coding_style.pdf

