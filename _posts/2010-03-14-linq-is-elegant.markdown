---
author: Chris Nicola
date: '2010-03-14 10:35:00'
layout: post
slug: linq-is-elegant
status: publish
title: LINQ is elegant
comments: true
wordpress_id: '70'
categories:
- .NET
- Software Development
- csharp
- linq
---

Sometimes I just love LINQ.  I had to traverse an object tree today and I wanted something simple to enumerate every object in the tree into a dictionary.  Here is the recursive function I wrote:

```csharp
public IEnumerable<TestDataItem> GetAllSubItems() {   
  return Items.Aggregate(Items.AsEnumerable(),(list, item) => list.Union(item.GetAllSubItems()));   
}
```

Calling that on any item in the tree will return all of that items sub items.  Then it is a simple matter of calling ToDictionary() on the return value.
