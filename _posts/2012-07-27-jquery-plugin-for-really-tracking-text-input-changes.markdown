---
layout: post
title: "jQuery plugin for really tracking text input changes"
date: 2012-07-27 17:49
comments: true
categories: [jQuery, javascript, coffeescript]
---

It may come as a bit of surprise to some, but the `change` event on HTML text
inputs does not actually get triggered when the text changes. It is only
triggers after focus is lost (on the 'blur' event). This isn't always a problem
but it comes up often enough for me to get fed up and create a jQuery plugin
for this.

This is the first real jQuery plugin I've really had the need to
write. I've generally always found what I've needed by googling for it, or at
least found something close enough to be easily modified for my purpose.

If anyone has suggestions for ways to improve this, or sees any pitfalls I've
missed let me know, or, you know, just edit the Gist.  On the other hand, if
anyone wants to suggest it should be written in javascript you can go take a
long walk of a short pier.

<!--more-->

{% gist 3191297 %}

It's really pretty simple but unfortunately we are forced into using polling to
detect the change. Fortunately, we only poll inputs we have explicitly attached
the plugin to and we only poll while it is focused. So we don't need to
constantly poll. The default polling period is 500ms but you can set it to
whatever you like. The `contentChanged` event also has some custom convenience
properties like `current`, `previous` and `hasContent`.

So what would you use this for? Well detecting when an input actually has some
content, or triggering a spellcheck or autocorrection or really just anything
that you want to execute when the content of a textarea or text input has
changed. For example if we wanted to disable a textarea when it has no content
we could do this:

```javascript
$('#my-textarea').contentChanged();
$('#my-textarea').on 'contentChanged', (e)->
  if e.hasContent
    this.removeAttr('disabled')
  else
    this.attr('disabled', 'disabled')
```

**Edit**: So it looks like there is a way (at least with most modern browsers)
to track input changes using the HTML5 `input` event. The docs are available
[here on MDN](https://developer.mozilla.org/en/DOM/DOM_event_reference/input)
though it is unlcear which browser versions support it. It was also
surprisingly hard to find info on this event too. 

I also found a crossbrowser [jQuery library](https://github.com/dodo/jquery-inputevent) 
that uses this event as well providing some legacy fallback support. So in the end this
appears to have been an entirely academic excercise in creating a jQuery
plugin. Oh well...

**Second Edit** The crossbrowser plugin above does not seem to work well when
binding further up the DOM to the event, something that I have to do to make
things work with dynamically loaded content from PJAX requests (I'm writing
another blog post about why I do this).

So it's back to polling for changes, but I've decided to make a small change to
the plugin above. I've just edited the GIST so you should already be seeing it
(look at the GIST if you want to see the older version).

We now automatically attach to any focused textbox/input and begin polling.
Polling stops as soon as blur is fired. As an added enhancement we catch don't
do this if the browser supports the input event correctly. This makes the
'contentChanged' event completely unobtrusive and it just works.
