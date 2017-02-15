---
title: An nHibernate lazy loading gotcha
date: 2009-11-13 19:16:00 Z
categories:
- ".NET"
- nHibernate
- ReSharper
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '47'
---

I'm sure something about this has almost definitely been written about before, in fact I vaguely recall reading about it, but I feel the same way about blog posts as Homer Simpson does about car horns "When you're angry you can never find one," so I figure one more can't hurt.  Besides I haven't blogged about anything in weeks and I'm not prepared to write about anything more complicated than this at the moment.

If you've worked with nHibernate for a while you probably already know the common patter of having your entities inherit from an abstract base class that provides a standard "Id" property and any other properties common to your entities.  In various examples across the web you will typically also find that `Equals()` has been overridden, you may have not given it a second thought, but I am here to tell you there is a very good reason for this and it is lazy loading.

<!--more-->

One of the things you get "out-of-the-box" with nHibernate is lazy loading.  This means if I have two entities like `Blog` that is related to `Post`, I can load a blog without loading it's posts right away.  Then when I access `Blog.Posts` say by calling `Blog.Posts.Count()`, nHibernate will load them for me at that time (that is assuming I have not already closed my session, in which case it will throw an exception, but that is another post).  Conversely, if I have queried for a post, I will get it's blog only if I access `Post.Blog`.

You can of course turn off lazy loading if this behaviour isn't desired, but I highly recommend learning to love it, it will lead you to much better and smarter ORM design.  Now on to the "gotcha".

This lazy loading is not achieved by manipulating the force, but through the use of a dynamic proxy.  Without getting into detail on dynamic proxies (since I'm sure I would start to show my ignorance on the subject pretty quickly) lets just say it provides a wrapper that dynamically inherits from your entity class while providing additional functionality at run-time, such as the ability to load itself from the database when a property is accessed.

One of the things that is preferable is to ensure that when two objects refer to the same entity in the database that calling Equals() will confirm this.  ReSharper provides a convenient facility to generate some default Equality members that return true when specific properties are equals and if we use the "Id" property the result is this:
    
```csharp
public bool Equals(Entity other) {
  if (ReferenceEquals(null, other)) return false;
  if (ReferenceEquals(this, other)) return true;
  return other.Id == Id;
}
 
public override bool Equals(object obj) {
  if (ReferenceEquals(null, obj)) return false;
  if (ReferenceEquals(this, obj)) return true;
  if (obj.GetType() != typeof (Entity)) return false;
  return Equals((Entity) obj);
}
 
public override int GetHashCode() {
  return Id;
}
```

You can even generate operators for == and != if needed, yet another reason why it is such a good idea to have ReSharper. The problem is this will fail if you ever compare an entity to a proxy returned by the nHibernate lazy loading. This right here is the reason:

```csharp
if (obj.GetType() != typeof (Entity)) return false
```

This is easily fixed though, doing a simple safe cast before checking makes this code both more readable and deals with the equality issue when using lazy loading:
    
```csharp
public bool Equals(Entity other) {
  if (ReferenceEquals(null, other)) return false;
  if (ReferenceEquals(this, other)) return true;
  return other.Id == Id;
}

public override bool Equals(object obj) {
  return Equals(obj as Entity);
}
```

I think that is much better, I would almost consider recommending this be changed in ReSharper, perhaps there is still time before ReSharper 5.0 is finished.
