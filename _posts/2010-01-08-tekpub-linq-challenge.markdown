---
author: Chris Nicola
date: '2010-01-08 16:51:00'
layout: post
slug: tekpub-linq-challenge
status: publish
title: TekPub Linq Challenge!
comments: true
wordpress_id: '60'
categories:
- .NET
- linq
- Tekpub
---

```csharp
var primes = Enumerable.Range(1, n) 
    .Where(x => (x > 1 && x < 4) || 
        !Enumerable.Range(2, x/2 - 1).Any(p => x % p == 0)); 
```

[http://www.codethinked.com/post/2010/01/08/TekPubs-Mastering-LINQ-Challenge.aspx][1]

[http://www.tekpub.com/][2]

Edit: This is definitely a pretty lazy solution I will admit ;P

   [1]: http://www.codethinked.com/post/2010/01/08/TekPubs-Mastering-LINQ-Challenge.aspx (http://www.codethinked.com/post/2010/01/08/TekPubs-Mastering-LINQ-Challenge.aspx)
   [2]: http://www.tekpub.com/ (http://www.tekpub.com/)

