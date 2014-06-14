---
author: Chris Nicola
date: '2011-02-19 13:06:19'
layout: post
slug: quick-and-dirty-aop-in-nancy-with-castle-windsor
status: publish
title: Quick and Dirty AoP in Nancy with Castle Windsor
comments: true
wordpress_id: '386'
categories:
- .NET
- AoP
- ASP.NET
- NancyFx
- REST
- Windsor
---

I’m really enjoying using Nancy to build my RESTful web API, however being as
new as it is means that it is missing some of the kinds of features you might
have in more mature web frameworks (though at the rate it’s being developed
this won’t be true for very long).  One of these missing features is a way to
do some aspect oriented programming around the request/response pipeline. 

As of right now Nancy basically lets you map a request URL to a method which can format the
response back. There is a concept of a “filter” but it isn’t like the
before/after filters of Rails, it’s just a boolean route matching filter (if
false then the route doesn’t match). What we really need is a way to insert our
aspects (logging, authentication, error handling) in the request and response
pipeline both before and after the handler method.  This is a common feature of
other web frameworks like [OpenRasta][1], [Rails][2] and also [Sinatra][3],
from where Nancy draws it’s inspiration.  

<!--more-->

A discussion is currently going on about how to implement this for Nancy and
something has been [promised soon][4], but what do we do in the meantime?  Well
we use our IoC container and it’s dynamic proxy support of course.  

Pretty much every container out there (except Unity I think, no suprise there)
supports some kind of interception as a way of getting some quick and dirty AoP however I’m
not going to cover all of them, just Windsor since I’m more familiar with it,
nonetheless you should be able to follow the same basic approach with any other
container supported by Nancy (again, except Unity). 

First, you will want to check out my forked “[development][5]” branch of Nancy
(at least until I submit a new pull request).  I had to make some adjustments
to the WindsorBootstrapper due to Windsor not having the same type of child
container behaviour as other IoC containers ([this appears to be by design][6]). 
I’m glad Krysztof has involved himself in the discussion and I’m confident we
will work out these issues very soon).  Bottom line, my Windsor bootstrapper
implementation is 100% stable (and memory leak free) but it only supports
running under ASP.NET, it will not work under anything else.  Also it doesn’t
support the Nancy per-web-request registration, instead register those
components using Windsor’s own `PerWebRequestLifeCycle` and add the module to
your web.config. 

{% gist 835350 WebConfig.xml %}

I like to have things wired up by convention (over configuration) so I’ve
create a facility to register my interceptors to my NancyModule classes. 

{% gist 835350 NancyFacility.cs %} 

The `IAuthenticated` interface decorates any NancyModule implementation where I
want to use the interceptor to do authentication.  To use an interceptor we
need to have a public or protected virtual method to intercept (this is
required by the dynamic proxy) so here is how I would write my module handler. 

{% gist 835350 AuthenticatedModule.cs %}

We the authentication happens right before the call to the virtual method.  The
`RequiresAuthentication` attribute tells us which methods require
authentication, we can also have attribute parameters to restrict access to
specific roles if we need.  Finally, here is the interceptor itself. 

{% gist 835350 AuthenticationInterceptor.cs %}

Obviously we are just faking authentication here, but you get the idea.  

You can inject aspects both before and after the request here.  I’m using
interceptors to handle authentication, generic logging and error logging (using
log4net) and also for handling exceptions by converting them into an
appropriate response object that the API client can use. 

I hope this is useful for people getting started with Nancy.  I fell that AoP
is an important part of designing a good HTTP API that is easy to work with. 
Though, as straight forward as this is to do using the container I am very much
looking forward to seeing this as a first class concept in Nancy as it will
avoid some of the need for virtual methods and attributes, which I don’t
particularly like (nothing sucks like creating a new handler method and
forgetting to make it virtual).

   [1]: https://github.com/openrasta/openrasta-stable/wiki/PipelineContributors
   [2]: http://api.rubyonrails.org/classes/ActionController/Caching/Actions.html
   [3]: https://github.com/sinatra/sinatra/blob/1.1.0/CHANGES
   [4]: https://groups.google.com/d/msg/nancy-web-framework/AaLKVLskUsY/xae00dsdQlcJ
   [5]: https://github.com/lucisferre/Nancy/tree/development
   [6]: https://groups.google.com/forum/?fromgroups#!topic/nancy-web-framework/tDaFWiALCD0

