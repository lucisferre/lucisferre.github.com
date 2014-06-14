---
layout: post
title: "What's so great about Twitter Bootstrap?"
date: 2012-06-27 08:32
comments: true
categories: [css, html, javascript, coffeescript, design]
---

I've been a pretty big fan of Twitter Bootstrap since it came out. I've used it
for [Pixel Publish](http://pixelpublish.com) and for the early prototypes of
[TicTalking](http://tictalking.com). However I've never really talked much
about it on my blog and I wanted to explain a bit about why I feel Bootstrap is
an absolutely invaluable tool, in particular, for developers who are just
getting their feet wet in the world of web design and want to "bootstrap" their
learning.

<!--more-->

## Modular CSS is awesome

One of the things that most developers struggle with early on with web
development (well I know I did anyways) is how to keep design modular. By that
I mean, separated, decoupled and reusable, all of the same attributes we like
to see in good code are just a relevant in our CSS and  HTML. In fact this is
crucial if you plan to be able to work on the same HTML and CSS with other
people concurrently (just as is true for code).

I recently attended Jonathan Snook's [SMACSS](http://smacss.com) workshop to
get more in-depth into good CSS and HTML structure. SMACSS stands for Scalable
and Modular Architecture for CSS. Snook mentioned that when he first took a
look at the CSS for Bootstrap he immediately recognized it as similar to his
SMACSS approach, even though the author's of Bootstrap claimed to have never
heard of Snook's guide. In other words well structured CSS is instantly
recognizable and Bootstrap comes with it built in.

Learning to write good modular CSS is *hard*. I mean CSS is pretty much pure
*insanity* to begin with and all SMACSS really does help to bring a modicum
sanity to it, but lets face it, no one should expect that running the asylum
will be an easy task. 

If you're even moderately familiar with CSS, it's very likely that Bootstrap
has a lot to teach about modular CSS architectures simply by way of example.
Personally I've learned more about CSS and more importantly well organized and
structured CSS in 3 months or so of working with Bootstrap than I ever knew, in
fact I've probably forgotten more about CSS in the past 3 months than I ever
originally knew too. Bootstrap isn't just a good way to bootstrap your project
or site, it's also a great way to bootstrap learning about HTML and CSS and
design language.

## Use a common design language

The web is full of commonly seen and repeated patterns of design. Things like
header menus, navigation, dropdowns, button groups, progress bars, labels,
alerts, image thumbnail boxes, etc. The list of simple web design patterns that
Bootstrap comes with out of the box is impressive. 

Snook likes to call these "modules" in SMACSS terms. Bootstrap gives you a
plethora of great working examples of these and how to keep these modules clean
and reusable. For this reason alone Bootstrap is invaluable for learning to
write CSS. The added bonus is that having so many of these patterns at your
fingertips is also great for prototyping productivity. So it's efficient learning
with no lost productivity, tastes great less filling!

I highly recommend checking out the [Bootstrap documentation](http://twitter.github.com/bootstrap) and seeing for yourself. A
few designers I've shown this too loved seeing all the design language laid out
and exposed this way. In fact I'd go so far as to suggest that every project
should have this sort of a living documentation. It's great for facilitating
design communication and helps to bring new team members up to speed quickly.

## Responsive design is totally trendy

Responsive design is *the* new hotness in web design and I friggin' love it.
I've always been bugged by sites that didn't work well or look good on my phone
(or a tablet for that matter). One of the big selling points of Octopress for
me was a completely responsive design (so if you're reading this from your
phone, you're welcome).  Mobile only versions of sites are often neglected or
restrictive, or just plain boring. 

Responsive design is about transforming site layout appropriately for any form
factor and Bootstrap just comes with it, in that "it just works (tm)".
Responsive design is something everyone should learn and once again Bootstrap
provides a great way to see how it's done.

## Javascript components for *all the things!*

Ok so putting aside any feelings you may have about or towards [semi-colons in Javascript](https://github.com/twitter/bootstrap/issues/3057) (btw the only
appropriate feeling to have is 'move along, nothing to see here'). Bootstraps
approach to Javascript is, in my opinion, quite elegant and I've started
applying it to a large portion of my interactive web components. What's so
elegant about it?

Each piece of Bootstrap's Javascript is built as a stand-alone, modular and
'only one thing' pluggable component. In fact I think Crockford was far too
busy bike-shedding about issues of style that he missed out on the other "good
parts", you know those things we all agree are *really* important in good code
(disclaimer: I actually agree with Crockford on the semicolons issue but I'm
not going near that one right now). Now I just generally dislike Javascript
(too much cognitive friction) so I use Coffeescript to build my components, but
just like the Bootstrap guys I've learned how to keep each one as a separate
reusable piece of pluggable behavior, and it's awesome.

I also love their use of data-api attributes to allow these components to be
used transparently and without relying on manually attaching them via
Javascript/jQuery glue code. For example, you just add a
`data-toggle="collapse"` to something you want to be a collapsible section and
it is!

So what does one of these components look like? Well, you could just checkout
Bootstrap, but then you would have to look at ugly Javascript so here is a very
simple one in Cofeescript.

```coffeescript
$ () ->
  $('body').on 'focus.location.data-api', '[data-location=true] textarea', (e) ->
    form = $(this).closest('form');
    location_field = form.find('.location')
    longitude_field = form.find('.longitude')
    latitude_field = form.find('.latitude')
    geo = new Geocoding()
    geo.locate (coords) =>
      latitude_field.val coords.latitude
      longitude_field.val coords.longitude
      geo.reverseGeocode coords, (results) =>
        location_field.val results[0]
```

I use this to automatically add geolocation data alongside any textarea that
specifies it expects it via the `data-location="true"` attribute. When the
textarea is focused this component will attempt to obtain the current location
and populate some hidden latitude, longitude and location text fields. The
`Geocoding` class is just a simple geolocation feature wrapper since I find the
API to be a bit weird.

We have lots of components for various purposes, some are copied (though
modified somewhat) from Bootstrap, like our tab control and collapsible
elements. Others are specific to our site, like the component that handles
toggle buttons for following/unfollowing someone. Often these components fire
events off whenever they do something and we can use those events to customize
and extend the behaviors further using Backbone (or whatever) in an open-closed
principle sort of way.

## Growing up and moving on

Bootstrap has been a efficient way to both prototype designs *and* to learn
modular CSS, HTML and JS but I now feel ready to go out into the world on my
own, build my own stuff without Bootstrap's assistance. I feel much more
confident and able to implement custom designs in a modular way, to break them
down into separate components and modules of style, layout and behavior and
I can keep things much more sane and manageable.

Bootstrap really lives up to it's name in more ways than one. Sure it's not
perfect and I've had frustrations and complaints with this and that, but what is?
Bottom line is, if you're looking for a way to level up your HTML, CSS and JS
there are certainly worse frameworks you could be learning from.
