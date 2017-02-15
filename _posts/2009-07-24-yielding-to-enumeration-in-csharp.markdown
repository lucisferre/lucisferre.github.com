---
title: Yielding to enumeration in C#
date: 2009-07-24 21:48:00 Z
categories:
- ".NET"
- C#
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '27'
---

I just discovered the `yield` keyword and I can't believe I hadn't found it sooner. I became a huge fan of using LINQ from the moment I started using it and I am in the habit of passing and returning IEnumerable whenever possible.

<!--more-->

Recently I had a problem where I was reading the lines of a CSV file using this code:

```csharp
var csvdata = File.ReadAllLines(filename);
```

That seemed to work fine, but for some mysterious reason (that I still am uncertain of) it had problems reading the CSV file cleanly from a networked drive for some users. I have no idea why but I wanted to use something a bit more transparent.

A bit of googling turned up a [solution ][1]that used a keyword I had never seen before:

```csharp
private static IEnumerable<string> ReadLines(string path)
{
  using (StreamReader reader = new StreamReader(new FileStream(path, FileMode.Open)))
  {
    string line;
    while ((line = reader.ReadLine()) != null)
      yield return line;
  }
}
```

I marveled at this `yield` keyword, it allows a method to return a result as `IEnumerable`, with delayed evaluation (each item in the result set is only returned when it is asked for) without having to implement a new `IEnumerable` class.

Anyways, nothing ground breaking, but a pretty handy tool nonetheless. Its been a while since I posted anything and I figured this was as good as anything else I've been thinking of posting, but nice and simple.

   [1]: http://stackoverflow.com/questions/286533/filestream-streamreader-problem-in-c-sharp

