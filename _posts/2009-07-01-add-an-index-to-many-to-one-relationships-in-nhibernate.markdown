---
title: Add an index to many-to-one relationships in nHibernate
date: 2009-07-01 00:50:00 Z
categories:
- ".NET"
- nHibernate
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '23'
---

I spent some time today creating some de-normalized views of my database for reporting. It isn't a big project so I've chosen to use SQL Views and SSRS reports for some of the reporting that is required. The main entities I am querying form a three level hierarchy and when I query the leaf nodes I often want to filter on data at the root node.

Well, for some reason, these views were unbelievably slow to run. It turns out that the only index I had on any table was on the surrogate primary keys. In essence, the table was indexed by a column I did't really care much about, what I do care about is the foreign key relationship between the parent and child. The quick fix for this is to put an index on the foreign key column. I could easily add that directly to the database, but I wanted to make sure I could keep using SchemaExport for updating my database scheme when my mapping files change.

In your nHibernate mapping files you can set a non-clustered index on any property by using nHibernate's index option. For the many-to-one side of my relationships I added:

```xml
<many-to-one name="Entry" index="entry_index"...
```

You can give the index any name you wish. Performance of my views shot up considerably after adding this. Parent-child queries within nHibernate will also benefit. A clustered index would probably make sense here, however that is not an option I am aware of in the nHibernate mapping files. I would have to set that directly on the database which means I can't use tools like SchemaExport.
