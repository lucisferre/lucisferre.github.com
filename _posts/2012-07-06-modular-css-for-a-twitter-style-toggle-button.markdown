---
title: Modular CSS for a Twitter style toggle button
date: 2012-07-06 08:46:00 Z
categories:
- css
- smacss
layout: post
comments: true
---

In the [last post][last] I did on Twitter Bootstrap I mentioned the
[SMACSS][smacss] approach to CSS. One of the reasons I really like SMACSS is
because I've found that, in general, good modularity is key to just about every
aspect of scalable and usable design in coding and is important for far more
than just CSS. 

I want to show a simple example of SMACSS that I think gives a good idea of how
it helps with building reusable and composable CSS modules. Spefically, we'll
build a nice and flexible toggle button.

<!--more-->

Now Twitter Bootstrap does come with it's own toggle button, but it doesn't suit
my purposes. You can toggle it's state and it will look pressed or not pressed,
I want a much more powerful toggle button. One that can display a different
message, icon and style to the user depending on this state. Ironically, this is
exactly the same sort of toggle that Twitter uses for it's own follow/unfollow
button.

This toggle button should be able to do the following:

* Have states for on/off as well as separate hover states for one or both of
  those states
* Have separate HTML for each state allowing for the content of the button,
  including text and/or icons to change
* Outside of toggling, the toggle module shouldn't dictate too much to us about
  what the button can be styled like. Everything else can be applied on top of
  this.

Here is what I came up with for my toggle button.

```html
<button class="btn btn-toggle is-active">
  <div class="default btn-icon">
    <i class="icon-default"></i>
    <span class="text">Default Hover</span>
  </div>
  <div class="default btn-icon">
    <i class="icon-default"></i>
    <span class="text">Default</span>
  </div>
  <div class="active hover btn-icon">
    <i class="icon-active-hover"></i>
    <span class="text">Active Hover</span>
  </div>
  <div class="active btn-icon">
    <i class="icon-thumb"></i>
    <span class="text">Active</span>
  </div>
</button>
```

The SCSS is simply as follows:

```css
.btn-toggle {
  > div { display: none }
  > .default { display: inline-block }

  &.is-active > .active { display: inline-block }
  &.is-active > .default { display: none }
  &.is-active > .active.hover { display: none }

  &:hover > .default.hover { display: inline-block }
  &:hover > .default.hover + .default { display: none }
  &:hover > .default.hover ~ .default { display: none }

  &.is-active:hover > .default.hover { display: none }
  &.is-active:hover > .active.hover { display: inline-block }
  &.is-active:hover > .active.hover + .active { display: none }
  &.is-active:hover > .active.hover ~ .active { display: none }
}
```

You can figure out the CSS pretty easily from that, it's just much more concise
to use SCSS in my blog posts (If you don't like it, just be happy I don't also use HAML).

So what we do here is use the `is-active` class to determine the 'on' state
style while the standard `:hover` selector handles hovering. The CSS2/3
selectors for siblings `+` for siblings after and `~` for siblings before ensure
that the hover state hides all of it's relatives *if and only if* it exists. In
other words if you don't include a hover state div in your HTML it won't hide
the non-hover state so they are optional in the HTML, but the CSS can remain
consistent.

One last thing of note is that if you are trying to maximize browser
compatibility you should put the hover state **immediately before** the state it
replaces. This ensures the `+` selector can be used which is a CSS2 standard
otherwise you may need to rely on the `~`, which is the general sibling selector
and then they don't have to be immediately next to one another. Either way
however the hover states **must preceed** the non-hover states for this to work
because CSS is stupid and has no truly general sibling selector.

Next the `btn-icon` module is applied to each "sub-button" which is responsible
for making these buttons *adaptive* so they change for mobile layouts.

```css
.btn-icon {
  @media (max-width: 767px) {
    .text { display: none }
  }
  @media (min-width: 768px) {
    i { 
      float: left; 
    }
  }
}
```

For tablet and up we float the icon left expecting a span of class `text` to
follow it and for mobile devices we simply hide the text showing only the icon. 

Finally, we've also applied the `btn` class which is simply the default site
style for a button. So there we have three modules that we can compose to create
a neatly styled and adaptive toggle button with icons and text. 

While this clearly isn't groundbreaking stuff I feel this gives a nice and
simple example of how to start thinking about and creating CSS modules.

[last]: /2012/06/27/whats-so-great-about-twitter-bootstrap/
[smacss]: http://smacss.com
