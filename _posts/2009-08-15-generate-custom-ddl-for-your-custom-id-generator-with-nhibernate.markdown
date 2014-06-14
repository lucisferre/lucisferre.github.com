---
author: Chris Nicola
date: '2009-08-15 21:49:00'
layout: post
slug: generate-custom-ddl-for-your-custom-id-generator-with-nhibernate
status: publish
title: Generate Custom DDL for your Custom Id Generator with nHibernate
comments: true
wordpress_id: '34'
categories:
- .NET
- fluentnh
- nHibernate
---

A while ago I [posted][1] about writing a custom id generator using nHibernate and extending one of the existing Generator classes nHibernate provides.  This was done to support working with a legacy database which uses a custom id generation strategy.  One thing I like to do with all my apps is write some basic CRUD tests to make sure my mappings and queries are all working properly.

<!--more-->

To do this I use Oren's approach of [creating a temporary in-memory database ][2]using SQLite to work with.  This involves using the  `SchemaExport` tool in nHibernate to generate the DDL to create the database.  If you are using a table generator strategy it also needs to create the table and columns for the custom id generator.  In some cases you may need to customize the DDL for your Id table.

As I discovered the code to generate the table for a custom id is only run once per class (and then only on the base class if you are inheriting).  For my id generator there is a separate column used for each table in the database.  So, for example the _Price _table has a column in the _Counter _table called _Price _to store it's counter and the _Holding _table has a column called _Holding_.

The TableGenerator class supports two parameters which are set in the HBM files (or using fluent) _tablename _and _columnname _which set the table and column names used for the id counter.  If you have a copy of the nHibernate source code you will see that the TableGenerator class calls `SqlCreateStrings()` and `SqlDropString()` to create and drop the table for the id generator.  Unfortunately, as I mentioned above this is only called once, and so it only creates one table with one column for whichever mapping is handled first.

In order to fix this I had to copy the `TableGenerator` class (yes, yes, I know copy-paste is bad form) and modify those functions.  I also added a third parameter _allcolumnnames_ in which I can provide a comma separated list of all the columns the table needs to have (one per table in my database).  The `SqlCreateStrings()` is simply changed to:

```csharp
public string[] SqlCreateStrings(Dialect dialect) {
  string create = "create table " + tableName + " (";
  string insert = "insert into " + tableName + " values (";
  foreach (string s in allcolumns.Split(',')) {
    create += " " + s + " " + dialect.GetTypeName(columnSqlType) + ",";
    insert += " 1,";
  }
  create = create.Trim(',') + " )";
  insert = insert.Trim(',') + " )";
  return new[] {create, insert};
}
```

(Yes, yes, I know I should have learned from my [last post][3] and used a `StringBuilder `here as I pointed out in my last post, but seriously, this method will only ever be called once)

Now when it is called it will create the correct table with all the columns I need. A couple of example mappings for the database in FluentNH are shown below:

```csharp
public InvestSpecMap() {
  WithTable("InvestSpec");
  Id(x => x.Id).ColumnName("InvestSpecID")
    .SetGeneratorClass(typeof (FdpSequenceGen).AssemblyQualifiedName)
    .AddGeneratorParam("allcolumns", "InvestSpec, InvestPrice")
    .AddGeneratorParam("table", "zSysCounters") //ID counter table name
    .AddGeneratorParam("column", "InvestSpec"); //ID counter column name
}
```

```csharp
public PriceMap() {
  WithTable("InvestPrice");
  Id(x => x.Id).ColumnName("InvestPriceID")
    .SetGeneratorClass(typeof (FdpSequenceGen).AssemblyQualifiedName)
    .AddGeneratorParam("allcolumns", "InvestSpec, InvestPrice")
    .AddGeneratorParam("table", "zSysCounters") //ID counter table name
    .AddGeneratorParam("column", "InvestPrice"); //ID counter column name
}
```

To be completely "clean code" and avoid repeating myself I should probably factor out the `Id()` code into a method in an inherited base class so I can call it from each mapping class like this:

```csharp
protected void SetIdGenerator(string tablename) {
  Id(x => x.Id).ColumnName(TableName + "Id")
    .SetGeneratorClass(typeof(FdpSequenceGen).AssemblyQualifiedName)
    .AddGeneratorParam("allcolumns", AllColumns)
    .AddGeneratorParam("table", "zSysCounters") //ID counter table name
    .AddGeneratorParam("column", TableName); //ID counter column name
}
```

Now I can unit test using a temporary in-memory SQLite database and all my tests are passing.

   [1]: http://lucisferre.net/2009/07/14/implementing-a-custom-id-generator-for-nhibernate/
   [2]: http://ayende.com/blog/3983/nhibernate-unit-testing
   [3]: http://lucisferre.net/2009/08/05/strings-are-immutable-stupid/

