---
title: Tell Don’t Ask
date: 2010-11-28 11:56:00 Z
categories:
- Agile Vancouver
- CQRS
- DDD
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '247'
---

During Michael Feathers talk on functional programming at Agile Vancouver 2010, he mentioned the OOP principle of “_tell don’t ask_”.  Since then, I’ve been thinking about it in the context of CQRS. [Tell don’t ask][1] is a principle of OOP proposed by Alec Sharp (someone correct me if my attribution is wrong here), who states:

> Procedural code gets information then makes decisions. Object-oriented code tells objects to do things.

So why do I think this is an important principle?  Because when we design OO systems that allow our objects to both tell _and_ ask, we start to build a spider’s web of dependencies in the relationships between our objects, and more often than not we end up with lots of unintended side effects in our methods (for example query methods that can change state).

<!--more-->

In a system that adheres to TDA, we can easily see and understand how each object can pass on responsibilities to the next.  The model of communication is clear and straightforward.  Looking at the model of a CQRS system we can see the following:

  * Commands tell the aggregate root **what to do** through it’s behaviours 
  * Aggregate roots tells the event bus and event store **what happened** through their domain events 
  * The event bus communicates tells relevant subscribed systems **what happened** by sending these events as messages 
  * Event handlers tell the read model what to create, update or delete via stored procedures or some other data logic layer 
  * Event handlers may also tell external systems, or extensions to our system what to do in response to the domain events (e-mail notifications, twitter updates, cross domain-boundary communication) 
  * The UI tells the query layer what it wants to know through query requests 
  * The query layer tells the UI, the state of the system by passing DTOs. 

[![cqrs-whole-system][2]][3]

The query layer is the only place we really have a two way communication and you could argue that we are ‘asking’ here, which is conceptually true, but it is, in fact, modeled as an exchange of messages (typically in an asynchronous fashion), which we could argue is just telling the system what we want to know, and then being told something in response.

What is compelling to me about the TDA aspect of CQRS is how it helps us to manage a system’s complexity.  Some level of complexity is inevitable whenever modeling business processes, but one of the key of OO combined with the patterns of DDD, is managing that complexity.  The TDA approach as applied through CQRS helps to make that job far easier.

Through CQRS we can keep the responsibilities of each link in the information chain small and minimize each pieces dependencies.  Small independent bits of logic, some as simple as handling off responsibility between two systems (e.g., the event bus) flow clearly from one object to another.  You will find other OO principles like SRP are extremely hard to violate within this model.

Moreover, the clarity and logical flow can be a big boon for teams.  The team can easily create a shared understanding of the system.  They can quickly separate responsibilities for a feature amongst multiple people separating into, UI/UX, command handling, domain logic, event handlers, database logic, queries, etc.  Each only needs to ensure they communicate with the next according to the patterns that have already been defined.

More importantly there is not benefit in circumventing this design.  On more than one occasion I have seen or heard of instances where a carefully designed ORM has been circumvented by a junior (or “senior”) dev who couldn’t be bothered to understand it’s function, with stored procedures, or even raw SQL being used to updated the data model in the database.  When this happens it usually signals the beginning of the end for an n-tier system and you can kiss your sweet ORM design goodbye.

Anyways, I’m starting to ramble so I’m not going to go on, just wanted to get these thoughts out in writing and perhaps see what other people think about this, even if you haven’t used CQRS directly in a project, can you see areas in your own code that either follow or violate TDA? Does it affect the quality, complexity and even testability of the code?

I should also point out nothing I’m talking about here is terribly original, I’ve stolen liberally ideas from [Greg Young][4], [Udi Dahan][5] and others from the [CQRS and DDD mailing list][6].  If anything you’ve read here seems interesting, I would highly recommend joining the discussions there.

   [1]: http://pragprog.com/articles/tell-dont-ask
   [2]: /images/cqrs-whole-system_thumb.png (cqrs-whole-system)
   [3]: /images/cqrs-whole-system.png
   [4]: http://codebetter.com/gregyoung/
   [5]: http://www.udidahan.com/
   [6]: http://groups.google.com/group/dddcqrs

