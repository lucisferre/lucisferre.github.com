---
title: Implementing a custom ID generator for nHibernate
date: 2009-07-14 02:58:00 Z
categories:
- ".NET"
- Software Development
- fluentnh
- nHibernate
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '26'
---

Currently I am doing some work developing a utility that imports securities data into a fairly old legacy database. It is still SQL but the database and software havn't been updated in about 15 years (maybe more) and never will be again.

The original application for this database uses a primitive hilo-style ID generator mechanism. Tt simply queries an integer value from a table in the database and increments it, then it adds a fixed value to it and uses that as the next unique ID. It doesn't seem very optimal, but that's how it works and there's no changing it.

It isn't complex, but it also isn't one of the identity strategies available in nHibernate and I fully intend to use nHibernate.

<!--more-->

So what to do? One option would be to set `generator="assigned"` and do it all myself, assigning the ID using `ADO.Net `directly or something similar... but what fun would that be? No, I've decided to take a peak at the nHibernate code and learn to extend it. Read on to find out how to create a custom class that inherits from `IIdentifierGenerator `to implement your own custom ID generator strategy for nHibernate.

One of the beautiful things about nHibernate the ability to extend it. You can extend its functionality without ever touching their source code.

It was very easy to locate the classes responsible for ID generation in the `NHibernate.ID `namespace. It was also clear immediately that most classes inherited from `IIdentifierGenerator`. I also knew that my strategy was similar to the procedure that hilo implements so I started with `TableHiLoGenerator `which inherts from `TableGenerator`.

After playing around with this for a while and trying different things, it was clear that `TableGenerator`, basically takes a `tablename `and `columnname `from the configuration, reads a value, increments it and passes it on as an ID. Voila! That was almost exactly what I needed to do with little else to change. I was quickly able to create my own class inheriting from TableGenerator

```csharp
public class FDPSequence : TableGenerator
{
  private const Int32 SeedValue = 1048576;
  private static readonly ILog Log = LogManager.GetLogger(typeof(FDPSequence));
  public override object Generate(ISessionImplementor session, object obj)
  {
    int counter = Convert.ToInt32(base.Generate(session, obj));
    return counter + SeedValue + 1;
  }
}
```

Not much to that is there? Now that is a fairly simple case, but even getting more complex isn't much harder than that. You can implement `IIdentifierGenerator `directly and create something far more customized. TableGenerator actually implements `IPersistentIdentifierGenerator `which is an interface for identifiers which need to write and updated value to the database. However, since nHibernate provides so many versatile and inheritable base class generators that you will likely get by with inheriting one of those and extending it as I did.

In order to use this custom generator class I setup my configuration to use it. Using Fluent I wrote a mapping like this:

```csharp
public class InvestSpecMap : FDPEntityBaseMap<InvestSpec>
{
  public InvestSpecMap()
  {
    WithTable("InvestSpec");
    Id(x => x.Id).ColumnName(tablename + "ID")
      .SetGeneratorClass(typeof(FDPGenerator.FDPSequence).AssemblyQualifiedName)
      .AddGeneratorParam("table", "zSysCounters") //Name of the ID counter table
      .AddGeneratorParam("column", "InvestSpec"); //ID counter column for this table
    Map(x => x.AnnualDividend).ColumnName("AnnlzdDiv");
    Map(x => x.AssetClass);
    Map(x => x.CurrencyType);
    Map(x => x.CusipNumber).ColumnName("CusipNum");
    Map(x => x.DividendFreq).ColumnName("Frequency");
    Map(x => x.FullName);
    Map(x => x.MaturityDate).ColumnName("MaturityDt");
    Map(x => x.PriceAsOf).ColumnName("PriceAsOfDt");
    Map(x => x.ShortName).ColumnName("AbbrName");
    Map(x => x.TaxTreatment);
    Map(x => x.TickerSymbol);
    Map(x => x.Type);
    Map(x => x.UnitPrice);
  }
}
```

You can do the same with XML mappings just declare the assembly and class as you usually would in XML. This is actually the first time I've used FluentNH.

What is great about this is that even when testing my mappings with SQLLite, `SchemaExport `is actually able to create the identity table based on the tablename and columnname properties in the nHibernate configuration. The main benefit here however is that identity generation remains transparent to the rest of my application. Also doing something cool with nHibernate, that's priceless of course
