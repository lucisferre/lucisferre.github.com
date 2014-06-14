---
author: Chris Nicola
date: '2009-10-15 09:55:36'
layout: post
slug: ab-using-statements
status: publish
title: ab-’using’ statements
comments: true
wordpress_id: '45'
categories:
- .NET
- C#
- fail
---

I was trying to squash a frustrating bug the other day that I just couldn't pin down despite the fact that it was right in front of my face the whole time.
    
```csharp
private ISession _session;

public void Consume(TransactionMessage msg) {
  using (_session = _sessionFactory.OpenSession())
  using (ITransaction tx = _session.BeginTransaction())
  {
    IMessage result = null;
    try {
      result = ProcessTransaction(msg, _session);
      tx.Commit();
    } catch (Exception ex) {
      result = new TransactionError(msg) {ErrorMessage = ex.Message};
      tx.Rollback();
    } finally {
      _bus.Consume(result);
    }
  }
}
```

I am sure C# veterans will see this novice error right away, but in case you didn't, I have combined a using statement with a field.  The problem here is that while I intended for the using statement to automatically dispose of my session variable it can't because the instance still holds a reference to it via the field.  I don't know why it took me so long to see it, it is obvious once you know what the problem is.

Long story short, don't combine using with fields or properties or anything that is going to hold a reference to what you intended the using statement to dispose of, it wasn't meant for that.
