---
title: Convention over configuration with fluent nHibernate and AutoPersistenceModel
date: 2009-12-23 13:29:00 Z
categories:
- ".NET"
- fluentnh
- nHibernate
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '58'
---

![xmlschool_1][1]

One of the things I get a fair bit of use out of is the fluent nHibernate project and the AutoPersistenceModel.  If you are not very familiar with fluentNH or AutoPersistenceModel then I would suggest checking out their [wiki][3], as what I am about to discuss, while not difficult, is some relatively advanced usage.

The purpose of AutoPersistenceModel is to automatically generate the nHibernate configuration (the HBM files if you are currently used to XML configuration) directly from your model based on Convention over Configuration, [you can hear James Kovacs discuss this concept on .NET Rocks][4].  With tools like SchemaExport and SchemaUpdate it is even possible to automatically generate and execute DDL scripts against your database to keep the schema in sync with your model.  This can be a bit rails like in it's use, in fact [Adam Dymitruk][5] is currently working on a utility to extend the SchemaUpdate to allow the creation of versioned database scripts like rails has (ok Adam it's official, now you actually _have to_ finish it!).

<!--more-->

The default AutoPersistenceModel conventions are kept quite simple, so often you will need to do a bit of customization.  One way to do this is through overrides which I showed how to use in [this post][6], were I implemented an override for a self-referencing tree relationship.  However, whenever possible, it is preferable to use fluentNH's conventions to do this.  Conventions can be used define custom behavior in very flexible ways.  Below I am going to show how this can be done to customize database constraints like _unique _and _index_.

S#arp Architecture provides a [[DomainSignature] attribute][7] which you can apply to your entity's properties.   The attribute is used to denote the set of properties that uniquely define the entity and is similar to the concept of a _business key_ often used in SQL database design.  It is important to point out that the _DomainSignature should_ _not be considered a primary key _and that you should _always use a surrogate primary key_ generated either by the database or nHibernate (hilo and guid.comb are the two I prefer).

The domain signature can ensures that entities can be compared using the properties decorated with [DomainSignature].  This is useful if say one object was loaded from the repository using nHibernate but another was constructed and I want to determine if I should treat them as the same entity.  It is also useful to determine if a new entity will violate a uniqueness constraint you want to enforce.

A good example of a useful DomainSignature would be the slug of a blog post.  A post also has an _id _for it's primary key_,_ in most cases a guid, but the slug also uniquely identifies a post as well and no two posts can have the same slug.  The only problem now is that when I generate my DDL I can see that nHibernate has no notion that it should enforce my uniqueness constraint.

What I want is for nHibernate and hence the database schema should be aware that DomainSignature implies a uniqueness constraint.  Fortunately, fluent nHibernate conventions make this is quite easy.  Fluent nHibernate defines an _AttributePropertyConvention<T>_ base class for exactly this purpose and we can extend it like this:

```csharp
public class DomainSignatureConvention : AttributePropertyConvention<DomainSignatureAttribute> { 
    protected override void Apply(DomainSignatureAttribute attribute, IPropertyInstance instance) { 
        instance.UniqueKey(instance.EntityType.Name + "DomainSignature");    
    } 
} 
```

The `DomainSignatureConvention` tells fluent that all properties that are decorated with `[DomainSignature]` should form a unique key.  Now if I then define my entity like this:

```csharp
public class Price : Entity { 
    [DomainSignature] 
    public virtual Security Security { get; set; } 
    [DomainSignature] 
    public virtual DateTime PriceDate { get; set; } 
    [DomainSignature] 
    public virtual bool CanadianPrice { get; set; } 
    public virtual decimal Bid { get; set; } 
    public virtual decimal Ask { get; set; } 
    public virtual decimal Close { get; set; } 
} 
```

then SchemaExport will generate DDL like this:
    
