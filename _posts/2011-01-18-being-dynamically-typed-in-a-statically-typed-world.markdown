---
author: Chris Nicola
date: '2011-01-18 20:04:03'
layout: post
slug: being-dynamically-typed-in-a-statically-typed-world
status: publish
title: Being dynamically typed in a statically typed world
comments: true
wordpress_id: '274'
categories:
- .NET
- C#
---

Ever since I started writing code in .NET 4.0 I’ve been trying to find both time and a reason to start using C#’s `dynamic` keyword.  It’s been a challenge.  Initially I found myself at a dead end almost every time I tried to use it to do something dynamic (usually ending at some RuntimeBinderException).

Well after several failed attempts and a lot of unexpected exceptions I finally had my moment of Zen.  I had previously read Rob Connery’s post on [redlining c#’s dynamic features][1] and thought I had understood it but I had missed the central message.  That is, when and where you want to use `dynamic` you sort of have to use it for everything.

No, this doesn’t necessarily mean using `dynamic` instead of `var `everywhere or having every class inherit from `ExpandoObject` (though I think Rob was probably wondering what would happen if you did).  Bottom line is that C# is still a statically typed language and like the functional support in lambda’s dynamic allows us to create a bit of handy syntactic sugar to make our code a bit more expressive, and maybe even fun and sexy.

<!--more-->

I’ll explain what I mean.  Here are some tests that demonstrate what I _thought_ should work and what ended up actually working.

{% gist 785667 %}

The first test fails, actually in hindsight it is kind of obvious.  I am passing a class that is cast as it’s base type to a method that takes the concrete type, in fact, this wouldn’t work even if I wasn’t using dynamic.  For some reason I just expected dynamic to figure it out, possibly because I’m used to using dynamic typing in _dynamic _languages /facepalm.  You might be asking why didn’t I cast the concrete type, well imagine you don’t have type and it comes as object, or the method that took the class only takes the base class (I have a practical example coming up)

This next test demonstrates what I was _trying_ to do using standard reflection.  Pretty basic stuff, call a method I may not know exists with an parameter for which I know the type satisfies the method signature, but I may not be able to actually cast it.

Finally, in the last test we redline it a bit.  I figured perhaps I should just cast the parameter to dynamic first, then it should determine the type at runtime, right?  Voila, it passes.  Honestly, it seems sadly obvious to me now that I’ve written it up and expressed it as simple tests but I suppose that says a lot about the value of TDD and learning tests.

I promised there would be a more practical example so [here it is][2].  These command and domain event dispatcher classes use Castle Windsor to resolve the handlers. However, the way they work we can’t statically know the type of the incoming ICommand/IEvent and, as a result, we also can’t know statically which  ICommandExecutor<T> or IEventHandler<T> handles it.  We would typically use reflection here, but instead it was possible to use dynamic for both.

So what do I think of all this now?  Is it a better idea to use **dynamic** instead of reflection?  In my opinion, yes, at least when you can but I have two parting thoughts about it, keeping in mind I’m just getting started with this **dynamic** stuff.

**WRITE SOME TESTS!**

I can’t stress this enough I mean it is important enough in general, but just like the reflection example we lose statically typed refactoring support when refactoring a class that is used dynamically elsewhere in our code.  Unit tests are what save us from checking in broken calls using dynamic.  From what I understand, it isn’t uncommon to see Ruby libraries with 100% test coverage for this reason.

**You may not want to publically expose your dynamic bits**

Without good reason anyways, I could see a mocking framework or test API using a lot of dynamic for convenience.  Likewise, a document database layer or REST API may want to use lots ExpandoObject or even custom dynamic types.

Just keep in mind C# is still, primarily, a statically typed language.  So you probably shouldn’t be passing or taking dynamics publically without a good reason.  Which is fine, since It is easy to leverage it internally via a cast and still avoid nasty reflection code.

All that said, I really love dynamically typed code so if we suddenly have a C# **dynamic** revolution as happened with **var** with people using it everywhere, I’m all for it.  However I seriously doubt we’ll see that happen any time soon.

   [1]: http://blog.wekeroad.com/2010/08/09/csharps-new-clothes
   [2]: https://github.com/lucisferre/ncqrs/tree/WindsorCommandSerivce/Extensions/src/Ncqrs.Config.Windsor

