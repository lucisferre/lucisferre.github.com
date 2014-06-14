---
author: Chris Nicola
date: '2011-11-07 09:00:23'
layout: post
slug: youre-having-nosql-problems-i-feel-bad-for-you-son
status: publish
title: You're having noSQL problems, I feel bad for you son
comments: true
wordpress_id: '729'
categories:
- CQRS
- rant
- MongoDB
- NOSQL
- SQL
---

Just recently on [Hacker News][1] a whole bunch of posts either deriding or
defending Mongo (or in some cases noSQL as a whole) appeared. It all started
with [this][2]. The problems with this little tirade against Mongo are multiple
but the foremost is that it was posted entirely anonymously, and I can't really
think of a reason why someone would feel the need to post something like that
anonymously other than to hide from ridicule.

The CTO of 10gen [responded][3] in the comments and pointed out that this
scenario seems highly unlikely at worst and at best is seriously hyperbolised.

<!--more-->

The list of posts in the fallout of this totally anonymous and unverified
"MongoGate" go on and on, here's the ones I could find quickly:

  * [Mongo's fine][4]
  * [Why the hate?][5]
  * [Failing with Mongo][6]
  * [Mongo 2.0 should have been 1.0][7]
  * [Let's have a serious noSQL discussion][8]
  * [and of course, the blatant trolling...][2]

Some people have rushed to the defence of noSQL, saying things like, "RTFM" and
"no SQL involves trade-offs that need to be understood." In fairness, I'll
admit these are little comfort to people complaining of noSQL's lack of ACID.
However I'm going to avoid the whole technological debate in favor of some
generalizing and hand waving.

First, no technology is a panacea, and I'm still confused why as a community we
feel the need to construct strawmen out of anything that becomes remotely
popular in order to tear it down for not being one. It's childish, it's stupid
and anyone capable of rational thought can see right through it.

Second, technology wars on the whole are just stupid. Technologies don't often
truly compete directly against each other, instead they simply shift the
landscape causing (some) people to make their decisions differently. I'm going
to make up some number on the spot here, but I'd wager a guess that that vast
majority, so 80%, of people involved in one side or another of a technology war
haven't actually used the technology they argue against. Doubly true (try not
to do the math here) for those on the side of whichever is the older
technology. Let's face it there aren't many developers out there who haven't
used SQL in the past.

Bottom line, the signal to noise ratio of technology flame wars is so low as to
be completely worthless as a discussion. Don't pit SQL against noSQL, or even
MySQL against MongoDB. Tell me why you've used what and what is amazing about
it. As an example I had lunch with someone from Heroku who works on their
Postgres team who, while starting the conversation with "don't use MongoDB",
went on to tell me about some seriously awesome new features of Postgres. I
honestly had no idea that while I'm having a grand time playing with noSQL db's
some SQL db's are getting pretty cool and modern too.

That said, I'll be sticking with MongoDB for what I'm doing right now. It's
going to take some more serious discussion than that of a simple flame war to
deter me from using it. It's simple, fast, fits with mental models of querying
and reading data for web sites easily. I still find the ability to easily
create embedded relationship and tree structures within a document useful a lot
of the time. It also suits the design of aggregate roots and fits with CQRS
concepts like keeping "read" models separate from "write" models (an approach
which addresses many of the issues people from the SQL camp constantly harp on
against noSQL).

Bottom line, if there is something fundamentally flawed with MongoDB, I have
yet to see it but I'll admit to not having had to deal with any serious scaling
scenarios. Most of the complaints I've seen have come from environments dealing
with very large volumes of data and/or high transaction rates. These are
scenarios where plain-old-SQL certainly has it's problems too, the reality is
scaling is still a hard problem. I'll argue that this is almost always CQRS
territory for many, many reasons I'm not going to go into in this post. Bottom
line, if you haven't begun having the discussion about separating writes from
reads, storing and publishing domain events to decouple the architectures, and
issues like eventual consistency, then no offense, but you likely aren't
qualified to talk about the choice of persistence technology and it's effect on
the scalability.

It's all too easy to blame the tools instead of the architecture and
infrastructure design. Obviously, CQRS by itself isn't a panacea either, but it
does provides patterns and a language around creating scalable domains that has
the added benefit of being completely technology agnostic. It applies just as
easily to a SQL only as it does to a noSQL, or hybrid environment. Though my
gut tells me there are distinct advantages to having noSQL in the mix here, and
that it makes more sense once we've moved beyond requiring the database to be
only safety net.

So if you're having noSQL problems, I feel bad for you son. I've got 99
problems but maintaining a schema ain't one.

   [1]: http://news.ycombinator.com
   [2]: http://news.ycombinator.com/item?id=3204510
   [3]: http://news.ycombinator.com/item?id=3202959
   [4]: http://blog.slyphon.com/post/12435929063/go-cry-on-somebody-elses-shoulder-mongodb-is-fine
   [5]: http://yourstartupsucks.com/post/12416816599/why-the-mongodb-hate
   [6]: http://blog.schmichael.com/2011/11/05/failing-with-mongodb/
   [7]: http://luigimontanez.com/2011/mongodb-2.0-should-have-been-1.0/
   [8]: http://blog.erlang.de/mongogate-or-lets-have-a-serious-nosql-discus

