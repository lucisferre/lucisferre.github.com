---
layout: post
title: "Controlling the FUTURE with the History API"
date: 2012-12-20 09:00
comments: true
categories: [HTML5, UX]
---

Scott Barnes issued something of a challenge on Twitter yesterday

<blockquote class="twitter-tweet tw-align-center"><p>one day i'd like to use a website that actually uses the "forward" button with page sets.. like.. wtf do i scroll down to hit a fake button?</p>&mdash; Scott Barnes c[×┬õ]כ (@MossyBlog) <a href="https://twitter.com/MossyBlog/status/281190204818198528" data-datetime="2012-12-19T00:12:23+00:00">December 19, 2012</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script></p>

It turns out it is possible to do this using the History API, though arguably
it requires a bit of a hack. This History API was designed to allow web
application designers to add history to the browser so they could use the back
button to navigate backwards even when no actual browser navigation occurred
(loading new state with an AJAX request). Unfortunately it was never really
designed to allow you to navigate forward. Still, there is a way.

<!-- more -->

What you can do is inject the next page into the API using the
`history.pushState` function and then immediately go back in the history to the
current page. Leaving you with a navigation breadcrumb in the forward direction.

Then, when the user navigates forward you can detect the `popstate` event and
load the next page. How you load the next page is up to you. In the first
example below I've just done a quick `location.reload()` but since we're using
the History API it would be snappier to do something with AJAX to just replace
the HTML in the page (similar in principle to the [jQuery PJAX plugin][pjax]),
I'll get to that in a moment.

```javascript
$(function() {
  if (!history) return;
  current = location.href;
  next = $('.pagination a.prev').attr('href');
  history.pushState('pagination', null, next);
  history.go(-1);
  onPjaxPopstate = function(e) {
    if (location.href === current || !e.state || !e.state.pagination) return;
    $(window).unbind('popstate.page-turner');
    location.reload();
  }
  $(window).bind('popstate.page-turner', onPjaxPopstate);
});
```

So why is it feel kind of hackish? Well the History API doesn't really expose
anything that lets you just add entries to the history list so you  literally
have to navigate forward and back again. This causes a visible flicker in the
location bar as you insert the forward link.

With a little more time to think about it, I now have some idea how we might
fix this. We could push the current url onto the history stack instead so the
browser address doesn't change and include the next url in the state object
which gets passed through to the `popstate` event. Then we can use replace
state later to update it so the back button functions. This is a bit trickier
though because the History API is a bit unreliable across browsers in it's
behaviors. I've usurped a trick from an older revision of [jQuery-pjax][pjaxhelp]
to help out though. It's still a bit quirky though.

```javascript
$(function() {
  if (!history) return;
  current = location.href;
  next = $('.pagination a.prev').attr('href');
  history.replaceState({ page: current }, null, current);
  history.pushState({ page: next }, null, current);
  history.go(-1);
  onPjaxPopstate = function(e) {
    var state = e.state;
    if (!state || !state.page) return;
    if (!popped) {popped = true; return;}
    history.replaceState({ page: state.page }, null, state.page);
    location.reload(true);
  };
  // Used to detect superfluous popstate event fired by Chrome
  var popped = ('state' in window.history), initialURL = location.href
  $(window).bind('popstate', onPjaxPopstate)
});
```

The problem is need to detect Chrome's weird extra popstate call and discard
it, Firefox on the other hand doesn't do this. This demonstrates one of the
issues with using HTML5 features, the inconsistencies. You can, of course, try
libraries like [History.js][historyjs] which are designed to smooth out
inconsistencies and can act as a shim. I played with it a bit, but there is
still seems to be some unexpected behaviours and hacking required.

Now lets try to use AJAX instead of this crappy `reload` call. In this case we
can also keep all of the state in the current page which also helps us smooth
out the inconsistent History state behaviour. Here is the final result.

{% gist 4338688 page-turner.js %}

This is of course just a quick and dirty proof of concept. It's designed
specifically to work with the paging of the Octopress, however I think it
demonstrates the concept and it could easily be made into a simple jQuery
plugin similar to PJAX. If there is sufficient interest I might spend a bit
more time on it.

There are some definite downsides too. By hijacking the "future" or even the
"past" you may break a users expectations. For example if you click on a post
from the post listing, then go back, the forward button will no longer take you
back to the post it takes you to the next page.

This is similar to the problem most people have with Android's back button,
where sometimes it takes you to a different level of the app's hierarchy and
other times it takes you out of the app to the home screen or perhaps another
app. It's inconsistent. So whatever you do, it is important to use something
like this wisely.

I will say that it's works great with a Macbook's forward swipe gesture.

Want to try it out? Go [here](http://lucisferre.net) and start clicking
forward.

[pjax]: https://github.com/defunkt/jquery-pjax
[historyjs]: https://github.com/balupton/History.js/
[pjaxhelp]: http://stackoverflow.com/questions/6421769/popstate-on-pages-load-in-chrome
