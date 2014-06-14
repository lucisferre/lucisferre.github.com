---
layout: post
title: "Three useful jQuery event binding tips"
date: 2012-08-19 12:36
comments: true
categories: [jQuery, javascript, HTML]
---

I want to share a few tips based on the way I've been writing Javascript (well
Coffeescript) for front-end development lately. A lot of it is co-opted from
the Bootstrap libraries but are completely universal and can be applied almost
anywhere.

I've been using these techniques to design small independent component
libraries to encapsulate the interactivity of UI elements. These components are
then bound to elements based on conventions. This has pretty much eliminated
any need to use frameworks like Backbone to keep the javascript clean and
maintainable. In fact, I find I prefer it this way, at least for Rails
development. In other scenarios, as always, YMMV, but either way I think these
tips will be useful regardless of your choice of javascript framework (if any).

<!--more-->

## Binding *way* up the DOM tree

One habit I've developed since I've been into a lot more front-end development
is using jQuery to bind to events way up the DOM tree along with
filter selectors to make sure the handlers are only fired on the right
elements.

Why do this? Primarily because dynamically loaded HTML content will not get events bound to
by jQuery unless I explicitly bind again---it's worth noting that the `live()`
method in jQuery is supposed to do this as well, but [it has been deprecated](http://api.jquery.com/live/) 

So how does this work? Let's say we have simple click binding like:

```javascript
$('.some-class').on('click', someFunction);
```

Binding it up the DOM we would instead bind it like this:

```javascript
$('body').on('click', '.some-class', someFunction);
```

You can also bind to `$(document)`, but I believe (don't quote me) there are
some events---possibly just for some browsers---that do not bubble up to
`document` correctly but do bubble up to `body`. Also as I understand it, there
are a couple of events---again I think only on some browsers and these may be
bugs---that still don't bubble up at all. So you may need to adapt this
approach in certain circumstances. In general, I've found this to work on
modern browsers everywhere I've used it (I'm not targeting IE8 at the moment).
Though, jQuery may have some internal magic to ensure it does work most of the time.

By binding this way event, we will always be able to intercept the events even
if it comes from elements that have been added to the DOM dynamically after
we've bound the event handlers. This is really useful if we are using something
like PJAX to handle navigation without full page loads.

## Namespacing your events

Another useful trick is to add a [namespace][namespace] to your event bindings
to indicate that this binding belongs to a particular component. If you have
not read my earlier post on [Bootstrap][bootstrap] I talk a bit about how they
do this and why I like buildiing small components as a pattern for front-end
javascript.

By binding in this way our components and plugins can bind and unbind their events
without unbinding event handlers attached by other parts of the code.

```javascript
// Binding click for my-component
$('body').on('click.my-component', '.some-class', someFunction);

// Unbinding click only for my-component
$('body').unbind('click.my-component');

// Triggering click only for my-component's handler
$('.some-class').trigger('click.my-component');
```

Triggering events for a specific namespace is probably not a very common
scenario but it's good to know you can do it if you need to.

## Bind to your events using conventions with classes and the data-api

Being able to automatically bind things based on conventions is a lot cleaner
(imo) than explicitly binding or using jQuery extension methods. This is,
again, another technique commonly seen in the Bootstrap components but I like
it so I've stolen it as well.

So say you have a button you want to act as a toggle button with state handled
by javascript, you can use classes to represent the visual state and the
data-api to tell your javascript to bind to it.

```javascript
$('body').on('click.toggle-button', '.btn[data-toggle]', toggleClicked);

toggleClicked = function(e) {
  $target = $(e.target)
  state = $target.data('toggle')
  if (state === 'on') {
    $target.data('toggle', 'off').removeClass('.is-active')
  } else {
    $target.data('toggle', 'on').addClass('.is-active')
  }
}
```

```html
<button class="btn btn-toggle" data-toggle="off">Toggle Me!<button>

<input type="submit" class="btn btn-toggle" data-toggle="off">Toggle Submit Me!<button>

<a class="btn btn-toggle" data-toggle="off">Toggle Link Me!<button>
```

You could also do this using only the classes, for example binding to
`.btn-toggle` and determining state from `.is-enabled`. and skip the data-api
altogether. I'm not really sure there is any real benefit in using the
data-api, but arguably it is seperating the markup that hooks the element into
it's interactive behaviour via the javascript from that which defines styles.
If you don't feel this seperation is important to you feel free not to use it.

Well, that's it for javascript tips. I hope some of you find these techniques as
useful as I have.

[namespace]:http://docs.jquery.com/Namespaced_Events
[bootstrap]:/2012/06/27/whats-so-great-about-twitter-bootstrap/
