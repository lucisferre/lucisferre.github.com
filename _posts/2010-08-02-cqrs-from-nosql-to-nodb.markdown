---
author: Chris Nicola
date: '2010-08-02 10:41:42'
layout: post
slug: cqrs-from-nosql-to-nodb
status: publish
title: 'CQRS: From NoSQL to NoDB'
comments: true
wordpress_id: '83'
categories:
- .NET
- CQRS
- nCQRS
- NOSQL
---

![ncqrs-logo][1] 

This weekend I spent most of my Sunday working on a filesystem based provider for nCQRS, something largely inspired by some work @adymitruk did recently.  I've done this for a few reasons:

  * To provide a quick and simple way for people to get up and running with NCQRS and test locally 
  * To allow developers minimize their reliance on third-party DB providers 
  * Make nCQRS more web-hosting friendly (you won't find many providers supporting .NET and MongoDB) 
  * Make a point, that using CQRS is not nearly as complex an architecture as [some out there would have you believe][2]. 

<!--more-->

The core idea behind [NoSQL][3] is not to facilitate the CQRS pattern (though they often go hand in hand), rather it's to recognize that a SQL database is not always the right tool for the job.  Similarly, there are likely to be scenarios where using _anything _more than some data files is overkill, or merely inconvenient and that is the idea behind the NoDB provider.  Just store the events in text files, of course the query-side data can still go wherever you want it to.

_(Note that I'm using the term 'database' here to refer to sophisticated database providers, arguably even file-based data storage is a form of database.)_

The design of NoDB is extremely simple.  Each aggregate root gets it's own individual file for event history.  Files are divided into folders and filenames based on their Guid for example:
    
    /a1/8156bf-fa5a-422e-91da-2a6b30e1fc6c

The first line of the file tells us how many events there are in the store in a fixed length long, this is updated every time we append a new event.  Events are appended one line at a a time, so we have one line for each event.

The provider also re-uses the existing support in the nCQRS framework that allow us to serialize/deserialize the event data to JSON.  An event file is pretty much human readable and looks like this:

There is some very basic support for multi-threaded read/write access.   Multiple readers can read the event store, but only one writer at a time can write.  @SzymonPobiega pointed out to me that this is similar to [Ayende's append only data store][4] except that in NoDB readers must wait for an active writer.

While this seems like a disadvantage it actually isn't.  In CQRS the event store is for the command side only, we don't use it to query data ever.  So the only reason we would read the data store would be to begin executing a command against an aggregate root.  If we don't wait for the writer we will have a stale version of the aggregate and this will guarantee a concurrency exception when we try to execute the command against the stale aggregate root.

Typically, you will want to have command processed via a queue anyways so threading concurrency issues like this will be abstracted away behind the queue.

I welcome people to take a look at [my fork on GitHub][5] and checkout the code (currently it is under the NoDB branch).  I would love to hear about any comments and criticisms.  In the meantime I am going to keep working away, I plan to try to add snapshot support today.

   [1]: /images/ncqrs-logo.png (ncqrs-logo)
   [2]: http://blog.robustsoftware.co.uk/2009/12/cqrs-crack-for-architecture-addicts.html
   [3]: http://en.wikipedia.org/wiki/NoSQL
   [4]: http://ayende.com/Blog/archive/2010/06/18/building-data-stores-ndash-append-only.aspx
   [5]: http://github.com/lucisferre/ncqrs

