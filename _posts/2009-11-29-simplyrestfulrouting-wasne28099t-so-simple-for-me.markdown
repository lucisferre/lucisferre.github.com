---
title: SimplyRestfulRouting wasn’t so simple for me
date: 2009-11-29 23:28:00 Z
categories:
- ".NET"
- ASP.NET
- MVC
- REST
author: Chris Nicola
layout: post
status: publish
comments: true
wordpress_id: '52'
---

![complicated2][1]

Ok, the title is a bit misleading, the SimplyRestfulRouting available from the MVCContrib package is indeed, very simple to use.  Adam Tybor created it and you can read most of what you need to know about it [on his blog][3], I highly recommend you start there and not here if you plan on using this.  Unfortunately, that post is a bit old and it also doesn't really specify or document exactly hot to use the class.   The sample I managed to find on code project also seems to be quite out of date and I could not get it running for whatever reason, I'll take stab and chalk it up to my own impatience.

<!--more-->

Anyways, while working on Graphite (my lame attempt at creating a blog engine) I read about SimplyRestfulRouting and decided I wanted to use RESTful style URLs too (trust me all the cool kids are doing it).  The basic concept is that instead of using the standard ASP MVC convention of `{controller}/{action}/{resource identifier}`, we use `{controller}/{resource identifier}/{action}`. The idea being that first we get the resource and we support a standard set of actions that one can perform on it, like _show, edit, new, delete, etc._ As I understand it a lot of this has its roots in Rails style conventions.

Lets try some examples, take the following two urls: 

`http://www.myblog.com/post/this-is-my-blog-post-slug-text`

`http://www.myblog.com/post/show/this-is-my-blog-post-slug-text`

Sure, it is completely clear that with the second link we want to show that blog post, however it isn't what you typically see on the web is it?  In the first example, show is assumed to be the default action for that resource.  If we wanted to edit it, we would see this:

`http://www.myblog.com/post/this-is-my-blog-post-slug-text/edit`

Likewise if we wanted to create a new post we would see this:

`http://www.myblog.com/post/new`

Which doesn't refer to a resource and would, in fact, be the same thing we would do with the standard conventions.

SimplyRestfulRouting defines a set of 8 actions that you can perform on a resource.  They are (this list is stolen from Adam's blog):

  * Show : handles a GET request for a displaying a single resource that the controller is representing. 
  * Create : handles a POST request for a creating a new resource. 
  * Update : handles a PUT request for updating an existing resource. 
  * Destroy : handles a DELETE request on a resource. 
  * Index : handles a GET request for a collection of resources. 
  * New : handles a GET request for a blank form for creating a new resource 
  * Edit : handles a GET request for a form with values filled in from the resource for updating. 
  * Delete : handles a GET request a confirmation / form with options for deleting a resource. * This is the extra action. 

They also correctly use all the fancy form methods like DELETE and PUT and for browsers that don't support DELETE and PUT SimplyRestfulRouting allows you to use a field called \_method on forms to provide backwards compatibility in this scenario.

The not-so-simple part for me was that I could not get the routing to work whatsoever.  The routes looked fine, but only Index and New worked, anything involving an id was failing.  This of course should have been the big red flag directing me to where the problem was but instead it took me the better part of a day to discover what was happening.

The crux of the problem actually stemmed from the `SimplyRestfulRouting.BuildRoutes()` function I was using.  If you do not use the last overloaded method which has the full set of parameters it takes it will default to filtering out all resource id's by assuming they must be integers and I unfortunately chose to use Guids.

When I finally saw the code I have to admit I facepalm'd a little.

```csharp
public static void BuildRoutes(RouteCollection routeCollection, string areaPrefix) 
{ 
  BuildRoutes(routeCollection, FixPath(areaPrefix) + "/{controller}", MatchPositiveInteger, null); 
} 
```

I have to vent a bit here, this really should use MatchAll by default. I really don't like the idea of having a default filter anywhere, if I don't write a _where_ clause in a SQL statement I don't expect SQL to choose some default for me. Nonetheless. with that issue out of the way I was able to easily register my RESTful routes for my two MVC areas:

```csharp
public class RouteRegistrar { 
  public static void RegisterRoutesTo(RouteCollection routes) { 
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}"); 
    routes.IgnoreRoute("{*favicon}", new {favicon = @"(.*/)?favicon.ico(/.*)?"}); 
    routes.CreateArea("Admin", "Graphite.Web.Controllers.Admin",GetRestfulRoutes("Admin/")); 
    routes.CreateArea("Root", "Graphite.Web.Controllers",GetRestfulRoutes("")); 
  } 

  private static Route[] GetRestfulRoutes(string areaPrefix) { 
    var routes = new RouteCollection(); 
    SimplyRestfulRouteHandler.BuildRoutes(routes, areaPrefix + "{controller}", null, null); 
    routes.Add(GetDefaultRoute(areaPrefix)); 
    return routes.Cast<Route>().ToArray(); 
  } 
  private static Route GetDefaultRoute(string areaPrefix) { 
    return new Route(areaPrefix + "{controller}/{action}", 
      new RouteValueDictionary(new {controller = "Home", action = "Index"}), new MvcRouteHandler()); 
  } 
} 
```

The `GetDefaultRoute()` method is there to enable selecting a default home controller.

Well, pretty simple isn't it? And it only took me all day.

   [1]: /images/complicated2_thumb.jpg (complicated2)
   [2]: /images/complicated2.jpg
   [3]: http://abombss.com/blog/2007/12/10/ms-mvc-simply-restful-routing/
