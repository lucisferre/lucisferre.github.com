---
title: Bootstrap 3, Foundation 4 and Bourbon Neat
date: 2013-06-14 11:16:00 Z
categories:
- twitter-bootstrap
- foundaton
- bourbon
- bourbon-neat
- css
layout: post
comments: true
external-url: 
---

Update: [I've finally made my decision](/2013/08/05/bootstrap-foundation-and-bourbon-part-2)

Decisions... decisions...

I've built up some of my recent work on [WealthBar](http://wealthbar.com) using
Foundation 3 however Foundation 4 has been out a while now and I think it's
time for me to catch up on some upgrades. Specifically moving things over to
Rails 4 now that it's RC2. While I'm at it I may as well choose my CSS
frameworks for the long haul.

Let me start by saying working without a framework is not an option I'm
considering. If that's how you like to work that's great **for you**. Leave me
out of your masochistic fantasies though.

<!-- more -->

I'm specifically looking for the following key features:

* Common design elemens included (and ideally looks good). Buttons, forms,
  typography, etc.
* Semantic grid system using mixins, I've learned the hard way why grid classes
  are bad news. Especially after watching both Bootstrap and Foundation change
  their classes in the latest versions.
* Responsiveness (obviously)
* Flexible

The decision isn't all that easy partly because of the similarities between the
choices but also because each has some negative consequences that gives me
serious pause. Here are the pros/cons as I see them

## [Bootstrap](http://getbootstrap.com)

Pros:

* Pretty decent as a default look and feel and can be improved fairly easily.
* Widest set of styles and UI components, most of which are really useful.

Cons:

* Waiting. Version 3 seems to be taking a while to drop. Because it has
  significant breaking changes I feel that starting anything with 2.3 would be
  a mistake.
* Not sure if their new semantic grid will be better or worse than Bourbon
  Neat's or Foundation 4's.
* LESS, because I use Rails that is well... *less* attractive than SASS.

## [Foundation 4](http://foundation.zurb.com/)

Pros:

* It's released, and is a major overhaul like Bootstrap. A lot of lessons
  learned built in to this version.
* A well thought out semantic grid, I'm pretty sure BS3 is stealing some of
  their ideas.

Cons:

* Ugly--er I  mean "minimalist"--design. Honestly, I don't know why, but I find
  the default look kind of plain.
* Worst docs of the three, not the biggest deal in the world, but poor
  documentation always adds overhead to my work.
* They didn't merge in my vertical rhythm pull request. Boooo :\_(

## [Bourbon](http://bourbon.io/)/[Neat](http://neat.bourbon.io/)

Pros:

* Very small doesn't get in your way
* Provides Compass type mixins that help you do lost of common things in CSS.
* 100% semantic, if you like, no class based styles at all.

Cons:

* Not a real competitor to BS or Foundation as it doesn't have any UI
  components just mixins and a grid. No default typography either. Everything
  has to be built.
* The semantic grid has a different syntax for nested columns than top-level
  columns which the styling components a little less portable than Foundation
  4. Probably not a deal breaker though.

My gut right now tells me to use Foundation 4 *with* Bourbon (but not Neat).
Worst case, later on I could choose to drop all of Foundation 4 except it's
grid mixins.

What do you think? Feel free to let me know [on twitter](http://twitter.com/lucisferre).
