---
layout: post
title: "Getting into vertical rhythm"
date: 2012-10-08 18:46
comments: true
categories: [css, design, Foundation]
---

I've been meaning to write up a blog post about Zurb's [Foundation][foundation]
CSS framework and comparing it with [Bootstrap][bootstrap] and I may still do
one in the near future, but the short version is I was impressed enough with
what I saw that I've already used it to launch [WealthBar's landing page][wealthbar] 
and opened a [pull request][pr] on the project that sets up a decent default vertical
rhythm.

At this point you may be asking, what is vertical rhythm? So let me explain.

<!--more-->

Imagine you have vertical grid on your page with lines spaced at exactly your
base line height (the distance between lines for your standard paragraph text).
Just like line-ruled paper.

What we want is the vast majority of our text and page elements to respect and
align to this grid.  We are free to deviate from it in places, it's even
desirable to call attention to certain elements, but we always return to our
line-ruled paper after for our paragraph text and most other typography on the
page.

The end-result of a solid vertical rhythm is improved readability and an
overall better looking, better feeling typography for the page.

So, how does one get started creating a nice looking consistent vertical rhythm?

## Getting into a rhythm

To get a consistent rhythm requires us to consider our font-size, line-height,
padding, borders and margins on just about every element. To keep things simple
in Foundation I started with the following steps.

1. Setup a baseline variable that could be reused everywhere and makes it easy
   to change. The default is 20px.
2. Setup line-heights for a small set of font sizes, one for each header value
   and also a set of class overrides for each size (.s, .m, .l, .xl, etc.).
   Each of these has a line height that is a multiple of the baseline.
3. Set a default bottom-margin of one baseline height. Top and bottom margins
   that add up to the baseline are also ok, but you run into problems with
   stacked elements as only the larger of the two element's margins is used. So
   this is a sensible default.
4. Carefully go page by page through each of the foundation elements for forms,
   panels, tabs, etc and adjust their padding and margins. This is where you
   may have to take into account top and bottom borders, which will require you
   to subtract those pixels from somewhere else to keep the total height
   correct.

## Show me the code!

The best way to see this in action is to checkout my [pull request][pr]. You
can review the changes, and see the effects of them by starting up the static
html demo in the `/test`. 

```
bundle install
cd text
bundle exec compass watch
```

I also suggest trying out Compass' grid debugger the line at the top of
`_typography.scss`. This will make Compass overlay vertical lines to show you
how everything is aligned.

One comment that I'm sure I'll hear is "why pixels and not ems". To be quite
honest, I'm personally a bit dubious of the value of ems outside of browser
compatibility for scaling on IE6/7. Foundation doesn't support 6 and 7's
userbase is infinitesimally small. Ems are very difficult to work with due to
cascading behaviour and I've regularly run into problems when using them.  That
said, I'm aware "rems" are the new hawtness, so when I have time I'll be
looking into using them with a fallback. Compass currently has a pull-request
in progress to add support for this, so once that's official, I'll probably
move over to rems too.

I've been really happy with the effects of setting up a really good vertical
rhythm. It's easier than it initially may seem, and it's definitely worth a
little bit of effort to get a really professional looking layout.


## Resources

In learning about vertical rhythm I found a number of really helpful resources:

* [A List Apart](http://www.alistapart.com/articles/settingtypeontheweb/)
* [Vertical Rhythm](http://verticalrhythm.org/)
* [Compass: Vertical Rhythm](http://compass-style.org/reference/compass/typography/vertical_rhythm/)

[foundation]: http://foundation.zurb.com
[bootstrap]: http://twitter.github.com/bootstrap
[wealthbar]: http://www.wealthbar.com
[pr]: https://github.com/zurb/foundation/pull/954
