---
author: Chris Nicola
date: '2009-12-31 18:00:18'
layout: post
slug: graphite-update-the-automapfilter-for-model-e280933e-viewmodel-mapping
status: publish
title: Using an AutoMapFilter for Model -> ViewModel mapping
comments: true
wordpress_id: '59'
categories:
- .NET
- ASP.NET
- MVC
---

This is my second post on the development of Graphite.  So far progress has been slow but good and I have learned a lot. 

To get started I managed to come up with a basic list of features the blog engine needs so I can work on them one-by-one, so far I have:

  * Can create/edit/delete blog posts
  * Can add comments to a post
  * Can create/edit/delete blog users
  * Users can login/logout and be authenticated to perform administrative tasks
  * Can show a standard RSS feed of posts 
  * Can ping blogging services 
  * Can handle trackbacks from other blogs 
  * Can import from other blog engines (or from RSS feeds at least) 
  * Provide some form of comment spam protection (Askimet and/or Captcha) 
  * Some sort of twitter following plugin 

<!--more-->

The twitter plugin isn't completely necessary but I like the one that I have with BlogEngine and it is a good excuse to try out the twitter API. 

While it has been a slow start, I am just learning ASP.NET MVC, s#arp architecture and Spark so there are many challenges as I learn the architecture.  A number of people in the .NET community have been a big help to me but I want to mention two in particular as they are both relevant to what I am going to show later.

