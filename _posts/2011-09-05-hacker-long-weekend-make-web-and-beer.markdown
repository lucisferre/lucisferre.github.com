---
author: Chris Nicola
date: '2011-09-05 23:12:45'
layout: post
slug: hacker-long-weekend-make-web-and-beer
status: publish
title: 'Hacker long weekend: Make web and beer'
comments: true
wordpress_id: '651'
categories:
- NOT.NET
- beer
- rails
- ruby
- web
---

![include beer][1]

I spent the better part of this long weekend creating things I love.
Specifically, web and beer.

For the beer part I managed to make a total of 45 litres of beer in two
batches. One was a Sierra Nevada Pale Ale clone I did on Saturday that [Dan][3]
claims is nearly identical. The second was a Belgian Dubbel, that I will be
adding 4-5lbs of apricots from [Klippers Organics][4] which is also (my wife
and I also have a CSA from there which has been awesome). 

As for the web part, I started on a Ruby on Rails side project
to build something my wife and I have been talking about recently. I'm going to
leave the details for when we have things a little more functional, but for now
I really just wanted to blog about my experiences with Rails a bit-oh, and
lest anyone think I didn't get up to anything a normal person would consider
fun this weekend, I also managed to get out to a full-on pig roast my tenant
Jeff and his friends put on which was outstanding, slow cooked for what had to
be at least 8 or 9 hours!

<!--more-->

{% pullquote %}
Now this is my second Rails project, but it's arguably the first I've started
to put any serious effort into. The first was a simple toy [pomodoro app][5]
which is fun but not all that interesting. This time I had more of a plan. The
main goal was to use Rails for what it is good at, **not writing code**. Not in
the ASP.NET conference demo sense---_and I did it all without writing a single
line of code---_ but rather, in the, _leveraging one of the riches FOSS
ecosystems for web development today---_or as one pundit puts it, 
{" "The greatest web framework, that God gave man, on this earth." "}
 Seriously that's a direct quote from you-know-who. 
{% endpullquote %}

Seriously though, one goal I have is to use off-the-shelf Gems for anything not
core to the business functionality. That includes everything from styling to
authentication to javascript frameworks and helpers, etc. If I don't have to
write it, I've won another small victory.

In the time I had this weekend, I haven't made it too far, but I have a style I
like (I'm no designer so that took a while), a simple e-mail subscription "tell
me when you're open" form that actually works and a bunch of scaffolding. I
even managed to write 8 test cases. 

So far my impression is largely what I expected. I'm not going to go so far as
to say Rails is easy, because arguably it's not, there are a lot of things
going on behind the scenes and a lot to learn about Ruby, what I will say is
that it's **fun**. Ruby in particular is a very fun and expressive language,
but Rails is icing on that cake. You can just do so much, with just a little
bit of succinct clear code and in most cases it _just works(tm)_.

In a couple of what were essentially half-days I managed to: 

  1. Get a nicely styled site up and running with Compass, Blueprint and HTML5 boilerplate
  2. Deploy to production (several times actually)
  3. Get outgoing e-mail working with a subscription form
  4. Make the form "progressively enhance" and support browser with JS turned off

Though, as usual, it was not all roses and caviar... 

  1. _Going with the bleeding edge may cause bleeding_. I found out that
     grabbing the hottest pressed version of rails (3.1) and then running it on
     the beta stack of Heroku will have a couple of configuration [bumps][6]. I
     did manage to sort it myself, but what should have taken minutes had me
     lost for hours. Still my experience is these sorts of things are par for
     the course when configuring web deployments and it not the first and will
     not be the last time I waste a few hours fixing web service configurations
     (Azure, for example, is beginning to compete with my Warcraft `/played`
     for lost productivity).
  2. Compass, SASS, Blueprint, HTML5 boilerplate are pretty nifty tools for
     getting some ver boring HTML and CSS tedium out of the way, but they were
     not necessarily created to play nice with each other. You will likely have
     to fiddle around a bit getting all of these working cleanly in rails. In
     my case getting these Gems loaded the right way was what caused my
     problems with Heroku.
  3. Unobtrusive javascript does not feel all that aptly named. To be quite
     honest I find the idea of sending a templated javascript file back in
     response to an ajax request repulsive and will definitely be refactoring
     what I've did hastily and look into using something cleaner like backbone
     along with the new coffeescript asset pipeline support in Rails 3.1. I'm
     hoping I still still use the UJS helpers with this too.
  4. In my humble opinion IE9 is still a POS. The style looks fine in Chrome,
     Firefox _and even_ Safari but in IE9 one of the two fonts I used for the
     stylized logo fails to load properly. To be fair, it also has a wierd
     problem on my iPhone which I can't figure out, but at least the fonts
     still show up. Yeah, that's right, my **phone** can display the fonts
     correctly but IE9 can't. Pathetic.

Anyways, here they are, the [fruits of my labours][7], well the web part
anyways, for the beer part it'll be another 30 days. I'd really appreciate any
feedback on the style and design or anything else, especially tips about Rails.
Happy Labour Day everyone!

   [1]: /images/include_beer-300x199.jpg (include_beer)
   [2]: /images/include_beer.jpeg
   [3]: http://www.beermaking.ca/
   [4]: http://klippersorganics.com/
   [5]: http://tomatina.heroku.com
   [6]: http://stackoverflow.com/questions/7311000/unable-to-get-rails-3-1-compass-sass-blueprint-working-on-heroku-cedar
   [7]: http://pixelpublish.com

