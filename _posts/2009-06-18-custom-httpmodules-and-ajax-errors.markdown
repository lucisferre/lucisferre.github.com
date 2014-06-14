---
author: Chris Nicola
date: '2009-06-18 22:29:00'
layout: post
slug: custom-httpmodules-and-ajax-errors
status: publish
title: Custom HttpModules and AJAX errors
comments: true
wordpress_id: '19'
categories:
- .NET
- analytics
- ASP.NET
---

Seems like all I do lately is fix other people's GA plugins. This one is really handy though. It creates a pluggable HttpModule to insert the GA script on all your ASPX pages. I found the original post [here][1]:

First thing I noticed was the Google script is out of date which was easy enough to fix. More recently. I found out it also doesn't play nice with things like AJAX which apparently trigger HttpApplication.BeginRequest a whole lot. I received [this][2] error on one of my pages that uses AJAX.

<!--more-->

Unfortunately, the suggested solutions are many fold, most likely because the causes of that particular error are many fold. In my case it was the HttpModule. The thing is, I couldn't just turn it off, my client likes the GA statistics as much as I do.

To fix this all you need to do is make sure the module logic is _only _fired if the source of event is an .aspx page. The relevant code looks like this:

```csharp
void OnBeginRequest(object sender, EventArgs e)
{
  //only if the request comes from an .aspx page
  if (HttpContext.Current.Request.Path.ToLower().IndexOf(".aspx") > -1)
  {
    application.Response.Filter = new AnalyticsStream(application.Response.Filter, accountNumber);
  }
}
```

This like will solve similar problems with other IHttpModule implementations out there too.

You can get the source for the fixed Clarius.GoogleAnalytics module here:

Edit: Removed link, sorry the file was lost in a blog move.

   [1]: http://weblogs.asp.net/cazzu/archive/2006/06/08/instantgoogleanalytics.aspx
   [2]: http://forums.asp.net/t/1243449.aspx

