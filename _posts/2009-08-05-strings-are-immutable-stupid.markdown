---
author: Chris Nicola
date: '2009-08-05 05:49:00'
layout: post
slug: strings-are-immutable-stupid
status: publish
title: Strings are immutable stupid!
comments: true
wordpress_id: '32'
categories:
- .NET
- C#
- fail
---

Last night, I was reading through the MCTS cram guide for Application Developer (and drinking beers) and enjoying a bit of a laugh as I learned ground breaking concepts like "value types" and "creating class" (it's not _all _that bad) when I came across this "Strings are immutable in the .NET Framework".

<!--more-->

I'm sure most people reading this (which, of course, assumes _anyone _is reading this) are either saying, "who cares, even in java strings are immutable, everyone knows this you idiot", or alternatively asking "what's an immutable?".  To the first of you I say "fuck off,  I skipped most of my compsci classes", to the rest of you I say "[let me Google that for you...][1]".

After reading that strings are immutable, I immediately thought of some code I am working on that reads a giant ASCII CSV file about 700k lines long.  As part of the process it reads and re-builds a CSV line character by character using nothing but "+=" assignments.  I was horrified, as I realized that I was literally creating and assigning millions of string objects unecessarily.

I had done a horrible thing, but I knew how to fix it, use a StringBuilder class instead.  Now, my morbid curiosity drove me to fire up my code in dotTrace just to see how horrible.  Here are the results for the original method:

```
18.99 % FixCSVStringQuotes - 142869 ms - 757810 calls - ISMDataAdapter.Data.ISMDataReader<T>.FixCSVStringQuotes(String)   
17.58 % Concat - 132204 ms - 152390966 calls - System.String.Concat(Object, Object)   
0.01 % ToString - 109 ms - 757810 calls - System.Char.ToString()   
0.00 % Concat - 0 ms - 64 calls - System.String.Concat(String, String)
```

Results for StringBuilder version:

```
10.00 % FixCSVStringQuotes2 - 29408 ms - 757810 calls - ISMDataAdapter.Data.ISMDataReader<T>.FixCSVStringQuotes2(String)   
7.00 % Append - 20574 ms - 153148776 calls - System.Text.StringBuilder.Append(Char)   
0.07 % StringBuilder..ctor - 194 ms - 757810 calls - System.Text.StringBuilder..ctor()   
0.03 % ToString - 101 ms - 757810 calls - System.Text.StringBuilder.ToString()   
0.00 % Append - 0 ms - 64 calls - System.Text.StringBuilder.Append(String)
```

Total time went from 143 seconds to 29 seconds.  Pretty significant really.  So now I know, "strings are immutable stupid!"  Thanks MCTS!

   [1]: http://lmgtfy.com/?q=immutable