First, I really like [Jimmy Bogard's posts on MVC][1] particularly the posts on [MVC best practices][2] and "[How we do MVC][3]".  More recently, [Howard van Rooijen][4] and some of his colleagues have re-released their "[Who Can Help Me][5]" web application using #A and a few other open source tools.  It is a great example of how to build a #A based app right and has provided me with some excellent guidance.

One 'best practice' I took away from all this was matching each strongly-typed view with a ViewModel rather than passing the Model directly to the view.  There are many reasons why you should do this but I won't get into them here as Jimmy has some good articles on this already. 

In order to facilitate the mundane and repetitive task of mapping fields from Model to ViewModel Jimmy has thoughtfully provided us with [AutoMapper][6] which Howard has also used in WCHM.  WCHM actually wraps the AutoMapper in interfaces which can then be used for dependency injection.  However Jimmy takes a bit of different approach and shows how to perform [mapping using an ActionFilter attribute][7].  This allows us to use attributes to handle the mapping.  Then you simply decorate the Action with an attribute like [AutoMap(typeof(ModelType), typeof(ViewModelType)] and the ActionFilter handles mapping the Model data contained in the ActionResult to the ViewModel.

What I wanted was the best of both worlds, wrapping the mappers like in WCHM but still being able to use Jimmy's ActionFilter approach.  Something that could work like this if I just wanted a generic automapper:

```csharp
[AutoMap(typeof(Post),typeof(ShowPostWithComments))] 
public ActionResult Show(Guid id) { 
    Post post = _posts.GetWithComments(id); 
    return View(post); 
} 
```

or like this if I want to use a specific mapper that I've registered to my container:

```csharp
[AutoMap(typeof(IUserIndexMapper))] 
public ActionResult Index() { 
    return View(_userTasks.GetUsers()); 
} 
```

In order to do this I modified the ActionFilter from Jimmy's post like this:

```csharp
[AttributeUsage(AttributeTargets.Method, AllowMultiple = false)] 
public class AutoMapAttribute : ActionFilterAttribute { 
    private readonly Type _sourceType; 
    private readonly Type _destType; 
    private readonly Type _mapperType; 
 
    public AutoMapAttribute(Type sourceType, Type destType) { 
        _sourceType = sourceType; 
        _destType = destType; 
    } 
  
    public AutoMapAttribute(Type mapperType) { 
         _mapperType = mapperType; 
    } 
 
    public Type SourceType { get { return _sourceType; } } 
 
    public Type DestType { get { return _destType; } } 
 
    public override void OnActionExecuted(ActionExecutedContext filterContext) { 
        IMapper mapper; 
        if (_mapperType != null) mapper = (IMapper) ServiceLocator.Current.GetInstance(_mapperType); 
        else mapper = GetDefaultMapperFor(SourceType, DestType); 
        var filter = new AutoMapFilter(mapper); 
        filter.OnActionExecuted(filterContext); 
    } 
 
    private static IMapper GetDefaultMapperFor(Type sourceType, Type destType) { 
        Type genericClass = typeof (IMapper<,>); 
        Type constructedClass = genericClass.MakeGenericType(new[] {sourceType, destType}); 
        return (IMapper) Activator.CreateInstance(constructedClass); 
    } 
}  
```

The `AutoMapAttrbute` now has two constructors.  The first one is if we just want to use a default generic automapper, this is virtually identical to Jimmy's example.  The second one allows me to specify a specific automapper interface type I want to use. S#arp arch uses the common service locator so I just use that to access my IoC container and instantiate whichever mapper class I have registered.  The actual filter is really simple:

```csharp
public class AutoMapFilter : EmptyActionFilter { 
    private readonly IMapper _mapper; 
 
    public AutoMapFilter(IMapper mapper) { _mapper = mapper; } 
 
    public override void OnActionExecuted(ActionExecutedContext filterContext) { 
        object model = filterContext.Controller.ViewData.Model; 
        filterContext.Controller.ViewData.Model = _mapper.MapFrom(model); 
    } 
} 
  
public abstract class EmptyActionFilter : IActionFilter, IResultFilter { 
    public virtual void OnActionExecuting(ActionExecutingContext filterContext) { } 
    public virtual void OnActionExecuted(ActionExecutedContext filterContext) { } 
    public virtual void OnResultExecuting(ResultExecutingContext filterContext) { } 
    public virtual void OnResultExecuted(ResultExecutedContext filterContext) { } 
} 
```

Finally, the generic mapper and interfaces are very similar to the ones from WCHM:

```csharp
public interface IMapper<TSource, TDest> : IMapper where TSource : class where TDest : class { 
    TDest MapFrom(TSource source); 
} 
 
public interface IMapper { 
    object MapFrom(object source); 
}

public class GenericMapper<TSource, TDest> : IMapper<TSource, TDest> where TDest : class where TSource : class { 
    public GenericMapper() { Mapper.CreateMap<TSource, TDest>(); } 
    public virtual TDest MapFrom(TSource source) { return Mapper.Map<TSource, TDest>(source); } 
    public object MapFrom(object source) { return MapFrom(source as TSource); } 
} 
```

I'm really happy with how this works now, though I do wish that there was a more convention based approach.  I don't really like using attributes and I think a convention based approach makes sense here.  In other words we would always map our model to  ViewnameViewModel whenever the action renders View.  Keep in mind that since we are sticking to the 1:1 View:ViewModel rule this would be appropriate.

I would really like to thank both Jimmy and Howard for all their hard work in helping provide the .NET community with guidance on ASP.NET MVC.  It has been invaluable to me.

   [1]: http://www.lostechies.com/blogs/jimmy_bogard/archive/tags/ASP.NET+MVC/default.aspx
   [2]: http://www.lostechies.com/blogs/jimmy_bogard/archive/2009/10/28/mvc-best-practices.aspx
   [3]: http://www.lostechies.com/blogs/jimmy_bogard/archive/2009/04/24/how-we-do-mvc.aspx
   [4]: http://consultingblogs.emc.com/howardvanrooijen/
   [5]: http://whocanhelpme.codeplex.com/
   [6]: http://www.lostechies.com/blogs/jimmy_bogard/archive/2009/01/22/automapper-the-object-object-mapper.aspx
   [7]: http://www.lostechies.com/blogs/jimmy_bogard/archive/2009/06/29/how-we-do-mvc-view-models.aspx

