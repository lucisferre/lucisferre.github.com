---
title: Behavior Driven, Test Driven, Domain Driven Design
date: 2011-02-05 16:54:52 Z
categories:
- BDD
- CQRS
- DDD
- nCQRS
- TDD
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '293'
---

Ah, the joys of xDDs, you can never use too many, right? One of the many
benefits of using event sourcing with CQRS is how well it facilitates using BDD
style testing to drive our domain model design.  [Behavior driven development][1] 
encourages writing tests which expose the use cases or scenarios required by
the users and stakeholders.  This is closely in line with the goals of domain
driven design, which encourages us to model the domain abstractions in our code
on a deep understanding of the business models and processes.  BDD style tests
are therefore quite well suited to the testing our domain model behaviors.

<!--more-->

After seeing Greg Young’s demonstrate how to write BDD style tests for CQRS in
[this video][2]—-a word of warning though, it’s over 6 hours and I’m not quite
sure where the testing part started—-I started working on applying it to my
own domain model which uses nCQRS. I also switched from nUnit to MSpec as my
preferred tool for testing, partly because it has better syntax and support for
BDD. Upon doing so I needed to create a simple base test context class to
facilitate the testing. After a couple of refactorings I’m very satisfied with
the result and I wanted to share it. 

I also think seeing these how these tests work goes a long way towards
understanding how [CQRS/event sourcing][3] systems work (testing is believing).
The generic pattern typically followed when applying BDD to an event sourced
CQRS model is as follows: 

> **Given **a history of events (the current state of the domain model)
> **When** this command is executed (the behavior we want to test) **Then** the
> following events should have occurred (the expectations we have for this
> test)

I think it’s worth noting that I’m not necessarily saying that Give/When/Then
is the only right way to approach BDD, and I’m aware there is some contention
around conforming to this style.  However, in this scenario they fit and so it
certainly makes sense to use them to describe the tests. However, since I’m
going to use MSpec for my tests, which doesn’t support this style of writing or
reporting the test cases, I won’t be conforming to it either and I don’t really
think it’s of any importance either.  All that is really important is how these
tests drive our design and expose our use cases. MSpec provides the following
conventions for setting up our tests: 

> `Establish` is used to provide the context for a test, this is where I set up
> the historic events and the domain state. `Because` is used to execute the
> action under test, in this case we are executing a command for the domain
> which will be handled by our command handler. `It` is the way we define our
> expectations of what _should have happened_. I’ve created a custom `Should`
> extension method to test the event results.

Here is an example of a test I wrote in this style: 

{% gist 812952 %}

Pretty clear and concise if I do say so myself (and I do). The base class for these tests provides it’s own base `Establish` context which is run first and sets up the scaffolding for nCQRS to execute for the test.  It’s generic parameters define the `ICommand` type and `ICommandExecutor<TCommand>` that the test will be based on.  

Finally we hook into the events using nCQRS’ built in delegate handler. Here's the Gist: 

{% gist 812958 %}

This base class even supports command excution involving multiple aggregate
roots. When we call `Given()` an aggregate root is created, initialized from the
event history and any new events are then tracked via the delegate call. It’s
also added to the mock UoW class used by the command handler so the handler
will be able to load it. It is extremely easy to write consistent, repeatable
BDD style tests using this pattern, and because we verify each event and ensure
no other events occurred we are testing not only what has happened but also
what _hasn’t happened_, something that is typically very hard to verify.

   [1]: http://en.wikipedia.org/wiki/Behavior_Driven_Development
   [2]: http://cqrsinfo.com/video/
   [3]: http://lucisferre.net/2010/11/05/a-brief-introduction-to-cqrs/

