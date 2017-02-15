---
title: Bootstrap 3, Foundation 4 and Bourbon Neat - Part 2
date: 2013-08-05 09:40:00 Z
categories:
- twitter-bootstrap
- foundaton
- bourbon
- bourbon-neat
- css
layout: post
comments: true
external-url: 
---

For those who have not already read about [my dilemma][1] previously I've been
trying to come to a decision on a CSS framework going forward. Now here comes
the part where I make my decision. Ok so I won't make you wait...

...THE WINNER IS... Foundation 4.

Well mostly, here's what I found after spending time redesigning a landing page
with each and why I decided on Foundation 4 after all.

WARNING: Opinions ahead!

<!-- more -->

First some lessons learned

## If you use grid classes you'll have a bad time

Both Foundation and Bootstrap demonstrated this when they both revamped their
grid system in the latest versions requiring a tedious change of all the classes
and even the way they are used. Fortunately both decided to ensure this would
not be a problem in the future by providing mixins so that grid alignment can be
used with more "semantic" HTML.

So while I've been using grid classes happily for quite a while I can say that
I'm much, much happier using the mixins now. There is one important caveat
though.

While I have a reasonable amount of experience with CSS and SCSS now, enough to
use these mixins comfortably and tweak things when they don't layout exactly
right. I'd suggest that beginners (who rely more heavily on the framework to do
things) will be better served sticking with the grid classes for a while.
Consider them your training wheels. The mixins are a bit more effort so don't
apply them until you are good and ready.

## I love typography but don't want to spend the time setting it up

The main reason I didn't use Bourbon + Neat was because of it's total lack of
default typography, form styles and components. Look there are a lot of little
details that need to be worked out but largely (aside from font choices, sizes
and rhythm) this is the same for every project. No one should be coding this by
hand these days. Even if you think you should, I shouldn't. I don't have the
time and I'm not enough a CSS wizard yet. Also you're still wrong.

Both Foundation and Bootstrap have reasonable typographic defaults. Foundation
has chosen to move on to EMs (I wish they had bit the bullet and gone with REMs
and a pixel fall back though) which I think gives it an edge here but to each
their own.

One option I did consider was adding [Typeplate][2] for typography, but after
playing with it a bit I found it was still a bit immature and inflexible for my
liking. Still worth checking it out if you have not seen it yet I really think
it's a great concept and we're using it with Wordpress on the [WealthBar blog](http://blog.wealthbar.com).

## In the end it came down to responsiveness and the grid

While I generally think Bootstrap looks nicer, has better documentation
Foundation just works "better". After cleaning up my HTML to be more semantic
for Foundation I repeated the process with Bootstrap and found it
harder. In fact while I should have been able to re-use exactly the same
"semantic" HTML when switching from F4 to B3 it wouldn't work. For Bootstrap I
would need to add some more wrapper/container divs.

Bootstrap's grid is heavily dependant on structure. A row must be inside a
container (mixin or not) and a column must be inside a row or things are not
going to look right. Here is a pretty trivial comparison using a simple
header/banner but it still manages to illustrates the problem.

```html
<!-- Bootstrap Banner -->
<div class="container">
  <header class="banner">
    <div class="my-logo">
      Some content here.
    </div>
    <nav class="my-menu">
      Some other content here.
    </nav>
  </header>
</div>

<!-- Foundation Banner -->
<header class="banner">
  <div class="my-logo">
    Some content here.
  </div>
  <nav class="my-menu">
    Some other content here.
  </nav>
</header>
```

Now this may not seem like a big problem for you, and perhaps it isn't, but like
I said this is a *trivial* example and as you build more complex layouts one
quickly finds Foundation's mixins more flexible and amenable to the HTML
structure rather than having to bend your HTML to suit the grid structure.

The second advantage for Foundation 4 is their use of EMs which adds a bit more
responsiveness to the whole thing. I was honestly skeptical of EMs at first but
after working with them for a bit I can see their advantages now (still
hopefully F5 uses REMs for simplicities sake). The only downside is EMs make
setting things in [vertical rhythm][3] much harder. I'm working on some helper
mixins for computing baseline grids that I'll probably contribute to Foundation
5 once I'm happy with them.

Most of the reason I use a CSS framework is for the grid, responsiveness and
default typography. In each of these areas Foundation came out stronger. Form
styles, components, etc. can easily be built or copy-pasted from anywhere.

In fact one might argue that it is these things that make a CSS framework a
"framework" is their ability to allow you to "frame" things up easily not their
ability to provide a pretty progress bar.

## If you are prototyping and inexperienced Bootstrap may still be for you

All that said many of the advantages of Foundation go away for the beginner who
is still working with classes and is less comfortable customizing CSS. It isn't
surprising that most of the people who prefer Foundation are more experienced.
That in and of itself doesn't make Foundation better, just better for them.

I'd probably agree with anyone who says Bootstrap is still better for "finding
product-market fit". However assuming you're comfortable with the tools, either
will serve you well, but in my opinion that Foundation is perhaps a little
more future proof right now.

## What I'd **really** like to see

I really like what Bourbon and Neat do. Entirely mixin centric SCSS tooling is
appealing and the Neat grid mixins seem to be incredibly flexible. What it is
missing is some sort of "theme" where I can get a decent somewhat configurable
starting point for typography, forms and basic components.

So essentially I'm proposing a fork of Bootstrap built on top of Bourbon and
Neat. Perhaps with REMs. Perhaps it could be called "Highball" or "Old Fashioned" ;-).

[1]: /2013/06/14/bootstrap-foundation-bourbon-neat
[2]: http://typeplate.com/
[3]: /2012/10/08/getting-into-vertical-rhythm/
