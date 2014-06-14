---
author: Chris Nicola
date: '2009-06-04 22:45:00'
layout: post
slug: persistence-laws-use-surrogate-keys
status: publish
title: 'Persistence Laws: Use Surrogate Keys'
comments: true
wordpress_id: '12'
categories:
- .NET
- Software Development
- nHibernate
---

As I learned nHibernate, several sources recommended that one _always_ use a surrogate primary key for all entities.  I follow this rule strictly. My personal preference now is to use the hilo identity generator nHibernate provides.

Today, I was working on updating values in a table from one of my first databases when the reason for this rule became very apparent.  All I am trying to do here is update the folder locations in a table of file references.  Here is a snippet of the code:

```csharp
// Create a new file link and delete the old one
// Have to do this cause I fail and was too stupid to use surrogate keys
// I made the Filename the primary key... I mean they're unique right???
var newfile = new Insurance_File
{
  Filename = fi.FullName.Replace(rootdir, newname),
  fk_idPolicy = file.fk_idPolicy,
  Policy = file.Policy
};
_datacontext.Insurance_Files.DeleteOnSubmit(file);
_datacontext.Insurance_Files.InsertOnSubmit(newfile);
```

The problem here is fairly apparent.  Acting on the suggestions I had read in a number of blogs and guides on building relational database I chose to make the clearly unique field of the Filename a primary key.  However, as a result I now have to delete each file entry and add a new one since one can't modify primary key values.

This is not a very efficient use of the database for this task.  It is however a good example of why you should always use surrogate keys. Oh, and yes, that is Linq2SQL I'm using not nHibernate. It is just a quick one-off utility I needed to update with and I find linq2sql to be really quick to do things like this sometimes.
