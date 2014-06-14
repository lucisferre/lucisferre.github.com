---
author: Chris Nicola
date: '2011-03-18 12:22:08'
layout: post
slug: were-all-out-of-duct-tape-try-putting-some-cheap-code-on-it
status: publish
title: We're all out of duct tape, try putting some cheap code on it
comments: true
wordpress_id: '559'
categories:
- .NET
- Agile
- Software Development
- agile
- code-style
- coding
- rant
- SOLID
- TDD
---

Before I get this little rant started I just want to make a quick apology.  I
started [this rant on twitter][1] regarding [this blog post][2] before I later
remembered one of my colleagues had actually sent it out to me and then
thinking that I should probably tone down the rhetoric.  So let it be known
that I'm an opinionated jerk sometimes and no one should feel the need to agree
with me on _anything. _In fact I think it's good when people don't. 

<!--more-->

I did reread it a couple time now and while and found I actually don't disagree
with most of the conclusions the article made. Instead, my problem is actually
that I find it’s premises are weak and in some cases misleading. My reaction
was partially fuelled by the fact that this had reminded me of Joel Spolsky's
post on [duct tape programmers][3] which I also had my [own opinions on][4].
Again nothing overtly wrong with the overall message in what Joel is saying,
but how he is saying it confusing and misleading. Both seem to imply that our
choice is between cheap code + duct tape = shipping on the one side and
over-architecture + bad use of patterns = never shipping.  This seems like two
extremes and I have to believe there is some solid middle ground. 

One thing that bugged me about both posts is the use of self-evident claims and
conclusions to make the arguments look like they make more sense.  For example
take this quote from Joel: 

> And the duct-tape programmer is not afraid to say, “multiple inheritance sucks. Stop it. Just stop.”

WTF!? Of course multiple inheritance sucks.  What year did you stop writing
code Joel?  This has been common knowledge for quite a while now, some have
even moved on to [attacking inheritance in general][5]. Is this the special
quality of the duct tape programmer?  An uncanny ability to make completely
obvious statements seem meaningful? I digress though, this isn't about duct
tape programming it's about cheap code so lets start with Richard's summary. 
Here are the conclusions Richard makes: 

>   * Know your design patterns and the benefits of each one of them
>   * Plan your code to be cheap 
>     * Use patterns where they are going to be beneficial and will save you time
>     * Ignore the patterns that will not (e.g when was the last time you ported a system to a different DB?)
>     * Use frameworks where appropriate to speed up development;
>   * Refactor when required, don’t get ahead of yourself;

Abso-#$%ing-lutely!  But seriously though, re-read all that one more time.  All
this seems to be saying to me is _“only do what you need when you need it.”_
Well, it is pretty hard to argue with a tautology like that. Lets face it, the
real problem is actually knowing what you need and what you don’t. 

Richard is absolutely correct that it is more important to know when to use
design patterns than how to use them (though that’s also statement I’ve seen
made so many times now it’s almost a cliché). The question is **how** do we
learn when and when not to use patterns. Just as important is how do we know
how much refactoring is _required_ and how do we safely plan our code to be
cheap without risking it becoming a mess of spaghetti code? 

I’m going to let everyone in on a _very_ big secret. Something that the most
secretive societies of computer science have kept carefully concealed only
practiced during private ceremonies and never taught to the uninitiated.  I
will probably be dead by tomorrow but I feel **the people need to know**. 

### TDD

There I said the words, test. driven. design. 

What I find disappointing about Richard’s (and Joel’s) post is not that his
conclusions are wrong, they are all right and good, but that he provides little
or no guidance on one gets there from here. To that I say one shorter answer is
TDD. 

It is a well known fact that you are going to write bad code, I am going to
write bad code, Richard (I presume) is going to write bad code, bad code
happens every day. This, is the reason we refactor.  However, refactoring
itself implies change and change has some amount of inherent risk.  The more
complex our code becomes the riskier it is and without any tests covering what
we’ve done the long term product will end up being a big ball of legacy mud no
one wants to refactor or improve in no time.  

After this point all anyone is comfortable doing is patching up the code with
cheap hacks and tricks that appear to work, but probably leave many hidden bugs
and problems that build up over time until our whole house of “cheap code”
falls in on itself. 

Does this sound familiar to anyone?  Of course it does, we’ve all seen, worked
on, probably even helped to build legacy code projects and we all know where
they end up in time.  It is testing that gives us the ability to refactor
fearlessly, to change, improve and fix our cheap code strengthening and shoring
it up to carry the next features when they are asked for. 

### Reversibility

Richard suggest we should _plan _for cheap code. Well how do we do that? In a
word: reversibility. We always know more about what is needed near the end of
the project than at the beginning and this is why reversibility is important. 

We want the freedom to change our minds and do so inexpensively. Reversibility
is our ability to make mistakes, take the wrong path and then change our minds
later. If you are planning for cheap code then it’s an absolute must you are
building for reversibility. However reversible code is not nearly as cheap as
plain old cheap code. Reversible code requires us to follow certain guiding
principles and even potentially apply some patterns to reduce coupling,
increase cohesion and avoid accidental complexity down the road. 

So how do we write reversible code?  Well unit tests are important for a start,
but we already talked about that.  The rest comes down to sufficient
architectural planning (as opposed to none) and understanding and applying good
principles of design. _Principles_ are more important than patterns on any
given day. 

### SOLID

Faster than a speeding flywheel, more powerful than an abstract factory and
able to leap tall singletons in a single bound, yes principles are far more
valuable than patterns because it is principles that guide us and help us to
decide when patterns are needed.  Patterns are merely the tools, principles
represent the most valuable knowledge. 

### One final thing…

In the end I just don't feel that terms like "cheap" and "duct tape" are good
when used as advice on how to create software.  I prefer "clean" myself.  Of
course, we should strive to be minimalists, adhere to YAGNI and _always_ avoid
over architecture. Our code should be as cheap as possible, but no cheaper.

That being said, truly cheap code (no TDD, zero real architecture planning,
quick and dirty, etc.) can still be useful. It can help you when testing
assumptions or even building a [minimal viable product][6]. However, if you
plan and need to use cheap code then treat it as a prototype, and by that I
mean you plan to really throw it away. Truly cheap code has an expiry date, and
it's usually the next minute after version 1 ships, after that it's time to
rebuild it with some clean code and a bit of planning and design.

   [1]: https://twitter.com/#!/lucisferre/status/47700593866842112
   [2]: http://www.geekm.ag/Archive/Good_code_is_cheap_code
   [3]: http://www.joelonsoftware.com/items/2009/09/23.html
   [4]: http://lucisferre.net/2009/09/28/the-technical-debt-of-red-green/
   [5]: http://vimeo.com/17151526
   [6]: http://en.wikipedia.org/wiki/Minimum_viable_product