```csharp
create table Prices (
  Id INTEGER not null,
  PriceDate DATETIME,
  CanadianPrice INTEGER,
  Bid NUMERIC,
  Ask NUMERIC,
  Close NUMERIC,
  SecurityFk INTEGER,
  primary key (Id),
  unique (PriceDate, CanadianPrice)
)
```

Except we have a small problem here.  The foreign key for the many-to-one relationship on `Security` was not included in the `unique` constraint.  To be quite honest I am not exactly sure why but I am guessing that the `AttributePropertyConvention` does not work with a reference.  Instead I will need to add something to my `ReferenceConvention` which is provided by default with s#arp architecture.

```csharp
public class ReferenceConvention : IReferenceConvention { 
    public void Apply(FluentNHibernate.Conventions.Instances.IManyToOneInstance instance) { 
        instance.Column(instance.Property.Name + "Fk"); 
        if (Attribute.IsDefined(instance.Property, typeof (DomainSignatureAttribute))) 
            instance.UniqueKey("DomainSignature"); 
    } 
} 
```

I don't really like this as I don't think the attribute should be ignored, but it does at least change the above to `unique(PriceDate, CanadianPrice, SecurityFk)`.

There are obviously many other types of constraints we could use, you could create an attribute for defining indexing on certain properties like `[Indexable("IndexName")]` and create a convention like this:

```csharp
public class IndexableAttribute : Attribute { 
    private readonly string _name; 
    public IndexableAttribute(string name) { _name = name; } 
    public string GetName() { return _name; } 
} 
   
public class IndexableConvention : AttributePropertyConvention<IndexableAttribute> { 
    protected override void Apply(IndexableAttribute attribute, IPropertyInstance instance) { 
      instance.Index(attribute.GetName()); 
    } 
} 
```

Now I can create indicies by using the `[Indexable("Name")]` attribute.  Properties with the same "Name" will be part of the same index constraint and indexed together. 

I found it is actually a good idea to always index many-to-one relationships, something I briefly mentioned in a [previous post][8].  Now that I am using fluent nhibernate however I can't set the index property in the HBM files so I will need a convention for that.  It also makes sense to set this indexing on all many-to-one relationships so there is a better way to do this that does not involve the use of an attribute, instead we implement `IRefrenceConvention`.  S#arp architecture already includes this convention by default so we can just edit it:

```csharp
public class ReferenceConvention : IReferenceConvention { 
    public void Apply(FluentNHibernate.Conventions.Instances.IManyToOneInstance instance) { 
        instance.Column(instance.Property.Name + "Fk"); 
        if (Attribute.IsDefined(instance.Property, typeof (DomainSignatureAttribute))) 
            instance.UniqueKey("DomainSignature"); 
        else
            instance.Index(instance.Property.Name + "Index"); 
    } 
} 
```

Notice the if.else, if we have already defined `UniqueKey` defining `Index` would be redundant as unique key's are already indexed.  When defining indexes you will typically see something like like the following DDL output from SchemaExport:
    
```
create index SecurityIndex on Prices (SecurityFk)
```

It is pretty easy to customize the behavior of the fluent nhibernate AutoPersistenceModel using conventions and I find it can be very useful to have this type of fine grained control over your database schema generation.

   [1]: /images/xmlschool_1_thumb.jpg (xmlschool_1)
   [2]: /images/xmlschool_1.jpg
   [3]: http://wiki.fluentnhibernate.org/Main_Page
   [4]: http://jameskovacs.com/2009/08/25/net-rocks-475-james-kovacs-on-conventionoverconfiguration/
   [5]: http://adventuresinagile.blogspot.com/
   [6]: http://lucisferre.net/2009/11/24/fluent-nhibernate-cane28099t-quite-see-the-forest-for-the-trees/
   [7]: https://github.com/sharparchitecture/sharp-architecture/wiki
   [8]: http://lucisferre.net/2009/07/01/add-an-index-to-many-to-one-relationships-in-nhibernate/

