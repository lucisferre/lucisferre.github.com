---
author: Chris Nicola
date: '2011-08-12 08:41:44'
layout: post
slug: more-quality-time-with-nancy
status: publish
title: More quality time with Nancy
comments: true
wordpress_id: '641'
categories:
- .NET
- Github
- NancyFx
- OSS
- Windsor
---

The project I'm working on uses Nancy pretty extensively for the RESTful API
portion. Working so much with Nancy has given me lots of opportunites to make
small improvements and tweaks to it as I go. A couple weeks ago I finally got
some time to submit some of the pull requests I've been meaning to get in,
which is always very satisfying. The two main things are: 

  1. Updated the [Windsor Bootstrapper][1] to be a separate project (like the
     other bootstrappers) for Nancy 0.7.1. It still needs a bit of work though,
     I realized after the fact that there is actually a different bootstrapper
     type I could have inherited from which makes more sense for Windsor.
  2. I've been using a custom [on error hook][2] which allows for custom
     filters to handle unhandled exceptions from any point during the pipeline
     and return a response from them. I find this useful in a few places.

<!--more-->

Now on this note I’ve had a couple people and ask me about how one gets started
contributing to open source projects. While there is certainly no big secret to
it, I think for many the initial barrier of where to start seems high, so here
are a couple of tips I hope encourage more people to start contributing. 

  1. First, you really **must** have a Github account and learn to use git,
     specifically how to fork a project and submit pull requests. Even for .NET
     most open source projects are on Github now (though a few stragglers still
     hang out on codeplex so learning Hg isn’t a bad idea either).
  2. Newly minted projects often have the most “low hanging fruit” available,
     but a mature project that you happen to use a lot will also offer lots of
     opportunities for you to find bugs or come up with potential features.
     Whatever it is pick something that interests you.
  3. Make sure to write unit tests for any code you plan to contribute, bitches
     love unit tests. Seriously though, most FOSS maintainers will not even
     look at bug fixes or features that have no tests.
  4. For any bug fix, make sure your test(s) fails on the latest version of the
     project. This clearly demonstrates the bug you are fixing. The test should
     obviously pass after your changes are applied. Even if you can't fix the
     bug submitting a failing test is 10x better than just explaining what the
     bug is.
  5. Just do it(tm). Find a bug, write a feature, keep it small but make sure
     you submit that pull request. Even if you think it is not perfect, the
     pull request will help you get feedback on your code. Most maintainers are
     very happy to encourage contributors. The Nancy project has yet to reject
     a single pull request I’ve submitted.

Contributing the FOSS is highly satisfying and fun. It is also a great learning opportunity. I highly recommend everyone take the time to experience it.

   [1]: https://github.com/lucisferre/Nancy.Bootstrappers.Windsor
   [2]: http://github.com/NancyFx/Nancy/pull/253

