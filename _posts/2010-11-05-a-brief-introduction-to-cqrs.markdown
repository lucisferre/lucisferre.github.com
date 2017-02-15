---
title: A brief introduction to CQRS
date: 2010-11-05 05:49:00 Z
categories:
- CQRS
- DDD
- event sourcing
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '221'
---

I had the opportunity recently to speak on CQRS at both Vancouver TechDays and Agile Vancouver and I felt I should put up a summary of the talk on my blog, so here it is.  This is just intended as an introduction but I’ve included a number of useful links at the end where you can get more info.  I’d like to do some “deep dive” posts in the future, but I’m not promising anything. 

### What is CQRS?

Command and query responsibility segregation is more of a set of architecture concepts then a single architecture. It is often associated with [enterprise message bus][1] architectures, [event sourcing][2], [NOSQL][3] databases and [domain events][4]. The subject itself originated from [discussions][5] around how to apply domain driven design principles, specifically how to apply it when building distributed systems capable of long term scalability. Often the term ‘distributed domain driven design’ is used to refer to the patterns and principles encompassed by CQRS. There are a lot of concepts that fit under the umbrella of CQRS but the guiding principle of CQRS is simple: Separate the handling of commands issued from the UI/user from the handling of queries. Command: An imperative action taken by the user which may mutate the state of the system. It may not return data but can only succeed or fail (with a reason given). Commands should express business concepts as tasks the user can perform and have a clear purpose. Query: A simple request for data that should answer a question that is of value to the business. It may not mutate state in any way (this ensures we have no side effects). Query will return a DTO which is in turn mapped to a view presented to the user. We can compare CQRS to the traditional N-tier architecture that is probably very familiar and a common pattern for most client-server systems in the diagram below: 

<!--more-->

![clip_image002][6]

In this example a single remote façade is used to abstract our service layer which in-turn encapsulates the domain-layer on top of a data-access layer (possibly via an ORM) and depending on the architecture a business-logic layer (in such systems it is not uncommon to place the business logic in a layer separate from the domain entities which is entirely contrary to the patterns and principles of DDD). Usually the façade returns some sort of DTO from queries made by the UI/user. The user then can modify the data presented from these DTOs and when they save it a modified DTO is returned back and the appropriate changes are made to the database. There are a number of problems presented by this type of architecture: 

  * Typically results in a database driven rather than model/domain driven design which often confuses business concepts and results in an inflexible system.
  * The UI also tends to be data-centric, the users is usually presented with tables to edit in a tab-type-tab-type fashion. It is error prone and provides a very limited user experience.
  * Business logic and concepts become scattered and are found in every layer, domain, BLL, DAL, stored-procs and even the UI itself.
  * The system is tightly coupled to the schema and vice-versa, so it becomes difficult to change either when business needs change.

The CQRS design patterns address these problems and it may also be possible to carefully evolve N-tier designs towards CQRS design in phases. In CQRS we separate our services into a Command Service and a Query service. 

![clip_image004][8]

The Command Service differs significantly from the traditional model in that we now have a task based UI design that is geared towards what the user is actually trying to accomplish. Commands are handled by the service and we can now apply them to our domain to make changes to the business objects state. Events are the result of changes in business state. Just as commands are issued as a statement of _what we want to do_ events represent _what happened._ These events are often incredibly important as business data so it is often useful to write these to an event store, but it is not necessary. What is important is that they are the direct result of actions applied to domain objects which themselves are the only objects that encapsulate the business logic. 

![clip_image006][10]

In the simplest terms the Query Service is a remote façade for requesting DTOs. The exact details of how the service is implemented are not critically important, what is important is that it should, ideally, be optimized for performing queries. This seems like a good place to use SQL and stored-procedures. We can even consider the quite drastic sounding option having a completely de-normalized data model to support the queries. However, the first question we ask ourselves is how will we ensure data is consistent without a normalized data model? This is where the domain events come in. The query service has handlers which determine which tables in the database to update and how to update them. A single event could easily have multiple handlers to update multiple tables to enable different reports. Even more interestingly is that if we store our events we could define a new table and its handler(s) and then populate it by re-processing the event history. So why bother with all this? What does this accomplish for the business? In terms of representing business concepts more fluently in our software we can gain quite a bit: 

  * A more intuitive design, by focusing on task driven, rather than data-driven UI logic the user is directed to take clear and meaningful actions rather than to just edit the data
  * An encapsulated domain containing all our business logic which cannot simply be circumvented with lazy programming
  * A clear flow of information from command to query all handled by simple but effective handlers
  * Compatibility with messaging architecture (command and events as messages) and the ability to be distributed
  * Asynchronous behaviour and the ability to leverage eventual consistency for greater partitioning and future scalability
  * An auditable history of everything that happened
  * Reversibility: The ability to change our minds, change the database schema, build new query tables and reports, new events, etc.

Out of all of the above I feel that reversibility is the most important. Systems that are designed to handle and represent core business concepts typically deal with the most important and valuable aspects of a business. However, the exact details of these concepts are not fixed and they will change as the business conditions change. Having reversibility allows the business concepts within our system to change with them. I hope that gives a reasonable overview of what CQRS is and does, in a future post I plan to elaborate more on domain driven design and how domain events and the event store work using examples from the Incentives 2.0 project. Useful Resources: 

  * Greg Young’s posts on DDDD: [http://codebetter.com/blogs/gregyoung/archive/tags/DDDD/default.aspx][12]
  * Udi Dahan’s (author of nServiceBus) views on CQRS: 
    * [http://www.udidahan.com/2009/12/09/clarified-cqrs/][13]
    * [http://www.udidahan.com/2010/05/07/cqrs-isnt-the-answer-its-just-one-of-the-questions/][14]
  * [CQRS][15] Info site: [http://cqrs.wordpress.com/][16]
  * nCQRS a framework for .NET: [http://ncqrs.org/][17]

   [1]: http://www.eaipatterns.com/
   [2]: http://martinfowler.com/eaaDev/EventSourcing.html
   [3]: http://en.wikipedia.org/wiki/NoSQL
   [4]: http://www.martinfowler.com/eaaDev/DomainEvent.html
   [5]: http://codebetter.com/blogs/gregyoung/archive/2009/08/13/command-query-separation.aspx
   [6]: /images/clip_image002_thumb.png (clip_image002)
   [7]: /images/clip_image002.png
   [8]: /images/clip_image004_thumb.png (clip_image004)
   [9]: /images/clip_image004.png
   [10]: /images/clip_image006_thumb.png (clip_image006)
   [11]: /images/clip_image006.png
   [12]: http://codebetter.com/gregyoung/category/dddd/
   [13]: http://www.udidahan.com/2009/12/09/clarified-cqrs/
   [14]: http://www.udidahan.com/2010/05/07/cqrs-isnt-the-answer-its-just-one-of-the-questions/
   [15]: http://www.udidahan.com/2010/05/07/cqrs-isnt-the-answer-its-just-one-of-the-questions/CQRS
   [16]: http://cqrs.wordpress.com/
   [17]: http://ncqrs.org/

