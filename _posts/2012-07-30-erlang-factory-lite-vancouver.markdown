---
title: Erlang Factory Lite Vancouver
date: 2012-07-30 09:30:00 Z
categories:
- Erlang
- conference
- Vancouver
layout: post
comments: true
---

Last Saturday I spent my precious weekend enjoying the Erlang Factory Lite
Vancouver edition, a one-day Erlang extravaganza.

Now, since I don't know all that much about Erlang---I've only managed to
complete the 7 languages in 7 weeks section on it, which culminates in creating
Tic-Tac-Toe---I've kept a running blog of my notes throughout the talks, for
later reference and afterwards distilled it down into the summaries you see
below. So essentially I'm live blogging with a two-day time delay (a little
trick I learned from NBC).

So lets get started...

<!--more-->

## Session 1: Noob to Production
### [James Golick](jamesgolick.com) of BitLove

James from BitLove (and Fetlife) talked about starting from scratch with Erlang
and using it to build a concurrent chat service for Fetlife which at it's peak
can serve around 200k active conversation in browser tabs (I hope I got the
right).

One of the main takeaways for me was from hearing about how James ran into a
lot of trouble with the currently available OSS libraries for XMPP and even
MySQL. The bottom line is the Erlang's OSS ecosystem is still immature, which
means you're going to have to roll up your sleeves a bit and contribute.
Interestingly enough James was not the only speaker who chose to build a custom
messaging and presence library after finding ejabberd nearly impossible to work
with.

The second big takeaway is the Erlang is both fast, very fault tolerant and
very powerful for diagnosing problems when they do happen. James had a serious
session timeout bug due to the defaults on the MySQL driver creating only **one
connection** in the connection pool. Despite this problem the system continued
to failover a restart and for the most part functioned. The bottom line is that
Erlang's code reloading and supervision features are a pretty big deal.

## Session 2 - Mongo on Riak
### [Pavlo Baron](https://twitter.com/pavlobaron)

Pavlo's talk centered around building a [Mongo interface on top of Riak](https://github.com/pavlobaron/riak_mongo/). A bit of
this session focused on some of the MongoDB hate in the Erlang community. To be
fair Pavlo had a point after showing some of the CPP code in the Mongo codebase
which included magic numbers and some ridiculous commenting.

While I'm still not convinced MongoDB is nearly as bad as it's made out to
be---don't get me wrong it's miles away from perfect but it gets shit done and
is great for quick prototype work with documents---I did start looking to Riak
more seriously, long story short, it's very cool.

## Session 3 - Building Services with Webmachine
### [Kevin Smith](http://twitter.com/kevsmith) of Opscode

