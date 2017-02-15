---
title: A RESTful weekend with Nancy
date: 2011-01-23 21:41:40 Z
categories:
- ".NET"
- NancyFx
- REST
- Windsor
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '286'
---

![CropperCapture][1]

This weekend I had a bit of time to spend trying out the Nancy web framework.
A lightweight HTTP framework inspired by Sinatra (hence the name).  My first
impression was pretty positive, if perhaps a tad over the top.

Nancy is still in it’s infancy but even so it is very cool stuff .  The guiding
principle seems to be an emphasis on simplicity, avoiding leaky abstractions
and providing lots of room for extension.  There is also some pretty cool use
of the new C# dynamic features which provide some pretty unique syntactic
sugar.

<!--more-->

IoC support was recently added for Ninject, StructureMap and Unity, so I
figured I’d have a go at adding Windsor support.  I copied the test cases from
the other containers, made some necessary changes and started working on making
them all pass.  Using the other implementations as examples it was fairly easy,
other than a couple of expected gotchas due to differences between containers.

I’m not 100% confident my changes are production ready, but I managed to pass
all the tests, plus one more to cover a bug I had.  I even made sure it would
play nice when using Castle’s proxies for AOP support.  A [shiny new pull request][3] 
was the fruit of these small labours.  I’d certainly appreciate a second
opinion from any Windsor veterans out there since my understanding of how
Windsor works internally is pretty minimal.

One of the things I really like about Nancy is how easy it is to understand how
it works, even when digging into the code under the hood.  The code is very
clean, easy to understand and clearly expresses how the HTTP request/response
is being handled.  The pattern for creating an HTTP method in Nancy couldn’t be
simpler either.

{% gist 792857 %}

If you haven’t seen any Nancy samples yet, this might look a bit weird.  Yes,
that is in fact the constructor that is containing all those route methods.
You can put all the method logic in those lambdas if you want, but in the end,
I just wasn’t entirely comfortable with that so I moved the logic into helper
methods.  I hope the rest of the Nancy users won’t hang me for that.

Also the `Request.Body.FromJson<T>() `is mine too.  Nancy doesn’t, as yet, have
any built in support for deserializing JSON or XML requests, but I also see
that as a good thing as it means lots of fresh opportunities to contribute.

On the POST method I’ve also added a filter `CanPost`.  A filter can be used on
a route to determine whether or not to resolve a route, however it returns a
`bool` and so that is pretty much all it does.  What I was expecting was more
like Ruby/Rails [before and after filters][4].  If I return false from a filter
in Nancy all I get is a not found result, which doesn’t give me much
flexibility. 

There is a discussion going on the Nancy group which gets into whether or not
filters should and could be used to handle cross-cutting concerns like
authentication.  In the end I took The suggestion to just use some sort of AOP.
I still think something better could be done here though and I have some ideas
currently stewing so we’ll see if anything comes of it.

Using a Castle Interceptor it was pretty easy to wire up some simple AOP for
this.  Here's the gist of it:

{% gist 792875 %}

It uses a `RequresAuthentication` attribute on methods I want to intercept.
The interceptor will replace the Response with a 401 if authentication fails.
It’s worth noting however that I had to modify Nancy’s IRequest/Request class
to access the URI query parameters.  I’m not sure why this wasn’t exposed
earlier, but there is a discussion on it here, and I’ve [added it to my pull
request][5].

I was very happy to make so much progress in a short time with Nancy.  In the
end I even managed to convert the project I’m working on from OpenRasta to
Nancy and get it up and running.  That isn’t to suggest that I feel Nancy is
better or that I’ve decided switch over too it.  For now I’m just kicking the
tires, but they definitely some pretty sweet tires.

   [1]: /images/CropperCapture1_thumb.png (CropperCapture[1])
   [2]: /images/CropperCapture1.png
   [3]: https://github.com/NancyFx/Nancy/pull/39
   [4]: http://guides.rubyonrails.org/action_controller_overview.html#filters
   [5]: https://github.com/lucisferre/Nancy/commit/257068933ec9991b5fb42afca01ced617683ae80

