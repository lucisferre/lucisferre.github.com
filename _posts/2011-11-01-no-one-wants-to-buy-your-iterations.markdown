---
author: Chris Nicola
date: '2011-11-01 20:10:45'
layout: post
slug: no-one-wants-to-buy-your-iterations
status: publish
title: No one wants to buy your iterations
comments: true
wordpress_id: '694'
categories:
- agile
- kanban
- lean
- scrum
---

![][1]

One of the most common agile tools in the toolbox is the good old iteration.
Call them what you want, sprints, cycles, releases, but iterations while a
potentially valuable and useful exercise for teams new to agile, can
easily become dangerous to the quality of the software and the morale of the
team if not used in moderation.

Iterations are useful for a number of reasons, they force teams to break down
the product development, they encourage [small batches][3] and they help to
keep work manageable and time boxed. Honestly, what's _not_ to like about
iterations?

<!--more-->

## So what's wrong?

Unfortunately the reality of iterations is often miles from the idyllic picture
Agile paints. Teams and even entire businesses tend to get so caught up in the
process of iterating that they lose sight of the purpose of it. SCRUM is
certainly the worst offender with it's PASS/FAIL style and it's use of the term
"sprint".

The problem occurs when teams time box a bunch of work that is often hastily
planned and loosely estimated and mold it into what _feels_ like a good
iteration. If _all_ the planned work gets done and the results are good the
team gets a **pass**, if they are not they get a **fail**. Usually the product
owner or some other team authority figure decides on PASS/FAIL for the entire
team (I'm aware this is a bit of a straw man and not the textbook principle of
the SCRUM sprint but this is typical of how it actually ends up working).  

The results of following and repeating this process are predictable. Teams are
constantly pressured to improve or at least maintain their _velocity_. They
cram more than they can handle into the iteration, they rush to get everything
through, and they cover up or ignore the rough edges. When things don't go as
planned they "fail" and team moral is left battered and bruised.  

The long term result is decreasing quality from pushing through half baked
features, increasing technical debt from all the rushing, stress and low morale
and the situation piles on itself as iteration after iteration are performed.  

## The road to recovery

The path back out is to return to the reason things like iterations and
velocity were conceived in the first place. The core idea here is [_pacing_][4]
which is why I completely loathe the term "sprint" because it portrays the
exact opposite principle we want at play here. I don't think anyone needs to be
convinced of why rushing to get this type of work done is bad.

The importance of pace is so underrated in software. We often feel like "crunch
time" is still a valid concept. Some SCRUM team literally have rules like "no
one goes home if the work isn't finish for this iteration". Agile iterations
are not an instant fix and the reality is that continuing to do the wrong thing
faster and time boxed into iterations isn't going to improve anything.

Are features being hastily planned and there isn't enough clarity? Slow down
and make sure the team gets clarity. No time to learn TDD? Slow down and learn
to do TDD. The code is becoming total crap and needs to be fixed? Don't put
[duct tape][5] on it, slow the @#$% down, and learn how to rewrite it properly.
Need time to understanding new tools, design techniques, architectures to do
something right? Well, you get the idea.

I personally feel that [Kanban][6] provides a deeper level of guidance and
exposes the root of the fundamental problem here. Kanban focuses on maintaining
a continuous process rather than one that stops and starts in fits. It requires
that teams are honest about what is a realistic and sustainable pace for
building quality software. Finally, Kanban focuses on completing small batches
by driving the delivery of individual _releasable_ features and pushing them
through a queue with a minimum of waste and waiting while ensuring each batch
is still deliverable and of value to the business and the customer.

Let's face it, no one wants to buy your iterations so why tie releasing and
features to this completely superficial concept?

_This is the first of what I very optimistially hope will be 30 [#NaNoWriMo]
blog posts. I'll probably miss a few, but it's the thought that counts. I
probably won't be releasing them daily though, I'll pace them out a couple per
week._

   [1]: /images/128799520880143432-300x225.jpg (128799520880143432)
   [2]: /images/128799520880143432.jpg
   [3]: http://www.startuplessonslearned.com/2009/02/work-in-small-batches.html
   [4]: http://blogs.balsamiq.com/team/2011/09/07/pace/
   [5]: http://lucisferre.net/2009/09/28/the-technical-debt-of-red-green/
   [6]: http://en.wikipedia.org/wiki/Kanban

