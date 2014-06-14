---
author: Chris Nicola
date: '2011-10-06 07:30:26'
layout: post
slug: dont-call-the-dom-in-a-loop
status: publish
title: Don't call the DOM in a loop
comments: true
wordpress_id: '679'
categories:
- javascript
- jquery
---

One of my mine pet peeves with [backbone.js][1] is how readily it leads to
collection views that are rendered by calling the DOM via `$.append()` in a
loop. 

I've discovered that making updates to the DOM in a loop is, in general,
a bad practice that should be avoided whenever possible. To be completely fair
with small enough collection sizes this isn't really a big problem. So as long
as you are doing proper paging of your collection you shouldn't run into this a
problem. That said you are not always going to work with small collections and
the fact is if it is a bad performance problem for large collections then it is
bad for small one just as well. 

We ran into this a few times to the point where we've decided to make it a
general rule that we do not call the DOM in a loop. Instead we either use
`each` in our jQuery templates (which seems to perform just fine) or
collect the child view html up in a variable and `.append()` it all at once. So
just my advice here, _do not call the DOM in a loop_.  

   [1]: http://documentcloud.github.com/backbone/