Opscode is the company that brings you [Chef](http://www.opscode.com/chef/).
Kevin gave us a basic rundown of Chef's architecture which was originally built
using Merb, Ruby, Unicorn and Nginx for the server API. On average this works
out to about 2.4GB per server running under Ruby which has motivated a shift to
Erlang and Webmachine.

[Webmachine](http://wiki.basho.com/Webmachine.html) is really very cool. It is
essentially a REST toolkit built on top of Erlang. It's based around defining
resources as modules and setting up their RESTful behavior within the module.
My only complaint was the lack of DRYness (a common problem with Erlang in
general). Kevin's solution to that was to create a kind of Erlang "mixin" which
allowed them to reuse REST behavior code across multiple resources.

As expected there are no real facilities to support HATEOAS, a concept I feel
is the unicorn of REST. Everyone thinks it's wonderful but no one has ever
actually seen it in the wild.

It also turns out there are other implementations of webmachine, including one
in Ruby. I think if you want to keep the benefits of concurrency but also keep
things "DRYer" I'd look at webmachine with Elixir (more on Elixir near the end)

##  Session 4 - Spawnfest: What I did on my Vacation
### [Mahesh P-Subramanya](https://twitter.com/dieswaytoofast)

Mahesh's talk was on participating in [Spawnfest](http://spawnfest.com/) while
also on a diving vacation with is wife. A comedy of errors that still resulted
in a solid entry in the form of [Erlymob](http://erlymob.com/) (currently down
for maintenance) a flash mob organizing tools.  The key learnings their
experience doing this and involving a complete Erlang noobie:

* OAuth Sucks
* Strings != Binaries
* 'if' is not 'if'
* Erlang syntax is easy but the semantics are hard
* Immutable variables -> instant karma

## Session 5 - Teaching Programming: Principles of Adult Learning
### [Casey Rosenthal](https://twitter.com/caseyrosenthal/)

Casey gave a great talk on how he teaches programming to adults (including
Erlang). He refers to the core technique as \*Games where \* is any language
being taught. It works as follows:

* Determine the relative experience of the participants
* Inverse pair people with experience with people who are new
* Provide a test problem or challenge
* Facilitate the solution of the problem
* Facilitate regular interruptions (every 25mins, sort of pomodoro style)

He also explained the basic principles behind adult learning, relevance,
consilience and reflection. Information must have a relevant foundation to be
retained, it can't introduce too much cognitive friction with existing
knowledge (consilience) and there has to be time for the learner to reflect on
what has been learned.

## Session 6 Erlang in Production
### [Geoff Cant](https://twitter.com/archaelus/)

Geoff talked a bit about the benefits of Erlang vs X in production
environments. This talk was a bit advanced for a noob like me, but did give me
a good idea of the debugging and tracing features of Erlang. 

The bottom line is Erlang is extremely powerful if you want to investigate
problems that are occurring live in production. 

## Session 7 - Erlang without OTP
### [Jay Nelson](http://twitter.com/duomark)

Erlang is typically used along with the "Open Telecommunications Platform" which provides a number of libraries and facilities that make things a bit easier. Jay is working on a different approach with his [ErlangSP](https://github.com/duomark/erlangsp) project and explained a little bit about how to build on Erlang without directly using OTP. Basically this was a whirlwind tour of what OTP does for Erlang.

Specifically, things like providing a framework for message pushing, async and
sync messaging, controlled code change and fault tolerance and
crash/termination handling. Jay explained this by showing what it would look
like to write OTP compatible processes without actually using OTP.

## Session 8 - The Erlang Microbrewery
### [Omar Yasin](https://twitter.com/omark://twitter.com/omarkj)

Ok so this talk was not actually called that, but since I can't remember what
it was called and since Omar's company [Kodi](http://kodiak.is/) is both a
software shop and a fledgling microbrewery that's the name I'm going with.

Omar talked about starting a financial software company after the Icelandic
banking collapse and his team's switch from .NET (the primary programming
framework in Iceland at the time) to Erlang in creating their solution. All in
all an interesting solution. As someone who has some experience moving away
with .NET I'm always interested in hearing other peoples take on it from
different angles.

## Session 9 - Elixir
### [Yurii Rashkovskii](https://twitter.com/yrashk) of [Spawngrid](http://spawngrid.com)

Yurii presented on [Elixir](http://elixir-lang.org/), which is essentially a
meta-language on top of Erlang that appears to be inspired by Smalltalk/Ruby
and Clojure. This was really interesting. 

Erlang is a powerful tool and an interesting functional language, but it is
also pretty old and lacks meta-programming facilities common to many of the
more popular modern languages like Ruby. Elixir, currently being maintained by
[Jose Valim](https://twitter.com/josevalim) of the Rails core team seeks to
address this.

I think that just like Coffeescript, it isn't practical to use Elixir without
first having a solid understanding of Erlang, but if you are planning on
getting into doing some Erlang I would highly recommend checking out Elixir as
well.

## Summary

Finally, [Tavis](https://twitter.com/tavisrudd) closed the conference with
another awesome demonstration of coding by voice. I felt that this was a great
way to start getting more familiarity with Erlang which has definitely been
getting more and more attention from the web community as a whole.

As a polyglot, I think Erlang is definitely something you want to have at your
disposal. There are definitely certain classes of problems it appears to solve
extremely well. As a Lean/Post-Agile person who cares about continuous
integration and deployment, verifiability and testability of systems and code,
Erlang is first-class as well, supporting things like hot code reloading and
live tracing and debugging of production. 

So thanks Yurii and Spawngrid and all of the EFL sponsors. I'm confident I'll
be around next time EFL comes to town.
