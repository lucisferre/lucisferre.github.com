---
layout: post
title: "Why I'm not supporting old IE versions"
date: 2013-09-11 09:03
comments: true
external-url: 
categories: HTML5
---

I'm serious, not even IE9.

It seems odd that in a world where most have just accepted dropping IE6/7
support that I should be so ready to drop IE8 and IE9 in the dustbin too but
the fact is old IE versions are no longer relevant for the vast majority of the
web and I see little reason to support their outdated quirks on new projects.

<!-- more -->

Now compared with IE9 dropping IE8 support is easier to justify. It's old,
already has it's own [countdown site][countdown] and Windows XP (the only
reason you would be limited to using IE8) is reaching "end-of-life" next April.
Google is pulling support for it from apps, analytics and other products, other
companies have already dropped support, even jQuery no longer supports it as of
version 2.0. I suppose one *could* argue that because it is still clinging on to
about 10% market share we should keep supporting it for now, but that argument is
weakened by the fact that few people use IE8 by choice. This recent [article][1]
looked at the daily trends of browser usage and noted that both IE8 and IE9
usage drops sharply on the weekends.

These days older IE versions are found almost exclusively in corporate
environments where software upgrades are controlled centrally by IT departments
and as a result there can be a bit of a lag. So unless you are specifically
targeting corporate use you can bet that most people don't use your site/app on
IE8/9 or at least the don't *need to*. Moreover, because the EOL on XP is
coming up fast it is likely that even these last remaining holdouts will be
planning upgrades in the next 6-8 months and likely will end up using IE10 or
even IE11 at that time.

But wait! There's is even more good news. Thanks to Microsoft's adoption of a
silent (Chrome style) update policy we are witnessing, for the first time, an
immediate and sharp drop in IE9 use as IE10 takes over and will likely see it
again when IE11 drops. IE users are now being upgraded to new versions as they
are released (at least the ones who aren't forced to keep behind at work).
Which leads to my next point, which is if we don't need to support IE8 we don't
need to support IE9 anymore either.

![IE Browser Share](/images/browser-stats.png)

Source: <a href="http://gs.statcounter.com/#browser_version_partially_combined-na-monthly-201302-201308">StatCounter Global Stats - Browser Version (Partially Combined) Market Share</a></p>

Lets face it, the technical reasons for not supporting IE8 are obvious, modern
standards and features, modern frameworks (jQuery 2.0) less need for shim code
and less effort spent making things work or look right under the limitations of
outdated technology. What may be surprising though is that these same arguments
largely apply to IE9, a browser that was supposed to bring IE into a bright
HTML5 and modern standards future.

It's true though, IE9 is missing support many, now commonplace, modern web
APIs. The history API, CSS transitions and animations, input placeholders,
XMLHttpRequest2, Web Sockets, etc. All this adds up to a huge PITA if you want
to start using modern features that are commonplace on every single other
browser (including IE10/11).

But as I said before, we have left the era of IE browser relics. Sure there
will always be a few around, a few holdouts here and there. But for the vast
majority of IE users we can start assuming they have the latest and the
greatest. Isn't it wonderful?

[countdown]: http://theie8countdown.com/
[1]: http://bl.ocks.org/erwaller/6511564
