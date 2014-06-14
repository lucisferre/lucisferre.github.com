---
author: Chris Nicola
date: '2009-05-19 19:42:00'
layout: post
slug: nhibernate-linq-and-queries
status: publish
title: nHibernate, Linq and Queries
comments: true
wordpress_id: '5'
categories:
- .NET
- nHibernate
---

![NHLogoSmall][1]

I have been using nHibernate a lot in my latest project. nHibernate is a persistence layer, it handles mapping of entities (objects) to a database or other storage medium. In this case a SQLServer database. I am not going to go into a big introduction to nHibernate here as there are many excellent resources for that available ([http://blogs.hibernatingrhinos.com/][3] for examples).

Learning nHibernate initially wasn't very hard, in fact it really couldn't be much simpler. Mastering nHibernate, and specifically the art of persistence and [domain-driven design][4] has been more difficult.  I have relied on several online blogs, one of them being [Ayende Rahien's blog][5].One specific problem has been exactly how best to separate the program into layers, often called [separation of concerns][6].  This involves separating out things like the Presentation/UI layer, the domain/model or sometimes "business" layer and the data access layer which typically handles persistence.  A common sample of building an application this way using nHibernate is [this sample ][7]of "best practices" which is often used by people new to nHibernate as a starting.  This was also what I used to learn with as well.

<!--more--> 

Despite being very easy to understand an implement I I find this design lacking in ways I find hard to describe, needless to say I find it clunky in some respects which is why I was really interested by [this post][8] by Ayende.  Ayende has argued a few times (and I am sure I will explain this poorly) that nHibernate really _is_ the data layer and that building an entire data layer on top of nHibernate is overkill.

Anyways, I didn't want to refactor my entire program just yet to eliminate my DAL so instead, to try out this `IQueryable` approach I added a simple method to the `AbstractNHibernateDao<T>` class:

```csharp
public virtual IQueryable<T> GetQuery()
{
  return (from entity in NHibernateSession.Linq<T>()
    select entity);
}
```

This is basically a method using generics which returns a generic query from Linq2Nhibernate.  It is very similar to Ayende's example, but it doesn't have any query logic.  That is ok though since whatever asks for the query can easily extend the query using `.Where()`.

I am curious what people think about this approach.  Is it sensible, or can you foresee pitfalls to it?

   [1]: /images/NHLogoSmall_thumb.gif
   [3]: http://blogs.hibernatingrhinos.com/
   [4]: http://domaindrivendesign.org/
   [5]: http://ayende.com/Blog
   [6]: http://en.wikipedia.org/wiki/Separation_of_concerns
   [7]: http://www.codeproject.com/KB/architecture/NHibernateBestPractices.aspx
   [8]: http://ayende.com/blog/3958/the-dal-should-go-all-the-way-to-ui

