---
author: Chris Nicola
date: '2009-11-24 08:37:00'
layout: post
slug: fluent-nhibernate-cane28099t-quite-see-the-forest-for-the-trees
status: publish
title: Fluent nHibernate can't quite see the forest for the trees
comments: true
wordpress_id: '51'
categories:
- .NET
- nHibernate
---

![forestandtree][1]

One of the first issues I’ve run into building my blog engine, which I’m calling Graphite for now, was an issue mapping nested comments using fluent NH.  By default S#arp Architecture uses the fluent auto persistence model to auto map your entities to the database.  This has many advantages, the primary one being that you don’t need to write any mapping code or XML. 

<!--more-->

For those unfamiliar with either fluentNH or the auto persistence model you can read more on the [wiki][2].  It is extremely well documented.  One of the few problems with the auto mapping though, is the fact that it cannot map self-referencing parent-child relationships, such as you would find it a typical tree or hierarchical structure.  This would include the nested comments I want for my blog.  The error you get is something like this:

`NHibernate.MappingException: Repeated column in mapping for collection: Graphite.Core.Comment.Children column: CommentFk`

The mapping for such a structure is very simple, and I figured it could be done easily using [conventions][3] (a way of telling the auto persistence model how to map things) but alas I have so far been unsuccessful.  Fortunately, fluentNH has [overrides][4] which allow you to write a little bit of code to handle simple overrides.  Here is my override for my nested comments:

```csharp
public class CommentMap : IAutoMappingOverride<Comment>
{ 
  public void Override(AutoMapping<Comment> mapping) { 
    mapping.References<Comment>(x => x.Parent).Column("ParentFk"); 
    mapping.HasMany<Comment>(x => x.Children).Inverse().KeyColumn("ParentFk"); 
  } 
} 
```

My comments entity looks like this:

```csharp
public class Comment : EntityWithTypedId<Guid> { 
  public virtual string Author { get; set; } 
  public virtual string EmailAddress { get; set; } 
  public virtual string WebAddress { get; set; } 
  public virtual string Content { get; set; } 
  public virtual DateTime DatePosted { get; set; } 
  public virtual Post Post { get; set; } 
  public virtual Comment Parent { get; set; } 
  public virtual IList<Comment> Children { get; set; } 
} 
```

It would be nice if fluent NH understood this convention, i.e., if the property names are `Parent` and `Children` and they reference the entity class and a collection of that class respectively.  I am hoping to either find a solution to this using conventions or a patch to fluent that can address this seemingly simple requirement.  I do know from googling this problem that the fluent NH guys are aware of this limitation and I believe they are working on it, but it would be nice if I could lend a hand.

UPDATE: Turns out this is actually a problem with the way fluentNH understands this type of entity, it sees the collection on the child of the same type as the parent and assumes you are performing a many-to-many mapping.  Since this was many-to-one the override is necessary, but I have posted a question to the fluentNH google group to see if anyone has another way.

   [1]: /images/forestandtree1.jpg (forestandtree)
   [2]: http://wiki.fluentnhibernate.org/Auto_mapping
   [3]: http://wiki.fluentnhibernate.org/Auto_mapping#Conventions
   [4]: http://wiki.fluentnhibernate.org/Auto_mapping#Altering_entities

